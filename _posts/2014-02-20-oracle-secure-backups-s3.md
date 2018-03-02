---
layout: post
title:  "Setup Secure Oracle Cloud Backups to AWS S3 "
date:   2014-02-20 21:03:13
categories: blog
---
Oracle Recovery Manager or RMAN is tool used to take backups. Traditionally RMAN backups are written to disks and then moved over to tape devices. Here we will look at taking backups directly via RMAN to the cloud using Amazon's Simple Storage Service or S3 to store our RMAN backups.

So why AWS S3? There are a number of advantages of using S3 in place of tape backups. First they are significantly more reliable than using tapes. Second, restoring from S3 is faster and you do not need to wait for the tapes to arrive if you are storing them offsite. You can also login to your AWS console and check your S3 buckets containing the backups.

# Download Oracle Secure Backup Cloud Module

Download the Oracle Secure Backup Cloud Module for your specific environment from [here](http://www.oracle.com/technetwork/database/database-technologies/secure-backup/downloads/index.html).
You will need an active OTN account for this.  Next, extract this to a staging area for installation.

Create a wallet directory

The Secure Backup Module will need to know the location of your Oracle Wallet directory. If you have one, note the location else create a new wallet directory in a secure location and make a note.

	$ cd $ORACLE_HOME/dbs
	$ mkdir osbws_wallet 
	/s03/oracle/PROD/11.2.0/dbs/osbws_wallet 

# Install Secure Backup Module

Next run the OSB java installer installing the module. You will need to supply your OTN username, OTN password, AWS Security ID, AWS Secret Key and the location of your wallet as mandatory parameters for theinstaller to work.

	$ java -jar osbws_install.jar -AWSID <Your AWS Secuirty ID> -AWSKey <Your AWS Secret Key> -otnUser <OTN username> -otnPass <OTN Pasword> -walletDir /s03/oracle/PROD/11.2.0/dbs/osbws_wallet

If you don't want to type the long commands or expose the sensitive information, you can use a simple installation script from my public repositery [here](http://github.com/samx18/osbcm_install)


	$ ./osbcm_install.sh
	Oracle Secure Backup Cloud Module Installation Script
	Enter your AWS Security ID:
	Enter your AWS Secret Key:
	Enter your OTN User ID:
	Enter your OTN Password:
	Enter your Oracle Wallet location:
	Oracle Secure Backup Database Web-Service Install Tool, build 2013-10-30.0001
	AWS credentials are valid.
	Creating new registration for this S3 user.
	Created new log bucket.
	Registration ID: eda9da5e-e197-45cf-8cde-1ddc08fe8c25
	S3 Logging Bucket: oracle-log-ctrcloud-1
	Validating log bucket location ...
	Validating license file ...
	Create credential oracle.security.client.connect_string1
	OSB web-services wallet created in directory /s03/oracle/PROD/11.2.0/dbs/osbws_wallet.
	OSB web-services initialization file /s03/oracle/PROD/11.2.0/dbs/osbwsPROD.ora created.



Among other things the installer will also logon to your AWS account and create the S3 buckets that will ultimately hold your RMAN backup sets


# Enable Archive logs

If your database is not already on archive log mode, You should enable it now.

Check archive log status

	SQL> archive log list
	Database log mode          No Archive Mode
	Automatic archival         Disabled
	Archive destination        /s03/oracle/PROD/data
	Oldest online log sequence     1
	Current log sequence           1

Shutdown your instance

	SQL> shutdown immediate
	Database closed.
	Database dismounted.
	ORACLE instance shut down.

Startup your database in mount state, enable archive logging and open your database

	SQL> startup mount
	ORACLE instance started.

	Total System Global Area 2137886720 bytes
	Fixed Size          2230072 bytes
	Variable Size         452987080 bytes
	Database Buffers     1660944384 bytes
	Redo Buffers           21725184 bytes
	Database mounted.
	SQL> alter database archivelog;

	Database altered.

	SQL> alter database open;

	Database altered.

	SQL> archive log list
	Database log mode          Archive Mode
	Automatic archival         Enabled
	Archive destination        /s03/oracle/PROD/data
	Oldest online log sequence     1
	Next log sequence to archive   1
	Current log sequence           1
	SQL>

# Open RMAN and connect to the target DB

Launch RMAN and connect it to your target DB. Verify if you have the correct recovery catalog attached or the control file by default.

	RMAN> connect target /
	connected to target database: PROD (DBID=250755512)
	using target database control file instead of recovery catalog

# Setup a RMAN configuration for SBT type device

To use the Oracle Secure Backup Cloud Module you need to set up an RMAN configuration for the SBT type device.

	RMAN> configure channel device type SBT parms 'SBT_LIBRARY=/s03/oracle/PROD/11.2.0/lib/libosbws11.so ENV=(OSB_WS_PFILE=/s03/oracle/PROD/11.2.0/dbs/osbwsPROD.ora)';

	new RMAN configuration parameters:
	CONFIGURE CHANNEL DEVICE TYPE 'SBT_TAPE' PARMS  'SBT_LIBRARY=/s03/oracle/PROD/11.2.0/lib/libosbws11.so ENV=(OSB_WS_PFILE=/s03/oracle/PROD/11.2.0/dbs/osbwsPROD.ora)';
	new RMAN configuration parameters are successfully stored

# Backup tablespace

Lastly test your configuration by backing up a tablespace

	RMAN> backup device type sbt tag 'DEMOPROD_SYSAUX021214' tablespace SYSAUX;

	Starting backup at 12-FEB-14
	using channel ORA_SBT_TAPE_1
	channel ORA_SBT_TAPE_1: starting full datafile backup set
	channel ORA_SBT_TAPE_1: specifying datafile(s) in backup set
	input datafile file number=00011 name=/s03/oracle/PROD/data/sysaux01.dbf
	input datafile file number=00018 name=/s03/oracle/PROD/data/sysaux02.dbf
	channel ORA_SBT_TAPE_1: starting piece 1 at 12-FEB-14
	channel ORA_SBT_TAPE_1: finished piece 1 at 12-FEB-14
	piece handle=0cp0f5r2_1_1 tag=DEMOPROD_SYSAUX021214 comment=API Version 2.0,MMS Version 2.0.0.0
	channel ORA_SBT_TAPE_1: backup set complete, elapsed time: 00:00:45
	Finished backup at 12-FEB-14

# Encryption

It is highly recommended that you encrypt your backups, specially when storing them over the cloud. By default RMAN & TDE support 128 bit encryption. You can make them more secure by moving to a 256 bit encryption.

	RMAN> show encryption algorithm;

	RMAN configuration parameters for database with db_unique_name PROD are:
	CONFIGURE ENCRYPTION ALGORITHM 'AES128'; # default

	RMAN> configure encryption algorithm 'AES256';

	new RMAN configuration parameters:
	CONFIGURE ENCRYPTION ALGORITHM 'AES256';
	new RMAN configuration parameters are successfully stored

	RMAN> show encryption algorithm;

	RMAN configuration parameters for database with db_unique_name PROD are:
	CONFIGURE ENCRYPTION ALGORITHM 'AES256';

	RMAN> set encryption on


# Verify

Lastly verify your backup sets by logging in your AWS Management console and going to the S3 section. If all went well, you should be able to see your newly created S3 buckets and the backup sets.

That's it. You can now backup your Oracle DB directly to the cloud.

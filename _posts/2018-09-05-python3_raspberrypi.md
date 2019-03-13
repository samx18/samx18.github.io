---
layout: post
title:  "Installing the latest Python 3 on Raspberry Pi"
date:   2018-09-05 09:03:13
categories: blog
---

Over the weekend, working on a personal Python project on the Raspberry Pi, I decided to get the latest version of the Python 3 branch.

The default OS of the Raspberry Pi comes with Python 3, but it is usually a couple of releases behind the latest stable version. At the time of writing this post, the latest stable version of the Python 3 branch is 3.7 and the Rapsbain version is 3.5.

Since the Raspbian repositories are updated, you will need to download and install the latest version manually. Below are the steps to get it working   

## Download  and extract 
Download and extract the latest version of Python 3 as root under as below

    pi@ai-pi:~ $ sudo su
    root@ai-pi:/home/pi# cd /usr/src
    root@ai-pi:/usr/src# wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
    root@ai-pi:/usr/src# ar -xf Python-3.7.0.tgz
    
## Install Dependencies
Since you are doing a manual install, you will need to install the dependencies manually

    apt-get update
    apt-get install -y build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libffi-dev
    

## Configure and Install Python 3 
Run configure with optimizations and make install from the source

    root@ai-pi:/usr/src# cd Python-3.7.0
    root@ai-pi:/usr/src/Python-3.7.0#./configure --enable-optimizations
    root@ai-pi:/usr/src/Python-3.7.0#make altinstall

## Update the links
Update the link to the newly installed version of Python. Since I wanted to leave the default python 2, I only updated the python 3 link

	root@ai-pi:/usr/src/Python-3.7.0# ln -s /usr/local/bin/python3.7 /usr/local/bin/python3

## Verify
Verify the python 3 version

	root@ai-pi:/usr/src/Python-3.7.0# python3 --version
	Python 3.7.0
	
## Clean up
Optionally clean up the src

	root@ai-pi:/usr/src# rm -Rf Python-3.7.0
	root@ai-pi:/usr/src# rm Python-3.7.0.tgz
	
You can revert back to the default python 3 version anytime by simply updating the link created above. Also keep in mind updating raspbian will likely break this and you may have to recreate the links if needed.


---
layout: post
title:  "Using R with Spark on a Hadoop Cluster: SparkR"
date:   2015-08-20 09:03:13
categories: blog
---
The release of Apache Spark 1.4 brought some much awaited and exciting news to R users working with big data. Along with Scala and Python, spark now allows for R to interact with the spark core API through RDDs using the SparkR package.

SparkR initially started off as an independent project at [AMPLab](https://amplab.cs.berkeley.edu/) Berkeley, however the project got merged with the Apache Spark project in April 2015 and now officially ships with the Apache Spark adistribution 1.4 onwards.

# Upgrade Hadoop to use Spark 1.4

In order to get sparkR, you will need to first upgrade (or install) the spark distribution on your Hadoop to 1.4 or later. As of writing this post, the latest version is 1.4.1. You can start by downloading a prebuilt version for your appropriate Hadoop distribution and unpacking. In my case

	root@xxxxx:~# wget http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1-bin-hadoop2.4.tgz
	root@xxxxx:~# tar -xvf spark-1.4.1-bin-hadoop2.4.tgz
	root@xxxxx:~# mv spark-1.4.1-bin-hadoop2.4 /srv/
	
If you are upgrading, also update any links associated with your old spark distribution

	root@xxxxx:~# ln -s /srv/spark-1.4.1-bin-hadoop2.4 /srv/spark

Next you may have to redo your environment specific config changes if any. In my case the only one I cared for now was to make the logging less verbose. We can edit the log4j.properties to change *log4j.rootCategory=INFO, console*  to  *log4j.rootCategory=WARN, console* 


	hadoop@xxxxx:~$ cd /srv/spark/conf
	hadoop@xxxx:/srv/spark/conf$cp log4j.properties.template log4j.properties


That's it you are done. Log into the the sparkR shell

	hadoop@xxxxx:/srv$ cd spark
	hadoop@xxxxx:/srv/spark$ bin/sparkR
	.
	.
 	Welcome to SparkR!
 	Spark context is available as sc, SQL context is available as sqlContext
	> 

The Spark context is available as sc, SQL context is available as sqlContext. If you are working from the SparkR shell, the SQLContext and SparkContext should already be created for you.If not, you can create them manually

	sc <- sparkR.init()
	sqlContext <- sparkRSQL.init(sc)

# SparkR Data Frames
Let's start by creating a simple local R data frame.

	>localDF<-data.frame(name=c("Ana","Anna","Sarah","Tina","Tom"),age=c(11,12,8,10,12))
	> localDF
	   name age
	1   Ana  11
	2  Anna  12
	3 Sarah   8
	4  Tina  10
	5   Tom  12

Next, convert the local R data frame to a spark data frame - SparkR DataFrames support a number of functions to do structured data processing. Few examples

	> sDF<-createDataFrame(sqlContext,localDF)
 

	> head(select(sDF, "name"))
	   name
	1   Ana
	2  Anna
	3 Sarah
	4  Tina
	5   Tom

	> head(filter(sDF, sDF$age > 10))
	  name age
	1  Ana  11
	2 Anna  12
	3  Tom  12

In addition SparkR data frames support a number of commonly used functions to aggregate data after grouping.
This time let's use the faithful example local data frame 'faithful' that comes with the spark distribution. First we will convert it to a spark DF using sqlContext

	faithDF <- createDataFrame(sqlContext, faithful)
	> head(faithDF)
	  eruptions waiting
	1     3.600      79
	2     1.800      54
	3     3.333      74
	4     2.283      62
	5     4.533      85
	6     2.883      55

	> head(summarize(groupBy(faithDF, faithDF$waiting), count = n(faithDF$waiting)))
	  waiting count                                                                 
	1      81    13
	2      60     6
	3      68     1
	4      93     2
	5      80     8
	6      47     4

# Setting up RStudio with SparkR

RStudio, the defacto IDE for for R is also pretty easy to setup to work with SparkR, both for the desktop and the server versions. The steps below are for RStudio server, but the desktop one shouldn't be much different.

Set a environment variable inside RStudio to point to your Spark home and the library paths for Spark. Next just load SparkR as you any other R package

	> Sys.setenv(SPARK_HOME="/srv/spark")
	> .libPaths(c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib"), .libPaths()))
	> library(SparkR)

	Attaching package: ‘SparkR’
	
Before you can using SparkR from Rstudio, you will need to create the spark context and initialize the sqlContext manually. Remember is created automatically only when you use sparkR from the console. 

	> sc<-sparkR.init()
	Launching java with spark-submit command /srv/spark/bin/spark-submit   sparkr-shell /tmp/RtmpucYqcU/backend_port5e966f6f9d84 
	...
	...
	...

	> sqlContext<-sparkRSQL.init(sc)
	>
	
That's it, you can now run your R jobs using sparkR from within RStudio.

You can also view the status of your R jobs on spark from the standard spark UI. Just point your browser to http://myawesomeserver:4040

![Spark](/img/sparkJob.png)

To stop the Spark context

	>sparkR.stop()
	>
This will also terminate the connected backend R session.

That's about it to get you started with exploring your data at scale using R on Apache Spark. 

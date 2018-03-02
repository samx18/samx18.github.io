---
layout: post
title:  "Big Data Insights with Tableau + Apache Spark - The Aadhaar Story"
date:   2015-10-16 11:03:13
categories: blog
---

{% include addViz.html %}

##What is Aadhaar?

The Unique Identification Authority of India (UIDAI) is a central government agency of India.Its objective is to collect the biometric and demographic data of residents, store them in a centralized database, and issue a 12-digit unique identity number called Aadhaar to each resident.

##About the Dataset

Aadhaar data catalog is made publicly available by the UIDAI to help with  research and building application on the data which is collected at national level. The raw datasets (to-date) are available in via API. Past 30 days data is also available directly for the UIDAI's website.

##The Technology 

- Python scripts were used to directly pull and concatenate the data from UIDAI datastore using APIs provided.
- The data was mined in a raw format into HDFS system again via Python scripts which were then loaded into Apache Hive. 
- Due to it's sheer size and volume (exceed 80 GB in raw format) , A 5 node Spark cluster was setup on AWS to aggregate and summarize the data.
- Ad-Hoc analysis was done by directly connecting Tableau to the Spark Cluster. 
- Finally the aggregated data was connected Tableau for visual analytics.


##Data Insights 

- The northern state of Uttar Pradesh leading the number of Aadhaar generated followed by Maharashtra, West Bengal and Bihar. Though these are not necessarily the largest states by area, they are definitely the largest by population. 
- The age group of 20-35 has the highest number of Aadhaar generation with 30 yr olds making up the largest chunk of Aadhaar holders across the country.
- The gender composition among Aadhaar is mostly even which is nice. Transgenders make a relatively negligible minority which can probably be attributed the still evolving social economic society.

##Some interesting Insights

- The newly created state of Telangana (in 2014) has the maximum number of Aadhaar holders among infants 1 yr old and under.
- Females have surpassed Males in the two southern states of Tamil Nadu and Kerala.
- The Aadhaar generation practically stopped during 2013 in the island state of Andaman & Nicobar. 
- Similarly the eastern states of Mizoram and Arunachal Pradesh practically did not start the Aadhaar until 2013

##Additional Insights
Within the UIDAI datasets are additional about mobile phones and email ids that citizens have volountrly  included. A quick visualization that tell you about the mobile and internet penetration across India. 

- The top three states with mobile penetration are Uttar Pradesh,Maharashtra and Tamil Nadu.
- The top three states with internet penetration are Bihar,Maharashtra and Gujrat.
Maharashtra is leading in both mobile and internet usage and in Bihar (the only state) internet usage among females is significantly higher than among males.
- The island state of Lakshawadeep has the lowest mobile and internet usage




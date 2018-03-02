---
layout: post
title:  "Agile Disaster Recovery Strategy Using The Cloud"
date:   2012-02-29 09:03:13
categories: blog
---

Traditionally disaster recovery (DR) solutions have been both expensive and often time consuming to roll-out.The solution often tends to carry around mass which makes it difficult and costly to both to implement initially and to adapt to rapidly technology or economic environments.
An agile DR solution that is built around the cloud aims to address these very problems by focusing on being lean and flexible

# The Agile (Lean) approach

The most important part of any DR solution is to achieve business continuity in case of disaster resulting in a system downtime and reduce any recovery times. This involves securing your business critical data and ensuring its availability as well as restoring critical business and operational processes. As such instead of going in for a big bang approach for a DR solution where you aim for a 100% fail over it might make business sense to approach this in an agile and iterative manner.

Let’s break this down


## Business Critical Data

The core concept behind any DR solution is sustainability in case of a disaster and fall back on the primary site as soon as possible, with that in mind do you really want to include your complete data set as part of the DR solution? Probably not.Have a strategy for your different data sets including your master data, your transactional data and your archived data. Depending on the complexity and the IT landscape this data might be distributed or reside with the same database.The idea here is to identify your business critical data that will enable your business to operate. This needs to part of your DR strategy first.

## A Product Backlog of Your Business & Operational Process

As with data, it is equally important to identify the critical business and operational processes that your enterprise will need to be able to operate. This will need to go hand in hand while you identify your critical data sets and system. The systems and data sets that will eventually be part of your DR solution scope will be the ones that are needed to support these critical processes. Think of these process or capabilities as your product backlog. What is critical gets higher priority than others, for instance while your order processing system will be need to part of your DR solution do you really need the systems to support internal procurement process? well, you may or may not depending on your business.

## Break it Down

Technically, smaller wins will increase your probability of success as compared to the big bang approach. For instance if you are aiming for a full fledged DR capability at a remote site in a different geography, you may want to start first by just implementing a remote backup solution and then build upon that to go in for a complete standby. Be cautious here, you do not want to overdo this and decompose your goals too much or too often. I am not a big fan of thumb rules, but I will do an exception here. Break it down to a delivery item that adds value.

##Test Often but Test Agile

Test and validate your DR procedures regularly. You do want to wait till an actual disaster happens to test the effectiveness of your DR solution. At the same time it important that you have a iterative plan that has a DR validation built in periodically. Dont disrupt your existing business process or system to validate or test DR.

# Leveraging The Cloud

Building your DR solution over the cloud can help cut the mass and add agility for around the DR solution. It also gives you the flexibility around the actual solution itself, for instance you may opt for a active(warm), always-on DR solution or may go in just for an on-demand passive solution.
Critical factors that influence a lean DR solution built around the cloud could include

## The Time Factor
The essence of having an agile approach to DR is the ability to roll-out the DR solution quickly. You don’t need to invest time and money to setup the upfront massive infrastructure to get going. Starting off using a PaaS or a SaaS model which would host and manage your DR site and solution might work just as well.

## The Cost Factor

One immediate advantage of implementing a DR solution over the cloud is the cost. A DR solution over the cloud which leverages an on-demand model would be more cost effective to roll-out as opposed  to a traditional in house or a even a hosted solution. It is not just the reduction in costs that is a factor here, but also value add that a cloud based business resilience solution brings in by leveraging technologies like server virtualization and rapid data replication to reduce recovery times.

## The Change Factor

DR solutions build on and leveraging the cloud have the ability to pivot quickly.These solutions can be scaled up or down rapidly in response to changing business dimensions. Also since there is seldom any upfront investment on technology, the dependency on the same is also significantly reduced.This allows the solution to be both lean and flexible.

## Solution Robustness

You can quickly and cost effectively build a DR solution that involves Multiple DR Sites via the cloud. With on-demand instance provisioning you can add robustness to your DR solution by having your instances spread across geographic regions. Under a traditional DR solution this was part of the trade-off that organizations had to make due to the cost and time involved.

## A Managed Service Model
Sometimes it might make sense to have a managed model for your DR solution.Under a cloud based managed model, you let the provider manage the complete disaster recover solution for you, including the initial setup and the ongoing validation of your DR process.

As always we are all ears on listening to your experiences on implementing DR solutions over the cloud. Jump in.

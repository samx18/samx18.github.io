---
layout: post
title:  "Deploy a rails app locally on OS X using Passenger with Nginx in under 5 minutes."
date:   2014-06-1 18:03:13
categories: blog
---

**The Why**

I honestly did not plan to write a post on this topic, mainly due to the reason that this configuration of deploying a rails application on Nginx using Passenger is fairly common and widely used, perhaps maybe even the most popular rails app deployment options out there. It however turns out that most of the documents and instructions out there were for the linux, which does make a bit of sense since production deployments are almost always on a linux environment. As for me however, I wanted to give this configuration a shot locally before pushing this configuration to a production ready linux environment.

My primary local development environment is OS X Mavericks and the rest of this post covers setting up Passenger with Nginx on OS X, so if you have a different local development environment you should probably stop here.

**The What**

Here is the what portion of the configuration, what as in what components are there and what role do they play.

Nginx is a fast, lightweight and open source web server designed out of the box to serve static webpages like Apache. While Apache is more popular, Nginx is faster and smaller and does it’s job very well when coupled with a powerful application server.

Passenger from Phusion is an application server. What makes Passenger very different from all the other ones is that Phusion Passenger integrates directly into Apache or Nginx. The fact that it’s completely written in C++ makes it very fast as well compared to the other application servers. In this configuration passenger acts the powerful application server behind Nginx to server dynamic content from your Rails application. Nginx also acts as a reverse proxy between the user requests and the application server to make sure the request are distributed correctly. The best part is that you don’t need to worry about setting this up, the integration takes care of this for you.

**The How**

Before proceeding, this note makes an assumption that you already have the following components installed and configured on your local OS X environment.

Ruby 2 (2.1.2)

Rails 4

RVM

The how is simple as it should be with just 3 steps

Step 1 — Install passenger.Since you already have ruby installed and setup you can do this by directly installing the passenger gem.


	$ gem install passenger

Step 2 — Next install Nginx. There are many ways to do this including installing Nginx directly using an OS X package manager like homebrew (and that’s how I did it initially). However, to get Nginx working with Passenger, its source must be compiled with the necessary modules. Agin it’s not as scary as it sounds and it’s as simple as running a simple command.

	$ rvmsudo passenger-install-nginx-module

Just remember however to use ‘rvmsudo’ else the installation will fail complaining permission issues. You will get a couple of simple prompts, answer them and you will be up and running in no time.

Step 3 — Finally before you start passenger and nginx, we will need to make a couple of configuration changes and copy our Rails app to the document root.

There a few different ways to do it, here the simplest. Create the following directory structure

	$ mkdir /var/www
	cd /var/www

Copy your Rails app to this location

	cp -R /Users/samx18/Workspace/Rails/myawesomeapp .
now Edit the nginx conf file under the following location

	$ sudo subl /opt/nginx/conf/nginx.conf
Search for the ‘server’ section and comment out the the following

	#location / {
 		#root html;
 		#index index.html index.htm;
 	#}

Then add the following just under the section you just commented out.

	passenger_enabled on;
	root /private/var/www/myawesomeapp/public;
Pay attention to the ‘/private’ appended in front of the path. Start passenger now

	$ passenger start
And that’s all to it, your rails app running locally on Nginx+Passenger setup.

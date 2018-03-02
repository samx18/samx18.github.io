---
layout: post
title:  "Setup a complete local development environment on your ARM Chromebook"
date:   2014-01-14 21:03:13
categories: blog
---
Chromebooks are often perceived as dumb machine that are good enough just for browsing or at max run your cloud based software. The way I look at it, anything that runs on UNIX can never be that dumb and always has room for some tinkering around. That’s exactly what I did when I was looking around for a secondary development environment.

Before we move further, I should state what I was trying to accomplish :-
I did not want to dual-boot to a different OS. Since my primary focus was going to be web apps, I wanted the chrome OS and my development environment to co-exist.This would allow me to develop on Linux and test the UI on chrome. I also wanted a way to sync my work to the cloud and be available on different machines. Since the Chromebook was not going to be my primary machine, this was important for me. My last requirement from this setup was, I wanted a the system to be fully functional even without a active internet connection. I must also clarify that I was not looking for cloud based development or deployment option. There are quite a few solutions for that which work really well. As for the hardware, I used the Samsung ARM based Chromebook for this test. Your results may vary slightly if you have a different model.

As an overview, here’s what we will be doing

* Enable developer mode on the Chromebook
* Setup chroot via cruton
* Install a fully functional Linux distribution (I used Debian for this)
* Install additional development components LAMP, Ruby & ROR
* Set up a versioning system with git and github
* Install a cloud based code editor
* Setup a working directory
* Setup a CLI Google drive client — Grive

# Enable developer mode on the Chromebook  

First things first. You can’t do much with your Chromebook without first enabling development mode. Pressing ctrl+alt+t will take you the crosh environment, but that’s it. The list of commands available are limited. To fully unlock your Chromebook’s potential you will have to enable developer mode. Luckily its straightforward. (Enabling developer mode will wipe off all your local data from your Chromebook.)

**Step 1** — Press and hold the Esc and Refresh keys together, then press the Power button.This will boot your Chromebook in recovery mode.

**Step 2** — While on the recovery screen (the screen with the orange exclamation mark), Press ctrl+d to boot your Chromebook in the developer mode. This will wipe out your local data and will take around 15 minutes to boot. This is one time. After this each time you reboot your system, remember to press ctrl+d to boot up in developer mode. Pressing the spacebar will boot the system in normal mode and wipe out your developer environment.

**Step 3** — After you boot in the developer mode, enter the command prompt by pressing ctrl+atl+t. You can now type ‘shell’ to enter the bash prompt.

	crosh> shell
	chronos@localhost / $ bash
	chronos@localhost / $

# Setup cruton,chroot and debian CLI 

The bash shell in itself is powerful, but to be able to install your favorite components easily you will need a Linux environment like Debian. Again a straightforward process

**Step 1** — Just open a different tab on your Chromebook and download theCrouton installer to your ‘Downloads’ directory.

**Step 2 ** — Open a new terminal tab (ctrl+alt+t) and switch to the shell and install Debian

	crosh> shell
	chronos@localhost / $ sudo sh -e ~/Downloads/crouton -r wheezyextra -n debian -t cli-

Just follow the prompts and that’s all to it. You can also choose the GUI option via crouton if you want.

**Step 3** — Login and update your system

	crosh> shell
	chronos@localhost / $ sudo enter-chroot
	Entering /usr/local/chroots/debian…
	(debian)sam@localhost:~$ sudo apt-get -f install

To enter the Debian (Linux) environment again just to a ‘sudo enter-chroot’ from the shell.

#Setup your local development stack

In my case this included. Apache, MySQL,PHP,Python,Ruby, ROR and node.js.

**Step 1** — The simplest way that worked for me was to get the LAMP (Apache,MySQL &PHP) stack bundled from a debian repository. You can also install the LAMP components individually if you choose so.

	(debian)sam@localhost:~$ sudo apt-get install mysql-server mysql-client phpmyadmin

**Step 2** — Setup ruby and ROR. There are various options here, but my favorite is using RVM (Ruby Version Manager) .

Getting a stable rvm

	(debian)sam@localhost:~$ curl -L https://get.rvm.io | bash -s
	(debian)sam@localhost:~$ rvm get stable
	(debian)sam@localhost:~$ rvm requirements

At this stage rvm should install any ruby related dependencies. Next install ruby using RVM

	(debian)sam@localhost:~$ rvm install 2.0.0 —with-openssl-dir=$HOME/.rvm/usr

The openssl is optional, but I suspect if your working with web application, you will need it. Before installing rails, it’s a good idea to update your gems.

	(debian)sam@localhost:~$ gem update —system 2.1.9
	(debian)sam@localhost:~$ gem install rails —version 4.0.2

The last command will install rails 4. With this a basic development is now setup locally on your Chromebook.

# Setup versioning with git & githib 

Now is a good time to setup a versioning system of your environment. git is a cross platform versioning system and chances are, you are already using it. You can setup git either directly from the source or from the repository. I will stick with the later. 
Before that ensure you have the necessary dependencies installed.

	(debian)sam@localhost:~$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
Install git directly from a repository

	(debian)sam@localhost:~$ apt-get install git

Once you have git setup locally you can start pushing your code to github.
Setup a working directory — At the core of this system is a working directory that is accessible easily from both the chrome OS and the Debian CLI. This will allow you to
 a) work with your files using the chrome GUI. b) Test your application real time from chorme.
 
To do this simply create your working directory under the default ‘Downloads’ directory of the chrome OS. Your new directory will now available both from within the file viewer and other apps that you install and also from the Debian CLI. It is probably a good idea to configure your apache web root directory under this as well.

	(debian)sam@localhost:~/Downloads$ pwd
	/home/sam/Downloads
	(debian)sam@localhost:~/Downloads$
	(debian)sam@localhost:~/Downloads$ mkdir <yourworkingdirectory>

# Setup backup and sync with Google Drive CLI

The Chromebook is probably not going to be your primary development machine. Even if it is, it would be best to have your data nicely synced up to the cloud. Enter Google drive. Unfortunately the native Google drive app for chrome OS does not work well outside the chrome OS. Enter Grive. Grive is a open source implementation of Google Drive client for GNU/Linux. It uses the Google Document List API to talk to the servers in Google.

Installing Grive can be a little bit more challenging than the above components. You start by cloning the source from github, building and then installing. Based on the development libraries you have on your system so far, you may have to download and install additional dependencies for Grive.

**Step 1** — Get the source code. Clone the Grive srource from github

	git clone git://github.com/Grive/grive.git

This is create a grive sub directory under the main source code directory you just downloaded.

**Step 2** — Build the grive executable

	cd <yoursourcecodedirectory>
	cd ./grive
	cmake .

At some point after you have resolved all the errors, you will get a 100% completion message and your grive executable should be built.

**Step 3** — Configure with your Google drive account.

Copy the ‘grive’ executable into the folder you want to sync. In our case this will be our working directory under the ‘Downloads’ folder.
Before you proceed further, BACKUP your Google drive data. Grive does not sync any of your Google drive documents. But still its a good idea to backup now.

Running ‘grive’ for the first time

	(debian)sam@localhost:~/Downloads/GD$ ./grive -a

This will generate a url which you will need to copy and paste to your browser (you will need to be logged into your google account). Post back the generated code.For the next subsequent runs just use

	(debian)sam@localhost:~/Downloads/GD$ ./grive

You can also schedule this in your crontab. Remember again, Grive does not sync any of your native Google documents like .doc files in your Google drive. This limitation however, works well with our setup.
# Setup your code editor 

This is a easy one. Download the caret app from the Google store. Caret is a lightweight, but deceptively powerful text editor based on the open source ace editor. You can open single files and complete projects as folders. It also supports syntax highlighting and code wrapping.

And, that’s it. You are ready to rock.

Of-course, this isn't a perfect development solution. One major drawback is you have to work with the limited processing power and memory of an ARM based machine. But this is the best local development environment you could possibly build for under $250. The SSD on the Samsung Chromebook really does speed things up.Obviously if you have the Chromebook pixel at your disposal, your results will be even better.

---
layout: post
title: Docker basics
date: 2024-08-18 22:27 +0200
---

## Introduction

So you want to learn Docker? Great! Here I will give you the basic that you need
to know to get started with Docker.

### What is Docker?

Docker is an tool that helps you to deploy, run or share you application with 
others in a very easy way by package your application in something that is called
images, these images can later be used to run anywhere by creating containers
from these images. If you don't know what an container or an image is don't
worry, I will tell you what each mean later in this article.

### Why use Docker?

In history when developers has been developing applications there is one problem
that often has occured, maybe you have been a victim of it? It is so common it
has become a meme and a phrase called "It Works On My Machine" and this is when
the developer has built something that works great on his/her work station but
once it hit production, is run by another developer or is sent to a customer it
doesn't work anymore. \
Why this happend could be because of different reasons 
but some of the common reasons was that there was some tools on the 
developers workstation that was not installed on client machine or there was
configurtion setup that the client was missing. This is what Docker is built to resolve.

### How did Docker solve this problem?

Docker came with an solution to prevent this problem from happening by package
the application with the required system 

## Containers and it's history

Time for a history lesson, the concept of containers are not a new thing and has 
it's origin all the way back since 1979, Yes it is that old! Let's go through 
the history book and go over some of the more noticable years of container 
history to see how containers has evoled to what it is today.

### 1979: chroot

chroot or Unix V7 was the first solution to isolate application in a filesystem
by changing it's and it's childrens root directory to another location we could
isolate what each service had access to making it only to being able to access
what it needed to access.

### 2000: Jails

In the 2000's FreeBSD added support for something called Jails that made it
possible for administrators to create smaller sub systems called "Jails" by 
setting up partitions and there own IP for each Jail so it only could assess the 
resources and services it needed. This was a big security benefit over chroot.

### 2001: Linux VServer

One year later Linux came up with it's own solution like "Jails" where they could
setup applications own Filesystem, Network configuration, Memory and so on. This
was done by patchin the Linux Kernel.

### 2006: Control Groups

Google released a new way to isolate applikations that was first called "Process
Containers" in 2006 but was renamed to Control Groups in 2007. What they did
was to bring forward a solution that made it possible to isolate resources like
Disk I/O, CPU, Memory and Network with the possiblity to also limit each 
resources.

### 2008: LXC

LXC was introduced in 2008 and is still used today by some softwares like
Proxmox to run containers connected to the Linux kernel without having to 
isolate parts of the host system and could instead run directly on a single
Linx kernel. This was done by using cgroups and Linux Namespaces.

### 2013: Docker

And then came Docker in 2013 with the first solution of the container manager,
in the begging it was build on top of LXC but has over the years migrated to an
own library called libcontainer.

Docker did such a good job to make it easy to maintain containers that it 
exploded in popularity and is now one of the biggest in it's field and one of 
the most common container solution out there.

## Images and Containers

The two main features that Docker evolves around is images and containers, an 
image is a package of the run environment that applikation is built with this
can be build tools or environment variables and so on that is needed to make
the applikation run as expected. An image is build from what is called an 
Dockefile where the developers specify in code what is required during runtime
and because the developer is/should run the same Dockefile (with some exception 
that I will bring forward later) when doing development it made sure that the
client that was receving the image would have the same tools needed to run the 
image.

Once the developer has a image build from a Dockerfile they can then create a 
container from that image and run it with the same configuration and and tools 
that the developer had when creating it. So you could basically say thet the 
image is the recipe and the container is the product created from said recipe.

Docker has many more features than Images and Container but these two are the 
core features if the project.

## Get started

The most common and easiest way to get started with Docker is to install 
Docker Desktop that can be found on the [Docker website](https://www.docker.com/products/docker-desktop/) below I will guide you through how to setup Docker 
Desktop on your machine.

Each OS is different, because Docker only runs on Linux each OS has it's own 
way to support Docker so make sure to find your OS in the guide below and follow
how it is installed on each OS.

### Install Docker Desktop on Windows
Installing it on Windows is really easy but requires some prerequisites to work,
as I mention Docker only run's on Linux but with the latest support of WSL2 that
basically without going to technical is Linux running on Windows. HyperV is also
supported but Docker recommends using WSL for this so that is what I will cover
in this guide.

#### Install WSL

If not done already we need to install WSL and your favorite Linux Distro, my
preference is Debian but I know many people out there rater use Ubuntu.

To install WSL you need to open up and Powershell terminal as an administation 
and then run the command below.

````bash
wsl --install
````

Running this command will setup WSL and also install Ubuntu as the Distro of 
choice, if you like to use another distro than Ubuntu you need to add 
`-u [Name of distro of choice]` like if one want to install WSL with Debian as 
distro one would runt the command below.

````bash
wsl --install -d Debian
````

> **Can I change distro once installed?**
>
> Yes you can change or even add new Distro's after installing the first one, 
> The specified distro is only there to have one distro setup after install of
> WSL.
{: .prompt-info} 

The installation process might take some time but basically that is all that is 
needed to get WSL running.

After installation is done you should start up the Distro one time, this is 
because it will ask you to setup a user that will be used inside to distro.

#### Install Docker Desktop

Now that we have WSL and a Distro installed we can setup Docker Desktop so we 
can start using Docker.
Start by going to the Docker website and find Docker Desktop,
[click here](https://www.docker.com/products/docker-desktop/)
to go there immediately.

1. Click on "Download for Windows" and select where to download the file.

2. Once the file is downloaded, click on the downloaded file.

3. Go through the installation wizard.

4. Once the installation is finished the Docker Desktop should be starting if
you left the checkbox to start Docker Desktop on finish checked. If it doesn't
start then search for it on either your Dektop or search for it in your start 
menu.

5. Once Docker Desktop is running you can see an box at the bottom of the left
side, that is the status of Docker Desktop and can show different statuses:\
\
**Orange** Docker Desktop is still starting up, give it some more time and it 
should be up and running soon.\
**Green** Docker Dektop is running and ready to serve.\
**Red:** Something went wrong when trying to start up Docker Engine.

If Docker Desktop status bar is showing green then we are ready to move on, if 
it is showing red then something is wrong and the best solution for this is to
check the logs to see what has gone wrong, I will not show you how to check the
logs in this guide, that will take to long and needs a separate guide.

### Install Docker Desktop on MacOS

Installing Docker Desktop on a Macbook can be done in two ways, by downloading
from Docker website or install using Homebrew, the benefit of using Homebrew is
that we get some configuration pre-configured.

#### Install from Docker website

To setup Docker Desktop from Docker website you need to download the applikation
from the official Docker website, you can do that by 
[clicking here](https://www.docker.com/products/docker-desktop/) and then
selecting the version of the Mac you are using (Intel Chip or Apple Silicon)
and then once the applikation is downloaded drag it to your applikation folder.


#### Install using Homebrew

To install Docker Desktop using Homebrew we first need to have Homebrew 
installed on our computer, if you don't have that already you can 
[click here](https://brew.sh/) and then copy and paste the oneliner in your 
terminal.

Once Homebrew is installed we can run the command below to install Docker
Desktop direktly from our terminal.

````bash
 brew install docker
````

#### Start Docker Desktop

Start Docke Desktop and once Docker Desktop is running you can see an box at the bottom of the left
side, that is the status of Docker Desktop and can show different statuses:\
\
**Orange** Docker Desktop is still starting up, give it some more time and it 
should be up and running soon.\
**Green** Docker Dektop is running and ready to serve.\
**Red:** Something went wrong when trying to start up Docker Engine.

If Docker Desktop status bar is showing green then we are ready to move on, if 
it is showing red then something is wrong and the best solution for this is to
check the logs to see what has gone wrong, I will not show you how to check the
logs in this guide, that will take to long and needs a separate guide.

### Install Docker Desktop on Linux
TBD...

## Run your first container

Now that we have...

## Summary

Now we have setup Docker Desktop on our machine and run our first applikation 
to make sure everything is running as expected.
In the next guide I will go over how we can configure our containers when
starting them.

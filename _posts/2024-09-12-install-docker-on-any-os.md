---
layout: post
title: Install Docker on any OS
description: In this guide I show you how to install Docker on your system, no matter what OS you run.
date: 2024-09-11 23:13 +0200
image:
  path: /assets/img/posts/2024-09-12-install-docker-on-any-os/rubaitul-azad-HSACbYjZsqQ-unsplash.webp
  alt: The Docker logo in gold
  lqip: /assets/img/posts/2024-09-12-install-docker-on-any-os/rubaitul-azad-HSACbYjZsqQ-unsplash_lqip.webp
categories: [Docker, Docker for beginners]
tags: [docker, containerization]
---
This is part 2 in my Docker series, if you haven't read part 1 already you can find it [here](https://blog.digidaniel.tech/posts/don-t-know-docker-let-s-fix-that/)

Before we can start using Docker we need to install it on your system, this is done different depending on what OS you run. Down here I will guide you through each OS so you can follow along and install Docker and start using you too.

## Install on Windows

As I described in my previous post on Docker, Docker primarily uses Linux containers, but Windows provides solutions like WSL2 or Hyper-V to run Docker efficiently. without the need of running dual boot.

### WSL2 Or HyperV

With Windows you can run Docker either in WSL2 or HyperV, both have their advantages and disadvantages it depends on your situation.

The most common solution and recommended way is to use WSL and it is called WSL2 because as you guessed it there once was an WSL1 before WSL2 but with WSL1 had performance and compatibility issues because it relied on a virtual machine and shared the file system with Windows, leading to the development of WSL2.

### Install WSL2

To get WSL2 installed is really easy, all you have to do is to open up the windows start menu and type PowerShell right click on it and select “Run as administrator” then in the PowerShell window, run the following command:

```powershell
wsl --install
```

When running this command it will setup everything needed to run WSL2 on your local machine, it will also install the default Linux distribution that is Ubuntu, if you want to change what distribution that is installed you can add `-d <Distro>` at the end of the command.

You can find a list of distribution to install with this command:

```powershell
wsl --list --online
```

Not sure what distribution to run? Don’t worry, you can start with Ubuntu and If you'd like to try a different distribution later, you can easily install it from the Windows Store without uninstalling the current one.

### Install Docker Desktop

Once we have WSL2 installed it is time to install Docker, this is really easy as well let me show you.

Start by going to https://www.docker.com/get-started/ and click on the blue button saying “Download for Windows” or click on the downward arrow and select the installation that suits your system.

After the installation file has been downloaded you can run it and go through the installation process.

Once the installation is done you can start Docker from your start menu if it has not already been started by the installer.

The Docker window should now be visible, in the left bottom corner you should see either a red, yellow or green box telling you what state Docker is in.

If it is red, then something has gone wrong and you have to investigate further.

If it is yellow, then it is starting so just lean back and take a sip from your coffee cup and wait for it to start.

If it is green, that means that Docker is running and you are ready to start using it.

We can also confirm that Docker is running by opening up the distribution by opening your start meny and typing Ubuntu (or what distribution you have installed) and then in the terminal write:

```bash
docker ps
```

If you see an empty table that means every thing is working, but if you see an error message then something is wrong and needs to be investigated.

One common thing that can happen if you have installed Docker Desktop before WSL2 is that Docker is not configured to use WSL2 then you can open upp the settings on Docker Desktop and go to General and select “Use WSL 2 based engine” and save, now try again and it should work.

Good job you have now Docker installed and running on your system and you are ready to rock some Docker!

## Install on MacOS

There is two ways to install Docker on MacOS either by downloading the application from Docker's official website or by installing it using Homebrew.

### Install application from Docker's official website

Start by going to https://www.docker.com/get-started/ and click on the blue button saying “Download for Apple” ensure that the download corresponds to your Mac's chip (Intel or Apple Silicon), you can press the downward arrow and select the one that is correct for your computer.

This will download the application to your system then drag and drop the downloaded file to your application folder.

Once installed, open Docker. In the bottom-left corner, you'll see a red, yellow, or green indicator showing Docker's status.

If the box is red, something when wrong trying to start up Docker and needs to be investigated.

If the box is yellow, then it is starting so just lean back and take a sip from your coffee cup and wait.

If the box is green, that means that Docker is running on your system and you are ready to rock Docker.

We can test this by open up the terminal and type:

```bash
docker ps
```

If you see an empty table that means that Docker is running, but if you see any errors then something is wrong and needs to be investigated.

### Install using Homebrew

To install Docker using Homebrew we first need to make sure Homebrew is installed on your machine, this can be done by open up your terminal and run the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After the brew install process is done we can confirm that brew works by running:

```bash
brew -v
```

If you see a Homebrew version then we know that brew is installed and we are ready to install Docker, this is easily done by running the following command:

```bash
brew install docker
```

Easy as that Docker is installed, and you can confirm this by running the following command in your terminal:

```bash
docker ps
```

If you see any errors then something went wrong and you need to investigate further.

If it shows you an empty table then it means that Docker is running and you are ready to rock Docker!

## Install on Linux

Installing Docker on Linux there are different command depending on what distribution you run.

This guide applies to distributions using apt. First, we need to set up Docker as a valid and secure apt repository:

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Once this has been configured you can run the following command to install Docker and its dependencies:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

The last thing you might need to do is to make sure your user is in the Docker group by running:

```bash
usermod -aG docker <username>
```

Now you might need to close and reopen your terminal so that the new group will be used in the session.

Now you are ready to test if Docker is running, you can do this by running:

```bash
docker ps
```

If you see an error then something went wrong and needs to be investigated.

If you see an empty table then it means that Docker is running and you are ready to rock!

One common problems that might happen is that the Docker service has not been started, then you need to run:

```bash
# Start's the Docker service.
sudo service docker start
# Enables the Docker service to run on boot
sudo service docker enable 
```

## Conclusion

Now you have Docker ready to be used on your system, in the next post in the series we will run out first container and also write our first image.
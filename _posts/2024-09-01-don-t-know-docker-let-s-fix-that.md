---
layout: post
title: Don’t know Docker? Let’s fix that!
date: 2024-09-01 21:59 +0200
image:
  path: /assets/img/posts/2024-09-01-don-t-know-docker-let-s-fix-that/img001.webp
  alt: Shipping containers symbolizing Docker Containers
  lqip: /assets/img/posts/2024-09-01-don-t-know-docker-let-s-fix-that/img001_lqip.webp
categories: [Docker, Docker for beginners]
tags: [docker, containerization]
---

You keep reading about Docker and containers everywhere, but you’re not sure what they are. No worries, in this article, I will explain what Docker and containers are and why you need to know how to use them.

In this first article on Docker, we will focus on what Docker and containers are, what problems Docker aims to solve and the main building blocks. In later articles, we will go deeper into the fun technical aspects of Docker.

## What are Containers and how do they differ from Virtual Machines?

Before we can start talking about Docker, we need to pinpoint what containers are. Docker is built around the concept of Linux containers, so without understanding what Linux containers are, Docker might seem like dark magic and will quickly become hard to grasp.

One thing to point out before we start is that there are Windows containers as well, but Docker utilizes Linux containers, and therefore that is what will be the focus of this article.

To make it easy to understand we can see a container as a physical box that contains all the material and tools needed to complete a task and this box can be moved around without any of the materials or tools get lost.

The same is true for containers in the IT world, where a container contains all the tools and libraries needed to run an application and often the application itself is included in the container so it can easily be moved to other computers without the need to install each library and/or tools on each machine the application need to run on.

Now, you might think, ‘That sounds great, so it’s like virtual machines?’ and that is a very common misunderstanding when talking about containers, what differentiates containers from virtual machines is that and virtual machines are much bigger and contains an entire OS and all the tools and libraries needed to run the OS and this makes a virtual machine bigger in size and requires more resources to run.

In contrast, a container doesn’t contain its own OS. instead, it runs isolated on the hosts kernel but with its own processes, network and file system and therefore shares the resources on the host and this makes the container smaller in size and requirements, far fewer resources to run, and therefore can spun up extremely quickly on different machines and even makes it possible to spin up multiple instances of an container to increase redundancy.

One more thing that is good to know is that an container needs to be run on a Linux container, but even with that requirement both Windows and Apple has implemented virtual machine solutions to make it possible to run containers on other OS’s but with a minimal Linux kernel to keep size and resource requirements at a minimum, Windows is using HyperV or WSL to make this possible and Apple are using HyperKit or VirtualBox, so working with containers doesn’t lock the user to use Linux.

## What is Docker and what does it aim to solve?

So now we know briefly what containers are, what then is Docker?

Docker is a solution that makes it easy to create and remove containers using what’s called images or more exactly Docker images, an image is basically a pre-configured container that is ready to be run on any machine, so by using an existing image we can quickly start one or multiple identical containers as simply as pressing a button.

Docker was originally created to solve the famous “Works On My Machine” problem where something that worked on the developers machine doesn't run on other machines due to missing required packages, libraries or version miss matching, by using Docker Images the developers can specify each requirements and that will guarantee that the application will work on any machine as it works on the developers machine.

One example can be that the developer has build an PHP application with PHP 7 but the webserver running the application is running PHP 8. This will risk making problems with removed functions or having problems with that PHP 8 is stricter than PHP 7 in how code is written. Not only can it make sure  the versions match it can also make sure system configuration and dependencies is the same between each container setup.

## Where does Docker Images come from?

We have covered that when we want to run a container in Docker, we need to have a Docker Image but where can we get one?

So there are mainly three ways to get a Docker Image:

- Create one yourself by writing something that is called a Dockerfile (more about them in another article).
- Use the official Docker Hub this is what’s called a registry where anyone can upload their own images to share with others or keep for themselves by selecting it as a private image, there exists a couple other public registry’s out there like Azure Container Registry and GitHub Packages but Docker Hub are the most popular one.
- Use a private registry, either by setting up one yourself or by getting access to another private registry that other people or companies have set up, private repositories are often used by companies to keep their images secure inside the company to limit the possibility of the images to leak outside the company or to being able to implement security features as an Active Directory, another reason can be to make sure only internal and approved images are used to enhance security of the application.

We will go more in depth on Docker Images in another article where we cover how we pull, push and creates our own images.

## Docker sounds awesome but there must be some challenges.

Even if Docker is awesome and something you should start using right now, there are some challenges with Docker that people who work with Docker need to be aware of.

One challenge is securing sensitive data, where a developer that creates a Docker Image need to be aware that all data added to an image can be read by the users of that image, so the developer needs to be careful when adding sensitive data and make sure to take security measures when handling sensitive data in images. One solution to this is Docker Secrets that we will talk more about in the article about docker and security.

Another challenge is malicious intent of people pushing public images to Docker Hub, most Images is fine to use but with everything that is public there is always a risk that the person who created the Image has malicious intent with the Image so therefore one should be careful with using public images that is not created by a well-known source. One thing to look for when selecting an image to use is whether the publisher is verified by Docker. There is a blue check mark by its name that informs the user that the publisher is well known and verified by Docker them self as someone that can be trusted.

There can also be an challenge to keep the images up to date with the latest security patches, both the publisher of the image needs to update image and the users using the image also need to update to the newer images, this can easily be missed but there is tools like watchtower that monitors when newer versions is released and informs the users and can even do automatic update to newer versions.

## And now you know what Docker is, let’s start using it!

Now that you have an overview of what Docker is and why it was created you can start setting up your own Docker Environments using Docker Images and Containers, what are you waiting for? Start right now!

In the next article we are going to talk about how to pull down a Docker Image and run your first container, we are also going to cover how to create our own first Docker Image and push it to Docker Hub so we can share it with our friends.

Until then, have an awesome day!

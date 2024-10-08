---
layout: post
title: GitOps - and why you should start using it right now
description: What is GitOps? Why should you use it? Here I will tell you why GitOps is extremely valuable for your project and your company.
date: 2024-10-08 17:12 +0200
image:
  path: /assets/img/posts/2024-09-23-enable-msi-on-existing-magento-store/chuttersnap-Q4bmoSPJM18-unsplash.webp
  alt: Topview of a shore with many containers
  lqip: /assets/img/posts/2024-09-23-enable-msi-on-existing-magento-store/chuttersnap-Q4bmoSPJM18-unsplash_lqip.webp
categories: [DevOps, Intro]
tags: [devops, gitops]
---
Git is not a new thing and if you are a developer you will most likely be pretty familiar with it by now. But the word GitOps might be something you never heard before, and it is not just another buzz word that is thrown around but an methodology that has many benefits and here I will explain why you should start using it now in you company or in your projects.

## What is DevOps?

Before we start talking about the benefits of using GitOps we first need to cover what GitOps is.

GitOps is a methodology that incorporate IaC (Infrastructure As Code) and development best practices like version control, reviews and CI/CD pipelines to improve and automate the process of configure and setting up infrastructure in environments like in the cloud or on premise servers to make it easier to maintain and keep infrastructure secure.

Using GitOps makes the infrastructure more secure and reliable in the same way as when developers develops applications.

This might be unclear still but hopefully when you have read the benefits below you will see why it GitOps is a very good methodology that is almost required in every project.

## Version control

One of the biggest reasons why people started using git when developing application was version control, with version control the developers can go back and find out when an bug was introduced and why a specific line was added.

With version control it also made it possible to rollback to a previous version of a code that was previously working before an change was made to get the application working again why ironing out the bug that was introduced.

By the same reasons has version control a huge benefits when configure infrastructure, by having the history we can go back in time and see why the memory allocation was increased, why and alert was setup or why the auto scaling rule was configured to five instead of three, because after six months, a year or even longer, no one will remember why a decision was made for that specific change.

With the history we can also easily rollback to an old configuration of we notice that the new configuration didn’t work as expected, for instance we might have some auto scaling rules that checks that if CPU is greater than 45% then we should increase the instance count to four instead of three but a decision was made to increase this to 85% to save money but after some time we started to notice that 85% is to high and has started to make problems for our customers and we want to rollback to previous configuration that worked but without the history it is not certain that we would remember what the previous configuration was and now we have to spend days guessing what configuration we had originally.

## Collaboration

By utilizing git we can also make it possible so share the configuration code between each other, this enables the possibility to ensure that configuration is reviewed by other people before it goes to production.

It also opens up the possibility for another person to continue where someone else had started making changes if the original person got sick, left the project or other reason that prevented the person to finalize the changes.

## Automation

Having the configuration as code we can utilize CI/CD pipelines to make it possible to automate the apply of changes, doing we can quickly adapt the same changes on multiple environments without the risk of missing something of we have for instance an dev, test, qa and production environment that we want to apply the same changes to.

It is also way faster instead of making each changes manually in the infrastructure and lowering the down time the changes might cause of multiple changes has to be made simultaneously to get a system working.

## Security

On the topic of using pipelines, we can eliminate the amount of people that have access to the environment configuration and therefore only allow certain people to make changes to specific parts of an environment and limit the risk of someone getting access to a part of the system they should not have access to.

Something that also can and should be incorporated in the pipelines is configuration validation and automated testing to make sure that the configuration that is made is secure and that no security flaws is let through like sensitive data in plain text or miss configuration that might open up part of the system to the public that should be isolated from the outside world.

## Single source of truth

Keeping all configuration in one place is a good way to make sure no configuration goes missing or get's over written, instead of the need to dig through every service that is set up in the cloud to see what resource group they are connected to for instance, we can quickly figure that out only by searching through the git repository containing all the services to collect that information.

There will be less work and reduces the risk of missing some services that otherwise could be missed by manual labour.

## Migration

Because everything is keep't in git we can very easily migrate all systems, alerts, storage and so on to other providers. Let's say we started our journey on premises but later wanted to move to the cloud, doing this manually would take week, months or even years depending on how complex the existing environment is.

Instead we can only rewrite the parts that is provider specific to support the new provider and then quickly get it up and running on the new provider in the same way that it was running before with all the same configuration and monitoring as before.

## Conclusion

So by utilizing GitOps we get a more secure, reliable and stable infrastructure that we can quickly and easily maintain in only minutes rater than days.

We also the second guessing on why something has been changed or why something has been over written and no longer working as before.

By using CI/CD pipelines we can automate the deployment so we can guarantee that all environments works the same and lower the down time that the changes could create.

These are only some of the benefits of incorporating GitOps in a project, this post would be way to long if I would list every benefit that comes with GitOps, but these are the biggest one in my opinion.

So now that you know more about the benefits of GitOps, it is time to start incorporate it to your projects, once you get started I can promise the only regret you will have is that you didn’t started earlier.
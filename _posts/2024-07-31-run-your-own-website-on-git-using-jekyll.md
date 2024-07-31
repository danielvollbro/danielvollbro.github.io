---
layout: post
title: Run your own website on Github using jekyll (100% FREE)
date: 2024-07-31 21:34 +0200
---

In this guide I will show how you can run a website on Github entirerly free 
using Jekyll and Github Pages.

## Why?

The first question you might ask yourself is:

> Why should I run my own website in 2024, when I can use social medias like Facebook, LinkedIn or X (Twitter)?

**Short answer:** Yes you should run your own website and No social media is not
enough. 

**Long answer:** with social media it is easy that you become one
in the crowd and it is hard to stand out, with a website you will stand out more
and make yourself noticable.

During my years in the IT industry when recuiting people one of the things I 
often look for is if the person has a digital portfolio or a website that I can
check, either in the resume or after a Google search.
If I find that a person has something to showcase that person has a higher 
chance to be picked for the job because then I know the person has an interest
to learn and does not only does what is needed to get a degree or look good on
paper. 

## How?

So now when we know the why, lets focus on the how. We are going to use Jekyll
to create our website, Github as storage and Github Pages as an HTTP Server.

Enough talking! Let's get started and you will have your site up and running in 
no time!

### 1. **Setup your Github account**

First and foremost you need to create and Github account if you don't already
has one. If you already have one you can skip to step 2.

* Go to [github.com](https://github.com/)
* Click on `Sign up` button in the right corner

![Sign in button](/assets/img/posts/jekyll/github-register.webp)

* Enter your email
* Enter an secure password

> **Use password manager**
>
> If you are not already using a password manager I strongly recommend you to
> get one now!
{: .prompt-info }

* Enter an username
* You can ignore the checkbox if you don't want to receive annoing emails from 
Github trying to sell you stuff.
* Click on `Continue`

![Account details form](/assets/img/posts/jekyll/github-account-details.webp)

* Verify you account by completing the verification assignment
* Fetch the verification code that has been sent to your email and enter the
code

![Enter code from email](/assets/img/posts/jekyll/github-enter-code.webp)

Easy as that your account is now created and ready to be used.

### 2. **Create your repository**

Now that you have an account it is time to create an repository where you can
store your files for your website that we later will use when we go live using
Github pages.

> **What is an repository?**
>
> If you don't know what an repository is then I recommend you to read more 
> about what an repository is. But simply put a repository is a place where you 
> can store you files that you have created, being able to see the history of 
> how the files has changed and also collaborate with other people so you can
> share files between each other.
{: .prompt-info}

#### Login

* Start by login into your newly created account with the email or username and 
the password you entered in step 1.

![Github signin form](/assets/img/posts/jekyll/github-signin.webp)

#### Create your first repository

* Click on `Create repository` to your left

![Create repository button](/assets/img/posts/jekyll/github-create-repository.webp)

* Enter the name of the repository
* Select `Public` to make the visibility of your repository public

> **Free accounts has to set repository as public**
>
> Github only allows people with free accounts to use Github pages with public
> repositories.
{: .prompt-info}

> **With public repository your files are visible to anyone**
>
> By doing your repository public anyone that finds
> your Github account can view your file but can't edit them without your
> permission.
{: .prompt-info }

* Everything else can be left as it is
* Click on `Create repository`

![Repository details form](/assets/img/posts/jekyll/github-repository-details.webp)

Your new repository is now created

#### Upload your first file

> **Never push sensitive information**
>
> You should never commit passwords or other sensitive information to your
> repository no mather if it is public or not, once your sensitive data is in
> your repository it is there and the only way to remove it entierly is to
> remove your repository.
{: .prompt-warning }

Time to upload a file start by click on `Uploading an existing file`

![New repository created](/assets/img/posts/jekyll/github-repo-created.webp)

* Save the code below into an file on your computer somewhere you can easily 
find it and call it index.html and drag and drop the file to the drop field.


````html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>My awesome website!</title>
  </head>
  <body>
    <h1>Welcome,</h1>
    <p>This is my awesome website on github.</p>
  </body>
</html>

````

![Drop file field](/assets/img/posts/jekyll/github-drop-file.webp)

* Click `Commit changes`
* And now you have uploaded your first file to your repository

![Repository with added file](/assets/img/posts/jekyll/github-file-added.webp)

### 3. **Configure Github Pages**

Time for the fun part, we new have a repository and our first file uploaded, 
next we want to get it live on the intraweb so we can showcase it to other
people.

* Start with click on `Settings` in our repository page
* In the left menu select `Pages`
* Under `branch` select your main branch
* And click on `Save`

![Github pages setup page](/assets/img/posts/jekyll/github-pages-details.webp)

* Wait a while... (The process to publish your page takes about 1-2 minutes to
complete)
* If nothing happends, refresh the page to see if the auto refresh has failed
us.
* Once you see the `Your site is live at...` it is confirmed that the site is
live.

![Github pages setup page with created page](/assets/img/posts/jekyll/github-pages-created.webp)

* To go to the site click on the `Visit site` button or navigate the the url
shown.

And easy as that your are live with your own website on Github entirely free!
Congratulations!

![The live website](/assets/img/posts/jekyll/github-pages-success.webp)

### 4. **Jekyll**

You now have your site live, but it really won't make your friends impressed
so let's make it more beautiful.

So Github Pages only works with static files (.html, .css, .js and so on) so we
can't be using an CMS like Wordpress or similar instead we will be using a tool
called Jekyll to build our website.

> **What is Jekyll?**
>
> Jekyll is a static site generator that generates an static website using 
> plain text files. You can read more about Jekyll at
> [jekyllrb.com](https://jekyllrb.com/)
{: .prompt-info}

#### Setup Jekyll

...

### 5. **Custom domain**

If the {username}.github.io domain is something you are not happy with it is 
possible to change it to a custom domain, unfortinutly here is where the free 
part ends setting up a custom domain cost some money and is stricly not needed 
but makes it look more personal.

## Conclusion

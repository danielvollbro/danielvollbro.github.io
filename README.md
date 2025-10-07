# Daniel Vollbro - Personal tech blog

This is my personal blog, here I document things that I learn or already know
to share my knowledge with others and also to have a place where I can go back
when there is something that I have forgotten and need to refresh.

I am no professional writer so don't expect the content to be in highest quality,
I try to make the feel more friendly and less professional and stale, so if you
are expecting any professional guides or posts then you are in the wrong place.

## Why keep this public on Github?

I don't see any reason not, hopefully this encurage more people to set up their
own blog and document their own journey with others. Doing this is a great way
to learn about documentation and show off your personal projects for others to
follow along.

## Guides

### Convert jpg to webp

Most of my images comes from stock image pages like https://www.pexels.com/ or
https://unsplash.com/ these images is often in jpg and to optimize the site I
convert the images to webp using Imagemagick, this is done by running the
following command:

````shell
convert <jpg_img_path> <webp_img_path>
````

### How to generate LQIP

This blog is using LQIP to lazy load images, I generate the LQIP images by 
running the following command:

````shell
convert <source_img_path> -resize 2x2 -quality 20 <target_img_path>
````

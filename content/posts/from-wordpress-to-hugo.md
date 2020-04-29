---
title: 'From WordPress to Hugo'
date: Wed, 29 Apr 2020 16:55:00 +0000
draft: false
tags: ['Blogging', 'Hugo']
---

If you’ve visited this site in the past, you’ve probably noticed a change. I recently changed my blogging platform from WordPress to [Hugo](https://gohugo.com). Hugo is static site generator. I also changed my theme.

## Why?
I was never really satisfied with the performance of my WordPress site. Combine that with the fact that my site kept going down every month, I decided that it was time to do something and be more in control. 

I like Markdown very much and wanted something that supported writing posts in Markdown (and enable me to write in wonderful writing tools like Bear and Ulysses). I also wanted something fast that didn’t require me to install an entire development platform just to write a post. Hugo was really the only generator that satisfied all those constraints.

I also changed my blog host from Bluehost.com to Github pages. I trust Github to stay up more than I do Bluehost, but I think the real culprit to my site going down regularly was WordPress.

## The Process
First, I found [blog2md](https://github.com/palaniraja/blog2md), a script that that converted all my WordPress posts to Markdown.
Then, I exported all my images from WordPress, put them in the proper place, and then tweak all my posts so they had the right image URLs . 

After that it was choosing a theme, tweaking it to me liking (changing colors, adding some links, configuring options), updating the DNS to point to Github, and I was done. The whole process took about a week.

## The Results
I’m really happy with the results. The site is much faster and easier to maintain. All I need is my Markdown editor to write a post. Once the post is ready, I simply tell Hugo to generate the site and then push it up to the [GitHub repo](https://github.com/infinitenil/rodschmidt.com)
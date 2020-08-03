---
title: "oakst.io"
date: 2016-10-01T11:30:30-05:00
slug: "oakstio"
draft: false
tags: ["front-end", "bulma", "hugo"]
project: "oakst.io"

inProgress: false
started: 2016-10-01
completed: 2016-11-01
description: "A little bit about this website."
---

The design and purpose of this website has changed significantly over the years.  As my career has shifted and my abilities improved, so has this website. Here's a little bit of history and context.

##  Wordpress - 2016-19
I graduated in 2016 with a Bachelor's in Electrical Engineering. I had never bothered with looking for
an internship and my grades from the first two years of college had dragged down my GPA into Don't Hire 
This Guy territory. I felt like I needed a way to prove to potential employers that I had the chops so
I created this website as a way to showcase the projects I had worked on throughout my undergrad.
I chose the name Oak St. Electronics because my a) jakejones.com was taken and b) I lived on Oak St.

I chose to use Wordpress because it was easy. Easy and bloated and ugly.

## Ghost - 2019
At this point I had found a job and I figured it was time to remove the projects I had worked on throughout
college. The website felt more like an entry-level resume more than anything else. Additionally
I hard begun working on my MSCpE with a focus on IoT and I figured that it was only proper for somebody
who works with web technologies to have a website more advanced than Wordpress. I also wanted to get into
blogging about the projects I was working on.

I went with the [Ghost](https://ghost.org/) as it seemed like a more advanced and less bloated CMS than
Wordpress. It was a pain migrating the little content that I had from my Wordpress site. I decided to skip
the hosting fees and deploy on my own [Digital Ocean](https://www.digitalocean.com/) droplet. I felt like
a real web developer!

Ghost felt like overkill for what I really needed. It was the small stuff I didn't like - things like
how each post had an author and how each author had a role. It was just me - why do I need to give myself
a role? At the end of the day the small stuff doesn't *really* matter but it just felt like I 
was working inside of a pretty rigid and pre-defined box.

## Angular - 2019-20
I had found some projects around the office that could be redesigned using technologies modern to this century and where I could apply some of the things that I had been learning about in my degree program.

This is where my career started to shift from Electronics Engineer towards Web Developer. My happy place
is where these two fields meet, somewhere in the realm of embedded IoT systems.

One of the first projects I redesigned was a multi-user data collection system. I can't get into
details but the old way of doing things involved proprietary hardware and the LabVIEW ecosystem. My new
system ran a [MEAN stack](https://en.wikipedia.org/wiki/MEAN_(solution_stack)) with the app served
to end users at stations powered by single-board computers. 
This is the project that introduced me to [Angular](https://angular.io/) and web frameworks in general.

I used these new skills to redesign my website into something more personal. It turns out that Angular isn't that great for simple, static blogs.

I spent a lot of time trying to figure out the best way to store my blog's content. I tried out a few
headless CMS's but they were all very opinionated about how the data was stored and accessed. I had been
burnt before trying to migrate content from one blogging platform to another and I wanted a storage
format that was portable. 

I decided to roll my own CMS with the blog posts formatted by Markdown. The plan was to store the site
content in a [mongodb](https://www.mongodb.com/) database and query the content with a homegrown API. As
a proof of concept the first version of my Angular-powered website simply displayed the content of the
site served from static files in the site directory. All of the navigation links had to be created manually
and it sucked.

I began work on a node package that would scan the directory for content and push it to my database if it
was new or updated. It worked! One of the issues though was post images. This was a real head-scratcher.
I had considered trying to store the images in the mongodb database, which was technically possible, but it
felt like the wrong thing to do. I had considered using AWS or Google Cloud buckets to store the images
and generate URLs on the fly but I ended up doing something even more clever -  I gave up.

I realized that I had been trapped by my ego and the [sunk cost fallacy](https://en.wikipedia.org/wiki/Sunk_cost).
Why was I spending so much time trying to reinvent the wheel? All for something that *maybe* got one page
view a month?

## Hugo - 2020-?

Around this time I had learned about static site generators. For those who don't know, a static site
generator builds your website into static HTML files. You write the build instructions and templates 
and once it's built, it's built. Instead of having to
pull the site's content from some database somewhere, the site's content had already been pulled from
the database during the build process and the website has already been pre-assembled on your webserver. 
Websites used to be build around static content but we've gotten away from the practice as 
web technologies have progressed.

The immediate benefit is that your users can load your website that much quicker. For me and the way
I have things configured, the biggest benefit is not having to manage a separate database and API.

I chose [Hugo](https://gohugo.io/) because I'm not a fan of JavaScript and I wanted to learn
more about the Go programming language. My entire website lives in a GitHub repository. I also
migrated from Digital Ocean to [Netlify](https://www.netlify.com/) to automate the build process and hosting.
I've got better things to do than manage my own server. 

This Hugo/GitHub/Netlify "stack" is such a breath of fresh air. It's so easy to update and maintain my site now.
A simple push to the repo is all it takes for Netlify to rebuild and redeploy this site. 
Where have you been all of my life?

As far as the front-end goes, this site is styled by the [Bulma](https://bulma.io/) CSS framework. 
This is my first time
ever using a CSS framework and I've got to say, I like it. Bulma is what they call utility-first, so styling 
can get pretty messy (eg. `<h1 class="has-text-primary is-italic is-size-2>`) but it works well with templating
where I don't have to redefine the component very often. Bulma is also opinionated but it's easy to customize and
I think it just looks prettier than the omnipresent [Bootstrap](https://getbootstrap.com/).

Anyways, this is where I'm at now. This post ended up being more autobiographical than I had planned but I
thought the context for this site's history was nice to know.

The source code for this website can be found in my [GitHub repo](https://github.com/bruthaearl/oakst). 
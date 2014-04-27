---
layout: post
title: "Getting Started with Octopress"
date: 2014-04-03 00:06:42 -0300
comments: true
categories: [Blogging, Tools]
---

This is just some thoughts after my experience of migrating my blogging platform from **Wordpress** to **Octopress**. For those who have never heard about it, **Octopress** is basically a tunned Jeckely for hackers with a set of plugins ready to use. The big goal of these blogging platforms is that you can write your posts using **Markdown** language instead of plain HTML or some word like editor (If you are used to write github **README.md** file, you will love it!). In this way it transforms the act of writing a post into a programming experience.

You can find the [installation](http://octopress.org/docs/setup/) and [deployment](http://octopress.org/docs/deploying/) instructions at the official [website](http://octopress.org/).

### Things that I did right after the standard installation
* Set aside config for different pages.
* Configure some built-in **Octopress** plugins.
* Enable comments through [disqus](http://disqus.com/).
* Create and configure a Google Analytics account.
* Configure the **about me** at the right side bar.

<!-- more -->

### Set aside config for different pages
There are a couple of entries in the `_config.yml` file that you can modify in order to set different aside (right barmenu) on different pages. The most common distintion is between the index (where you usually show a summary for the last 10 posts) and the post page. For this example you have to modify `blog_index_asides` and `post_asides`.

### Configure some built-in Octopress plugins
On this step you will set the basic configuration for integrate.
**Google+**, **Twitter**, **Facebook**, **Github**. This step it's very easy just follow the [official documentation](http://octopress.org/docs/configuring/) for it.

### Create a Disqus account and configure it
The standard way of making your posts commetable in **Octopress** is configuring **disqus** for it. This is a website that will manage your comments for you, so you will be able to migrate your blogging plataform in a much easier way. The only thing you have to do is to [create an account](https://disqus.com/profile/signup/) and then configure it on **Octopress**. In order to do this you have to set these two variables in the `_config.yml` file:
```
disqus_show_comment_count: true
disqus_short_name: YOUR_DISQUS_ACCOUNT
```

Finally make sure that your blog entries have the `comments: true` option on top.

### Configure the blog domain
NOTE: I'm using github pages for hosting and Go Daddy to get my domain. Other deployment strategies can be found [here](http://octopress.org/docs/deploying/).

* Ceate a `CNAME` file with your domain and put it into the `source` folder. Then push it `git push origin source` and deploy your changes `bundle rake deploy`.
* Configure your domain provider ([Godaddy](http://godaddy.com) in my case) to tell it where your application is hosted.
* Finally we have to wait for the DNS to be updated. This could take a few minutes, so be patient.

### Configure Google Analytics
First you have to configure your site in Google Analytics anf get the Google Analytic ID for it. Then go to the `_config.yml` file and set it like this:
```
google_analytics_tracking_id: YOUR_APP_ANALYTIC_ID
```
Again this will take some time, so be patient.

### Configure the "about me" at the right side bar so you can show who you are
**Octopress** (at least the version I'm using) already comes with a aside partial `/source/_includes/custom/asides/about.html`. There you can write your About Me section and the configure it to be shown as any aside in the `_config.yml`. In my case I'm showing it in the `posts_index_page`, just next to the last 10 posts summury.

NOTE: For those who come from **Wordpress** and are used to [gravatar](http://gravatar.com), I've created a jQuery plugin for it. You can find it on the [jQuery plugin repo](http://plugins.jquery.com/gravatar/) and [Github](https://github.com/ezilocchi/jquery-gravatar/).

{% include_code About Me partial about.html %}

### Conclusion
What I really like about **Octopress** it's the way that you write posts, it's just like programming. You don't need to use any specific tool of editor, just your favorite Editor or IDE :)

BTW, since I'm hosting [my blog](https://github.com/ezilocchi/ezilocchi.github.io) on github, you are able to take a look at it.


---
layout: post
title: "Getting Started with Puppet and Rails"
date: 2013-12-23 20:51:04 -0300
comments: true
categories: [Puppet, Ruby, Rails, Ubuntu]
---

This is just an example project showing how to get a **Ruby on Rails** environment up and running with just one command line using **Puppet**. It was original created for (but it can be easily modified to run with other setups):

* **Ubuntu** 13.10 (since it’s one of the most supported distros in the linux community)
* **Rbenv** as Ruby manager (there is no way of developing ruby without a Ruby Version Manager. You can user RVM as well)
* **Postgresql** (since it’s 100% open source and it’s the *<a href="http://www.heroku.com" target="_blank">heroku</a>* default)
* **VIM** (with *<a href="https://github.com/carlhuda/janus" target="_blank">janus</a>* distribution)
* **Nodejs** (since it’s the most popular and stable JS engine)
* **Git** (it doesn’t need any explanation)

<!-- more -->

Any contribution to improve it (like proposing a better way of writing
the config) or add more features (like supporting others linux
distributions) is very welcomed :).

So basically you can run this command from your terminal:

```
sudo curl https://raw.github.com/ezilocchi/my-puppet-conf/master/install.sh | bash
```

or you can go to the [github repo](http://github.com/ezilocchi/my-puppet-conf) and see how it works

Long story
----------

During the last 6 month I had to set the development environment in my laptop 3 times. First I got a new job as freelance developer and I had to follow a step by step document to convert my machine into a full developer workstation. Then my hard drive broke and I had to set everything again, following every little step, running every command from the console. Finally I got a new laptop, an amazing Dell XPS, and guess what I went to that setup document one more time. Actually the story doesn’t end there, some months later two other devs joined the project and they were able to run this command instead of follow the whole document. At some point this  became something useful

So my first attempt was to create a shell script, something that you can run from the console and automatically sets everything. But then I remembered puppet, I used it in a project but I’ve never configured it by my own (only run ```puppet agent```). Usually devops are who handle this kind of things, not developers.

This is just a puppet config that I created as a self learning project that has grown with the time and helps other devs on this project to set up their own environments.  

IMPORTANT: The final purpose of puppet it’s much larger than this. The idea it’s to stay in sync all your **environments (demo, stage, prod, etc…)** and avoid any kind of configuration issues during deployment, among other things. But as I mentioned before this started as a learning experience but ends up in something useful for this project.

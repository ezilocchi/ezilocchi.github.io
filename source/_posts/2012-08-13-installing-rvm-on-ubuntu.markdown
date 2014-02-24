---
layout: post
title: "Installing RVM on Ubuntu"
date: 2012-08-13 11:04:59 -0300
comments: true
categories: [Ruby, Ubuntu]
---

I have recently started to work on a **Ruby on Rails** project which is composed by different subsystems under a RESTful architecture. The thing is that every piece in the whole picture has different deployment characteristics, for example some of them use **ruby 1.8.7** some others use **ruby 1.9.3** and even the ruby interpreter could be different (**ruby RMI** or **ree** for example). So imagine that you have to work with all these projects at the same time in the same work station, even for a single Story you you might jump between them.  So the idea is to find a way to deal with all these infrastructure issues in just one dev machine without pain.

So here we have one amazing tool that makes this process much easier: **Ruby Version Manager (RVM)**

Note: This tutorial was written based on an official RVM [site](https://rvm.io/rvm/install/) and shows how to do it for **Ubuntu 12.04** but it should also works for other Ubuntu versions.

> IMPORTANT: The RVM alternative is [rbenv](https://github.com/sstephenson/rbenv), if you want to try it check the [rbenv-installer](https://github.com/sstephenson/rbenv)

<!-- more -->

1. Install curl, we will use it for the RVM installation

   ```
   $ sudo apt-get install curl
   ```

2. Install the stable RVM version. Complete guide and source [here](https://rvm.io/rvm/install/)

   ```
   $ curl -L https://get.rvm.io | bash -s stable --ruby
   ```

3. We need to reload the RVM in the shell

   ```
   $ source ~/.rvm/scripts/rvm
   ```

4. Check the installation

   ```
   $ type rvm | head -n 1
   ```

   Expected output `> rvm is a function`

   > NOTE:if don't see this message you probably forgot to reload the RVM `source ~/.rvm/scripts/rvm`

5. Install the ruby version that you want to use (To see available versions: `rvm list`)

   ```
   $ rvm install <version>
   ```

6. Let’s try it!!! To do this we will run the ruby console `irb`, and if everything is fine you should see the irb prompt. (something like this depending on the ruby version):

   ```
   1.9.3-p194 :001 >
   ```
   > NOTE: if you see this message right after run irb, check that you have all the RVM requirements
   
   ```
   Readline was unable to be required, if you need completion or history install readline then reinstall the ruby.</pre> You may follow 'rvm notes' for dependencies and/or read the docs page https://rvm.io/packages/readline/ . Be sure you 'rvm remove X ; rvm install X' to re-compile your ruby with readline support after obtaining the readline libraries.
   ```

7. For checking the RVM dependencies just run rvm requirements, and install if it is needed, all the RVM dependencies. You will probably see this

   ```
   bash >= 4.1 required
    curl is required
    git is required (>= 1.7 for ruby-head)
    patch is required (for 1.8 rubies and some ruby-head's).
    ...
    Additional Dependencies:
    # For Ruby / Ruby HEAD (MRI, Rubinius, & REE), install the following:
    ruby: /usr/bin/apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion
   ```

   Usually you have to install this:
   ```
   sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion
   ```

8. Finally define the default ruby version, in order to load it automatically

   ```
   $ rvm use <version> --default
   ```

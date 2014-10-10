---
layout: post
title: "Managing Sidekiq with Upstart"
date: 2014-10-10 15:47:53 -0300
comments: true
categories: [Sidekiq, Rails, Ubuntu]
---


This post has started as a research on **How to start Sidekiq on a system reboot**. After read blogs and try different approaches we ended with Upstart, that is the new way of managing jobs on the Ubuntu platform (Actually not so new, it was included on the standard releases since 10.04 I think). With it you basically define a standard **Ubuntu Job** and then you can just:

```
sudo service sidekiq [start | stop | restart | status]
```

We have a quite standard Rails stack that involves:

* **Ubuntu 12.04** LTS
* **Sidekiq** with **Redis**
* [RVM](http://rvm.io/) to manage our ruby versions (with [rbenv](https://github.com/sstephenson/rbenv) you can use the [example](https://github.com/mperham/sidekiq/tree/master/examples/upstart) on the official sidekiq github repo)
* **Capistrano** for deploying the application

<!-- more -->

The only thing to do in order to get your new job up and running, it's create a conf file under `/etc/init` directory. The file name will be the job name that you will `[start | stop | restart | status]` later.

The structure of this file must follow the Upstart DSL (you can see different example on `/etc/init` directory and on the [official documentation](http://upstart.ubuntu.com/cookbook/)).

For our purpose we are going to create a file `/etc/init/sidekiq.conf` so you can run `sudo service sidekiq [start | stop | restart | status]` and at the same time we can tell Upstart that it should start everytime the **system boot**.

On my case I used this [example](https://github.com/mperham/sidekiq/tree/master/examples/upstart) as a starting point adapting it to RVM (note that it uses **rbenv**). On the other hand this script was not thought to start sidekiq when the **OS is booted** but that is a quite simple thing, just add `start on startup`

```bash /etc/init/sidekiq.conf
# Just a custom description for our Job
description "Sidekiq Background Worker"

# On which conditions the job should start. In this case it's very simple: On the system startup (this is basically when the system is booted)
start on startup

# On which conditions the job should stop. In this case when the system reboot (http://upstart.ubuntu.com/cookbook/#runlevels)
stop on runlevel [06]

# This are the User and User Group that will be used to run the Job. On our case it should be the user that we have set on our capistrano script for instance.
# You can check at `config/deploy/<environment>.rb` on this line `server <some_ip_addreess>, user: <deploy_user>`

setuid deploy_user
setgid deploy_group

# This indicate that we want to restart the Job if it crashes
respawn
respawn limit 3 30

# TERM is sent by sidekiqctl when stopping sidekiq.  Without declaring these as normal exit codes, it just respawns.
normal exit 0 TERM

script
# this script runs in /bin/sh by default
# respawn as bash so we can source in RVM
exec /bin/bash <<EOT
  # use syslog for logging
  exec &> /dev/kmsg

  # Jump into the capistrano deployment directory
  cd /var/projects/<YOUR_PROJECT_DIR>/current

  # Start Sidekiq through RVM. Note that I'm using the standard Capistrano paths
  exec ~/.rvm/bin/rvm-shell -c 'bundle exec sidekiq --index 0 --environment <environemnt> --logfile /var/projects/<YOUR_PROJECT_DIR>/current/log/sidekiq.log'
EOT
end script
```

One more thing that might be helpful is how to debug this in case it doesn't work on the first try or if you need to adapt it. Well you can just
`tail -f /var/log/syslog` and see the errors.

That is pritty much it, now your Sidekiq process is being managed as a standard Ubuntu Job.

Resources:

* https://github.com/mperham/sidekiq/tree/master/examples/upstart
* http://upstart.ubuntu.com/getting-started.html

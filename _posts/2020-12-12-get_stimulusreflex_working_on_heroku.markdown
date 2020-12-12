---
layout: post
title:      "Get StimulusReflex working on Heroku"
date:       2020-12-12 19:30:18 +0000
permalink:  get_stimulusreflex_working_on_heroku
---

#### Hint: You need to get Redis configured locally and on Heroku


This is part tutorial, and part my own experience. It is not all encompassing, but it's a start.

If you aren't hosted yet check out  my [blog](#)(coming soon) on why you should host at the start of every rails build.

If you need a primer on Stimulus Reflex check this [blog](#)(coming soon).


## Setting up Redis Locally
###### The following is straight from https://redis.io/topics/quickstart

1. Install redis
```
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
```

2. Test if redis installed correctly
```
//start the server in on console
$ redis-server

//test the server in a separate console

$ redis-cli ping

// Terminal output should be Pong
```

###### You want to secure Redis

Now the quick start makes it seem super scary, as they should, that redis can be a big security concern.  Here are their steps : 
```
1. Make sure the port Redis uses to listen for connections (by default 6379 and additionally 16379 if you run Redis in cluster mode, plus 26379 for Sentinel) is firewalled, so that it is not possible to contact Redis from the outside world.

2. Use a configuration file where the bind directive is set in order to guarantee that Redis listens on only the network interfaces you are using. For example only the loopback interface (127.0.0.1) if you are accessing Redis just locally from the same computer, and so forth.

3. Use the requirepass option in order to add an additional layer of security so that clients will require to authenticate using the AUTH command.

4. Use spiped or another SSL tunneling software in order to encrypt traffic between Redis servers and Redis clients if your environment requires encryption.
```

TL&DR: Steps one and two. On Mac open your firewall and make sure that you don't allow connections to Redis. Make a config file in your root directory name ```redis.config``` and put the following line ```bind 127.0.0.1```. 

You can also dowload the redis.conf directly from redis at http://download.redis.io/redis-stable/redis.conf. This will give you a lot of stock options to change and already has bind 127.0.0.1 uncommented.

Woof. Step one Redis is a go. Remember to run it every time you will be working with the live version of your site locally. Stimulus will shut down your rails server if it can't connect to a redis server. 

## Setting Up Redis For Deploy to Heroku

## Start with your local machine

We need to add a few gems

```
///Gemfile
gem 'redis', '~> 4.0'

gem "hiredis", "~> 0.6.3"

gem "redis-session-store", "~> 0.11.3"

```

Set up production.rb to cache using redis.

```
config.cache_store = :redis_cache_store, {driver: :hiredis, url: ENV.fetch("REDIS_URL")}
   config.session_store :cache_store,
     key: "_session",
     compress: true,
     pool_size: 5,
     expire_after: 1.year
```

## Dun Dun Dun! Add Redis to Heroku and push

In your command line run the following

```
heroku addons:create heroku-redis:hobby-dev -a your-app-name

\\ the heroku add on takes a bit to install you can check the progress with

heroku addons:info

\\ When ready

git push heroku main

```







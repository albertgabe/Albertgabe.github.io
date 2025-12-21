---
layout: post
title: Fixing Jekyll server & Ruby Gem on macOS Catalina.
date: 2021-04-16 09:49:20 +0800
category: Debug
tags: [tutorial, debug]
---
After I upgraded my Mac Pro & MacBook Pro to macOS Catalina, I can no longer run Jekyll bundler on it, keep throwing weird errors in ruby gem, since I know there are some shit ass system r/w issues so I'm not surprise that it ran into some stupid issues like this.

<img src="{{ site.baseurl }}/images/20210416_Jekyll/No_Perm.png" width="400"/>

The screenshot I showed here is pretty common on macOS. Ruby gem that are preinstalled on macOS doesn't come out of the box with a proper Ruby development environment. Before doing proper research I just used `sudo`, which after research turns out is **highly** not recommend to do it. And even doing it so on Catalina still make bunch of errors while do "bundle install" (Which works on Mojave before.), I have to dig deeper.

## Before start

I decided to clean out the whole Ruby Gem mess I did w/`sudo` permission.

First, open terminal and type in `sudo gem uninstall -aIx`, This will uninstall all gems.

After that Navigate to /Library/ and delete Ruby folder.

### • Prerequisites
[HomeBrew](https://brew.sh/)

## Install Ruby

Now, launch Terminal and type in `brew install ruby`

When the installation is finished, type: `echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc`

Restart Terminal.

And now the full Ruby environment is set!

You can check if you are on HomeBrew Ruby by typing `which ruby` in terminal, it should report /usr/local/opt/ruby/bin/ruby and not /usr/bin/ruby, here's the screenshot showing both preinstalled macOS Ruby and HomeBrew Ruby.

<img src="{{ site.baseurl }}/images/20210416_Jekyll/Ruby_System.png" width="400"/>
<img src="{{ site.baseurl }}/images/20210416_Jekyll/Ruby_HomeBrew.png" width="400"/>

## Bundler & Jekyll setup

cd into your Jekyll blog folder

Terminal: `gem install bundler` and then `bundle install`

But at this moment when you try to run `bundle exec jekyll serve` you might be greet with bunch of errors.

That's because Ruby 3.0 doesn't come with webrick anymore, so you have to `bundle add webrick`, after that Jekyll server should be working! :)
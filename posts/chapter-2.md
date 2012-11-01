---
title: "Chapter 2"
date: '2012-10-29'
description:
categories:
tags: []

<!-- type: draft -->
---

It should come as no surprise that, as a Ruby on Rails developer, I will choose that as a web development framework.  I'm also using a Dell XPS13 with the developer image of Ubuntu 12.04 installed.  Most of the commands in this tutorial should work fine on Macintosh.  If you are on Windows, I recommend borrowing someone else's machine or downloading Ubuntu and running it on yours.  Here is a good place to start:

http://www.ubuntu.com/download/desktop/windows-installer

Now that you're on a Unix machine, open a terminal.  Bash is fine, though I converted to the Z shell recently.  If you'd like to know why or perhaps even try it out, I recommend Ryan Bates' excellent screencast:

http://railscasts.com/episodes/308-oh-my-zsh

If you don't know what the terminal is, you may be in the wrong place.  It's cool.  I was there, too.  And you know what really helped me out?  Michael Hartl's excellent Rails Tutorial, which you can find here:

http://ruby.railstutorial.org/ruby-on-rails-tutorial-book

For my text editor I've chosen Sublime Text 2.  There are many text editors and IDE's like it, but this one is mine.  My text editor is my best friend.  It is my life.  I must masterit as I must master my life.  On Ubuntu 12.04 I've found the best way is to add a ppa and let Ubuntu do my work for me.  I recommend starting here:

http://www.webupd8.org/2012/06/sublime-text-20-stable-released-ppa.html

Now that your text editor is up and running, you'll want to install Ruby.  The easiest way I've found to do so is via the Ruby Version Manager.  I have found installing RVM to be a bit more of a moving target than other things, and so will leavy you to start here:

https://rvm.io/rvm/install/

Once you've installed RVM, the first thing you'll want to do is install the latest version of Ruby.  For the purposes of this tutorial, I recommend downloading ruby 1.9.3 patch level 194 and setting it as your default ruby.  From your terminal:

$ rvm install 1.9.3-p194 --default

Now let's create a gemset.  I like to create a gemset with the name of the application I'm building combined with the version of rails I'm using.  Since I'm planning on doing this tutorial using Rails 3.2.8, we can create an appropriate gemset like this:

$ rvm use 1.9.3-p194@anydiver328 --create

Now let's install rails.  Before we do this, let's set up our .gemrc file to ignore ruby documentation when installing gems.  

$ subl ~/.gemrc

This should open a hidden file in your home directory called .gemrc in Sublime.  It should be empty.  In order to save yourself the time it would take to download the documentation, as well as the space you'll need in order to store them.  Don't worry, the online documentation is more than adequate and easier to navigate:

gem: --no-ri --no-rdoc 

Now we can install Rails.  

$ gem install rails -v 3.2.8

Whoah.  We've come a long way so far.  Enough for today.  Come back tomorrow.
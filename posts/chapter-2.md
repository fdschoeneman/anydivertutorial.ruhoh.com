---
title: "Chapter 2"
date: '2012-10-29'
description:
categories:
tags: []

type: draft
---

It should come as no surprise that, as a Ruby on Rails developer, I will choose that as a web development framework.  I'm also using a Dell XPS13 with the developer image of Ubuntu 12.04 installed.  Most of the commands in this tutorial should work fine on Macintosh.  If you are on Windows, consider borrowing someone else's machine or downloading Ubuntu and running it on yours.  Here is a good place to start:

http://www.ubuntu.com/download/desktop/windows-installer

Now that you're on a Unix machine, open a terminal.  Bash is fine, though I converted to the Z shell recently.  If you'd like to know why or perhaps even try it out, I recommend Ryan Bates' excellent screencast:

http://railscasts.com/episodes/308-oh-my-zsh

If you don't know what the terminal is, you may be in the wrong place.  It's cool.  We've all been there.  And you know what really helped me out?  Michael Hartl's excellent Rails Tutorial, which you can find here:

http://ruby.railstutorial.org/ruby-on-rails-tutorial-book

There are many text editors and IDE's out there, but Sublime Text 2 is mine.  My text editor is my best friend.  It is my life.  I must master it as I must master my life.  On Ubuntu 12.04 a decemt way to get it is to add a ppa and let Ubuntu do the work for you:

http://www.webupd8.org/2012/06/sublime-text-20-stable-released-ppa.html

Now that your text editor is up and running, you'll want to install Ruby and Rails.  I recommend following [Daniel Kehoe's](http://daniel.merciboq.com/) insanely detailed and incredibly awesome instructions, which are likely to be up-to-date even when this tutorial is not:

http://railsapps.github.com/installing-rails.html

Once you have a good working version of rvm, lets get you on the version of ruby we'll be using for this tutorial:

`$ rvm install 1.9.3-p194 --default`

Now let's create a gemset.  I like to create a gemset with the name of the application I'm building combined with the version of rails I'm using.  Since I'm planning on doing this tutorial using Rails 3.2.8, we will create a gemset like this:

`$ rvm use 1.9.3-p194@anydiver328 --create`

Before we install rails, let's make sure our .gemrc file is configured to ignore ruby documentation when installing gems.  

`$ subl ~/.gemrc`

This command tells sublime to open a hidden file in your home directory called .gemrc.  In order to save yourself time installing gems, you can skip installing unnecessary documentation -- which is best viewed online -- by adding this line to your ~/.gemrc file:

    # ~/.gemrc

    gem: --no-ri --no-rdoc

Now let's go back to the terminal and install the Rails 3.2.8 gem into that gemsest:

`$ gem install rails -v 3.2.8`

That should take care of most of our overhead.  
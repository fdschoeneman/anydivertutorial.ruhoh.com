---
title: "Chapter 3"
date: '2012-11-01'
description:
categories: tutorial
tags: []

type: 
---

You can generate a Rails application the hard way, or the easy way.  If you want to do it the easy way, you can take advantage of the work Daniel Kehoe has done with RailsApps to generate your application with a number of pre-rolled goodies.  The name o four application is anydiver, and we are passing the -T option because we don't want to have to remove the test directory later, which we will not be using.  

`$ rails new anydiver -m https://raw.github.com/RailsApps/rails-composer/master/composer.rb -T`

RailsApps will then ask is a number of questions.  

    question  Install an example application?
        1)  I want to build my own application
        2)  rails-stripe-membership-saas
        3)  rails-prelaunch-signup
        4)  rails3-bootstrap-devise-cancan
        5)  rails3-devise-rspec-cucumber
        6)  rails3-mongoid-devise
        7)  rails3-mongoid-omniauth
        8)  rails3-subdomains
    railsapps  Enter your selection: 

We want to build an application on our own, rather than by using one of his pre-built templates.  And so when you reach the first prompt, enter '1'.

    railsapps  Enter your selection: 1

Following the same question/answer process outlined above, we will use Thin as our development and production server.  I've heard good things about unicorn in production on Heroku, but we can cross that bridge when we get to it.

    question  Web server for development?
          1)  WEBrick (default)
          2)  Thin
          3)  Unicorn
          4)  Puma
       setup  Enter your selection: 2
    question  Web server for production?
          1)  Same as development
          2)  Thin
          3)  Unicorn
          4)  Puma
       setup  Enter your selection: 2

Heroku recommends that you use PosgreSQL in development, and I'm sure they are correct.  But unless I run into problems using SQLite in development, I'm going to stick with it because I don't want this tutorial to be about installing PostgreSQL.

    question  Database used in development?
          1)  SQLite
          2)  PostgreSQL
          3)  MySQL
          4)  MongoDB
       setup  Enter your selection: 1

I'm using Haml becuse you can use a smaller number of lines of code, which makes navigating around your views easier.  Further it is much less ugly than ERB.       
    
    question  Template engine?
          1)  ERB
          2)  Haml
          3)  Slim (experimental)
       setup  Enter your selection: 2

RSpec is all I know, though I've heard good things about both Test::Unit and MiniTest:

    question  Unit testing?
          1)  Test::Unit
          2)  RSpec
          3)  MiniTest
       setup  Enter your selection: 2

I like the cucumber syntax -- or Gherken, actually -- but it annoys me when I have to have a features directory separate from a spec directory, which cucumber requires.  And so we will use turnip with capybara as a way to keep all of our tests -- unit, request, and acceptance -- in the same directory.  It will also allow us to cut down on the number of supporting files necessary for two different test frameworks. 

    question  Integration testing?
          1)  None
          2)  RSpec with Capybara
          3)  Cucumber with Capybara
          4)  Turnip with Capybara
          5)  MiniTest with Capybara
       setup  Enter your selection: 4

I've heard good things about fabrication, and their site is slick.  But the advantages I'm seeing of using the fabrication gem aren't big enough for me to stich over from factory girl at the moment.
    
    question  Fixture replacement?
          1)  None
          2)  Factory Girl
          3)  Machinist
          4)  Fabrication
       setup  Enter your selection: 2

I've used skeleton in the past, and liked it, but am going to go with twitter bootstrap because I know most people are already familiar with it.  We will be using it with Sass, so select that in the next prompt:
    
    question  Front-end framework?
          1)  None
          2)  Twitter Bootstrap
          3)  Zurb Foundation
          4)  Skeleton
          5)  Just normalize CSS for consistent styling
       setup  Enter your selection: 2

    question  Twitter Bootstrap version?
          1)  Twitter Bootstrap (Less)
          2)  Twitter Bootstrap (Sass)
       setup  Enter your selection: 2

We will be sending mail, and Heroku has a nice add on for sendgrid, so let's go that way:

    question  Add support for sending email?
          1)  None
          2)  Gmail
          3)  SMTP
          4)  SendGrid
          5)  Mandrill
       setup  Enter your selection: 4

The site will be using Devise for authentication.  Devise isn't perfect, but it does most of what I want it to do.  While it can sometimes be difficult to customize, and there is lots of good documentation for it.  We will be using the confirmable module, so select that as well:

    question  Authentication?
          1)  None
          2)  Devise
          3)  OmniAuth
       setup  Enter your selection: 2
    question  Devise modules?
          1)  Devise with default modules
          2)  Devise with Confirmable module
          3)  Devise with Confirmable and Invitable modules
       setup  Enter your selection: 2

The application will not have different user roles, so let's leave that out:

   question  Authorization?
          1)  None
          2)  CanCan with Rolify
       setup  Enter your selection: 1

SimpleForm is pretty slick, and I've been using it for the last year.

    question  Use a form builder gem?
          1)  None
          2)  SimpleForm
       setup  Enter your selection: 2

And lets get set up with a starter home page and user accounts:

    question  Install a starter app?
          1)  None
          2)  Home Page
          3)  Home Page, User Accounts
       setup  Enter your selection: 3

Now we can navigate into the application and see that it has generated a Gemfile for us



migrate our database, and start our server, after which we should be able to navigate to localhost:3000 and see something:

`$ cd anydiver`

`$ bundle exec rake db:migrate`

`$ rails s`

<img src="/assets/media/rails_composer_anydiver.png" width="800">

First migrate your database:

`$ bundle exec rake db:migrate`

Then create a pair of sample users:

`$ bundle exec rake db:seed`

Now when you navigate to [localhost:3000](localhost:3000) you should have a running site that will allow you to login or sign up.  Go ahead and play around with the site if you like.

Heroku
======

In order to push our application up to Heroku, we'll need to add postgres to the Gemfil in production.  Clean up your Gemfile so it looks like this:

    #Gemfile

    source 'https://rubygems.org'

    gem 'rails', '3.2.8'
    gem 'jquery-rails'
    gem "thin", ">= 1.5.0"
    gem "haml", ">= 3.1.7"
    gem "bootstrap-sass", ">= 2.1.0.1"
    gem "sendgrid", ">= 1.0.1"
    gem "devise", ">= 2.1.2"
    gem "simple_form", ">= 2.0.4"

    gem "therubyracer", ">= 0.10.2", :group => :assets, :platform => :ruby

    group :assets do
      gem 'sass-rails',   '~> 3.2.3'
      gem 'coffee-rails', '~> 3.2.1'
      gem 'uglifier', '>= 1.0.3'
    end

    group :development do 
      gem 'sqlite3'
      gem "haml-rails", ">= 0.3.5"
      gem "hpricot", ">= 0.8.6"
      gem "ruby_parser", ">= 2.3.1"
      gem "hub", ">= 1.10.2", :require => nil
    end

    group :test do 
      gem "turnip", ">= 1.0.0"
      gem "email_spec", ">= 1.2.1"
    end

    group :test, :development do 
    gem "rspec-rails", ">= 2.11.4"
    gem "factory_girl_rails", ">= 4.1.0" 
    end


    group :production do
      gem 'pg'
    end


heroku
======

run bundle to create a Gemfile.lock with the --without production tag

`$ bundle install --without production`

`$ git add .`

`$ git commit -am "configured application for heroku deploy"`

`$ heroku apps:create anydiver --stack cedar`

`$ git push heroku master`

If you have your own domain and want to get it hooped up to heroku:

`$ heroku addons:add zerigo_dns:basic`

`$ heroku domains:add anydiver.com`

`$ heroku domains:add www.anydiver.com`

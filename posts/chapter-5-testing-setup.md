---
title: "Chapter 5: Testing setup"
date: '2012-11-13'
description:
categories:
---

Books have been written on how to test, why to test, and the advantages of testing.  Some developers like to test, others do not.  Testing tends to be fairly popular in the Railsl community.  But I'm not dogmatic.  Sometimes it is appropriate to test, other times it is not.  For this application, however, I'm going to be using tests.  A lot of tests.  And I'm going to be using test first development because I believe it's easier to develop that way.  I also believe that for this particular application, which I know will require me to write code that prcesses incoming emails, not testing is, well, not an option.  The idea of having to create a new email address to test confirmation manually, or of having to manually send an email every time I want do do a number of things is not an appealing one.  

Why Turnip?
==========

Some people hate Cucumber.  One of those people is [David Heinemeier Hannson](http://37signals.com/svn/posts/3159-testing-like-the-tsa). Of those who don't hate it, many more believe it is nire useful for teams that combine programmers with non-programmers so that everyone on the team understands where things are going, rather than to a programmer working by herself.  Or at least that's what [Matt Wynne](http://blog.mattwynne.net/2012/11/02/is-cucumber-just-a-scam/) seems to be saying.  I like it quite a bit, or at least I like the gherkin syntax, and in any case since many of the cucumber steps will be using either rspec or capybara, the learning curve isn't that steep.  Where gherkin shines is anywhere you're using lots of capyabara.  Or someplace you're going to be hitting different models and controllers.  Perhaps the place I like to use it the most is when I don't have the time or desire to understand how the code inside a particular gem I'm using works.  The best example of this is Devise.  I don't really care how Devise works, only that other people have tested it.  Devise is kind of a black box for me.  I click buttons and do signup and it generates migrations and adds columns to my user tables and sends emails out for confirmations.  So we'll use cucumber on it.

Rspec
=====
Some people prefer Test::Unit.  I'm open to the idea that Test::Unit is a more compact and faster test framework than Rspec.  But I really don't care.  Rspec is well documented and that counts for a lot.

Shoulda
=======
I tend to write a lot of tests some of which, in retrospect, turn out not to be imporant, or even redundant.  I test for the existence of database columns, model validations, pretty much everything I can think of.  The danger of over testing is almost exclusively that of wasting time.  Specifically, while there is always some value to writing a test, that value may not always equal the amount of time you spend writing it.  Fortunately, a lot of tests I write share similar structure between different applications, and writing a test today doesn't take nearly as long as it did when I was, well, still trying to figure out how to write tests.  Some of this is achieved by memorizing the structure of tests, or having several projects up on Github I can consult to see what I tested on a different project, as well as how I tested it.  But also it helps that I've figured out a lot of shorthands.  And Shoulda helps me to write them even more quickly. 

Email Spec
==========
This is a terrific library for testing incoming and outgoing emails.  If all you need to do is test outgoing emails, this is probably a winner.  If you must also test incoming emails, then it may pay off to roll your own -- as we will be doing in this tutorial.




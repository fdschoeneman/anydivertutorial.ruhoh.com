---
title: "Chapter 1: Welcome to the Anydiver tutorial!"
date: '2012-10-29'
description:
categories:
tags: []

type: draft
---

Welcome to the Anydiver tutorial.  If you've already been through Michael Hartl's excellent [Rails Tutorial](http://ruby.railstutorial.org/), have bought into the concept of behavior driven development, and would like to bend your brain in a bit different direction, I hope to make this a good place for you.  I do this not because I pretend to be an expert Ruby on Rails developer, but because I want to become a better one:  Just as taking notes in class can help me remember the information in lectures, so I hope writing down the steps I'll be using to develop a simple but useful wine review website will help me to understand Rails better; and just as one of the best ways to learn something is to figure out how to teach it to someone else, so will figuring out how to simplify these steps for readers headed down this same path.

Another reason for writing this is that I want to use the tutorial as a way to motivate myself.  If you're anything like me you have plenty of half finished projects out there.  Something cooler came up, or you fell in love, or you became obsessed by Sudoku.  And then you forgot about the project.  And then you forget about Sudoku, and then you think back to the project, left unfinished, and you get down on yourself.  Perhaps you just got halfway into the project and decided it was dumb.  Or maybe you shared your idea with someone else who didn't understand what you were trying to do, or who did understnad it but still thought it was stupid, and you lost confidence.  Writing this tutorial is my way around the self-confidence trap.  I genuinely believe there is a place in the market for a wine review application, but until it is finished and in front of actual users, I will have doubts:  Will anyone ever use it?  Does it solve a real problem?  Am I wasting my time?  Why not take a consulting contract that offers a sure paycheck?  By writing a tutorial, I get out of this trap.  I get out of my vacuum.  I don't have to worry about whether or not this wine review website is viable because I've lowered the stakes.  I will have solidified my knowledge of some important things, and I will feel good about having help other Rails developers out.  And this wine review website turns out to be viable, and there are a few dozen Rails developers out there who want to launch a competing wine review website, well, let's just say that will be a nice problem for me to have. 


Why a wine review website?  

My family is in the wine industry, and I'm in and out of wineries with some frequency.  As a consequence I'm familiar with two groups of people.  The fist is people who consume wine, and the second is people who make it and sell it.  

It seems like lots of people are into wine, even some [rock stars](http://en.wikipedia.org/wiki/Caduceus_Cellars), and that they've registered all the cool domain names.  No matter.  Here's what I think you should be looking for in a domain name:

That's what the Internet Anagram Server is for.  And when I plugged in "vineyard," it spat out "any diver."  And since I used to be a diver, and because I spend far too much time thinking about things before doing them, I figure I may as well roll with it:  "anydiver.com."  Bam.  done.  I'm sure people will disagree with me on the name, but I think it meets at least three of the following four characteristics of a solid domain for a web property that I stole from Al Ries:

1) AnyDiver.com Cannot be mis-spelled.  "Any" is never spelled any other way, nor is "diver" ever spelled "dyver" or "deiver" or the atrocious 2005-sounding "divr."  And while I suppose it's possible that someone might type in "DiverAny.com," it's important to remember that even the best cowboys will lose the occasional dyslexic bull. 

2) AnyDiver.com is memorable.  Okay, so it's not actually memorable.  But neither is Facebook, and that hasn't stopped them from being awesome.

3) AnyDiver can be used in a sentence.  "Are you on anydiver, Jane?"  "Why yes I am, Ted."  

4) AnyDiver.com suggests the category of your business or web property.  Well okay, you have me there.  "Anydiver.com" doesn't suggest the category unless people know it is an anagram of vineyard.  So we'll see about that one.

The key is, come up with a name.  Any name.  Don't let the name stop you from moving forward.  If your idea is viable, you can change it later.  

And then I registered it on Godaddy.com using a coupon for $3.50.  You can find a coupon for yourself by googling for "godaddy coupons."  

In the next chapter we will set up a bare bones rails application and push it ot Heroku.  Please take a few moments to configure your domain registrar's name servers to zerigo -- we will be using the Zerigo add on for Heroku, and you can find instructions for that [here](https://devcenter.heroku.com/articles/zerigo_dns).

f 

ISince it sometimes takes a while for nameservers to propagate, go ahead and set them 

https://devcenter.heroku.com/articles/zerigo_dns

That's enough for today.

Tomorrow I'll go through the steps of deciding on which open source technologies I'll use, and why.
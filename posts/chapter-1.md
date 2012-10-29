---
title:
date: '2012-10-29'
description:
categories:
tags: []

type: draft
---

Welcome to the Anydiver tutorial.  The goal of this tutorial is to build on Michael Hartl's excellent [Rails Tutorial](http://ruby.railstutorial.org/).  If you've already been through that tutorial, understand the concept of test driven development, and would like to build something just slightly more complicated to push your brain in a different direction, I hope to make this a good place.  

I do this not because I pretend to be an expert Ruby on Rails developer, but because I want to become a better one.  Just as taking notes in class helps me to remember the information in them later, so I hope writing down the steps I'll be using to develop a simple but useful wine review website will help me to understand; and just as one of the best ways to learn something is to figure out how to teach it to someone else, so will figuring out how to simplify these steps for the benefit of different readers.

Another reason for writing this is that I believe the wine review website to help motivate myself.  If you're anything like me you have plenty of half finished projects out there because you lost motivation, or interest, or something cooler came up, and then you forgot about that even cooler thing and then you think back to the older project you still haven't finished, and you get down on yourself.  Perhaps you shared your idea with someone else who didn't understand, or who thought it was stupid, and then you lost confidence in yourself or in it.  Writing this tutorial is my way around the self-confidence trap.  I genuinely believe there is a place in the market for a wine review application, but until it is finished and in front of actual users, I will always have doubts.  Will anyone ever use this piece of crap?  Will it solve a real problem?  By writing a tutorial, I get out of this trap.  I don't have to worry about whether or not the application is viable.  If it isn't viable, I'll still have the tutorial, and will have learned a number of interesting things, and can feel good about helping other Rails developers out.  And if it is viable, well, let's just say that would be a nice problem to have. 


Why a wine review website?  

It seems like lots of people are into wine, even some [rock stars](http://en.wikipedia.org/wiki/Caduceus_Cellars), and that they've registered all the cool domain names.  And I don't like domain hacks.  No matter.  That's what the Internet Anagram Server is for.  And when I plugged in "vineyard," it spat out "any diver."  And since I used to be a diver, and because I spend far too much time thinking about things before doing them, I figure I may as well roll with it:  "anydiver.com."  Bam.  done.  I'm sure people will disagree with me on the name, but I think it meets at least three of the following four characteristics of a solid domain for a web property that I stole from Al Ries:

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
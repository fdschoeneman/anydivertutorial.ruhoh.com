---
title: "Chapter 1: Welcome!"
date: '2012-10-29'
description:
categories:
tags: []

<!-- type: draft -->
---

Welcome to the Anydiver tutorial.  If you've already been through Michael Hartl's excellent [Rails Tutorial](http://ruby.railstutorial.org/), have decided you'd like to use Rails for a new project of your own, have bought into the concept of behavior driven development, and would like to bend your brain in a bit different direction from where it otherwise might have gotten bent, this tutorial may be a good place to spend some time.  It is my hope that you will use some of the ideas found in it not just to build a toy Rails application, but to use Ruby on Rails to build your own business.  I do this not because I pretend to be an expert on either Ruby on Rails or entrepreneurship, but because I want to become a better at both.  Just as taking notes in class can help me remember information in lectures, so will writing down these steps imprint them in my mind; and just as one of the best ways to learn a thing is to force yourself to try to teach that thing, reformulating these concepts so they're clear and simple enough for an intelligent reader to understand will deepen my own understanding.

If you're anything like me you have plenty of unfinished projects out there.  Perhaps you just got halfway into the project and decided it was dumb.  Perhaps you shared your idea with someone else who didn't understand what you were trying to do.  Or worse, you shared it with someone who did understand what you were trying to do, but thought you were and idiot and your idea was stupid, and being a sensitive creature you lost confidence in yourself.  Perhaps something cooler came up -- like you fell in love, or you became obsessed by Sudoku, or you went all shut-in and lost your job because of Warcraft.  Whatever.  The point is that eventually you forgot about the project.  And then eventually you forgot about the cool new thing, too.  It broke your heart.  You reached bottom.  Whatever.  And you're sitting around feeling sorry for yourself, and thinking hey, your real problem is that you just don't fucking finish things.  Whoah.  Okay.  Sorry.  Please excuse the authorial intrusion.  But if you haven't guessed by now, writing this tutorial is my way around the self-confidence trap.  It's both a tutorial on how to build a business using Rails and a way for me to stay focused on building a business with Rails.  

### What kind of a web application will this tutorial show me how to build?
The we application will not be a toy.  It will be a real one, designed to solve real problems for real people.  It may not actually accomplish that, but that is the goal.


Like any good tutorial, the point isn't to teach you how to build a wine review business.  It's to help you think about the approach to building your own web business.  Of course, you are probably not interested in building a wine reviewing application.  You have ideas for your own applications.  There are many possible applications you might want to build.  But this is the one I want to build.  I hope some of the thought process I'm using to build it will prove useful to you.  

sketch outline of the application:

1. Users can find a wine on the website.  If it does not exist, they can enter it.  
2. Users assign the wine a rating from 1-100, as per the wine rating standard.
3. Users write a few sentences about the wine.
4. Once a review has been entered into the database, look at all the reviews and then publish a heat map or tag cloud showing which words are coming up most often in the reviews.

Customer validation: 

Fail.  Sophisticated wine drinkers who would like to write reviews are already using other wine reviewing applications like Cellar Tracker.


[<img src="{{urls.media}}/cd-process.png" width="600">](http://steveblank.com/)



### Lets think about users

winery owners
vineyard people
winemakers
tasting room folks

### Lets think about problems

__wine consumers:__

1. Would like a way to keep notes on wines they like.
2. Want to be able to compare their notes with others.
3. Want to be able to find wines with similar taste profiles.
4. Want to help winemakers they like
5. Want to feel like their opinion is important.
6. Do not want to be spammed by wineries.  Or, well, anyone.


__farmers or vineyard owners__

1. Farmers don't always have a good idea what happens to their fruit once they sell it to a winemaker.  Their client is the winemaker, not the consumer of the end product -- the wine.  That's fine for some bulk producers, but there are a lot of farmers who love to farm and love wine grape farming in particular.  They take pride in what they do.  They want the recognition.  And any good winemaker will acknowledge that they deserve it.  

1. Track reviews of wines made from their fruit
2. Feel all warm inside

__winemakers or vintners__

1. Want to sell wine.  Often make a substantial portion of their paycheck as commission on wine sales.
2. collect email addresses, and send marketing messages to their owners
3. Want to do direct sales (no distributor middleman)
4. Want to have the link for direct sales to show up higher in google page rankings
5. Want to be able to show reviews of their wines (social proof)
6. Want customers to understand their opinions really are important.


### Customer validation

Get out of the building.  Or, in my case, get out of the Starbucks.

winery owners want to stay in touch with people who like their wines, or who enjoyed the tasting experience in the tasting room


tasting room people want to be able to stay in touch with people who come to their tasting rooms.  They want to sell them wine.  They want to differentiate themselves from the bulk wineries, to be associated with a great day with a loved one, and the special feelings that come from doing that in the presence of wine.  

Thefeel awkward asking customers to sign a guest list or give up their email addresses


### Lets think about what a solution might look like

### Customer development

### Why Rails?

### Why TDD?

### Tools


And so I've chosen to develop an idea for a small web property I've been thinking about off and on for the last few years.  I've talked to a number of people about it, and enough of those conversations have been positive that I believe I may be solving a real problem for real people.  

  have also chosen to do tutorial in such a way that I genuinely believe there is a place in the market for a wine review application, but until it is finished and in front of actual users, I will have doubts:  Will anyone ever use it?  Does it solve a real problem?  Am I wasting my time?  Why not take a consulting contract that offers a sure paycheck?  By writing a tutorial, I get out of this trap.  I get out of my vacuum.  I don't have to worry about whether or not this wine review website is viable because I've lowered the stakes.  I will have solidified my knowledge of some important things, and I will feel good about having help other Rails developers out.  And this wine review website turns out to be viable, and there are a few dozen Rails developers out there who want to launch a competing wine review website, well, let's just say that will be a nice problem for me to have. 

### Why a wine review website?

As it happens, I have lots of friends who love wine, and lots of other friends who make and sell wine.  I'd like do something that helps both.  Always look for the win-win.  Here are some of the problems I've seen in the last few yearIn broad strokes, here is what I want the application to do:

When a wine fan goes into a wine tasting room

It seems like lots of people are into wine, even some [rock stars](http://en.wikipedia.org/wiki/Caduceus_Cellars), and that they've registered all the cool domain names.  No matter.  Here's what I think you should be looking for in a domain name:

That's what the Internet Anagram Server is for.  And when I plugged in "vineyard," it spat out "any diver."  And since I used to be a diver, and because I spend far too much time thinking about things before doing them, I figure I may as well roll with it:  "anydiver.com."  Bam.  done.  I'm sure people will disagree with me on the name, but I think it meets at least three of the following four characteristics of a solid domain for a web property that I stole from Al Ries:

1. AnyDiver.com Cannot be mis-spelled.  "Any" is never spelled any other way, nr is "diver" ever spelled "dyver" or "deiver" or the atrocious 2005-sounding "divr."  And while I suppose it's possible that someone might type in "DiverAny.com," it's important to remember that even the best cowboys will lose the occasional dyslexic bull. 

2. AnyDiver.com is memorable.  Okay, so it's not actually memorable.  But neither is Facebook, and that hasn't stopped them from being awesome.

3. AnyDiver can be used in a sentence.  "Are you on anydiver, Jane?"  "Why yes I am, Ted."  

4. AnyDiver.com suggests the category of your business or web property.  Well okay, you have me there.  "Anydiver.com" doesn't suggest the category unless people know it is an anagram of vineyard.  So we'll see about that one.

The key is, come up with a name.  Any name.  Don't let the name stop you from moving forward.  If your idea is viable, you can change it later.  

And then I registered it on Godaddy.com using a coupon for $3.50.  You can find a coupon for yourself by googling for "godaddy coupons."  

In the next chapter we will set up a bare bones rails application and push it ot Heroku.  Please take a few moments to configure your domain registrar's name servers to zerigo -- we will be using the Zerigo add on for Heroku, and you can find instructions for that [here](https://devcenter.heroku.com/articles/zerigo_dns).

f 

ISince it sometimes takes a while for nameservers to propagate, go ahead and set them 

https://devcenter.heroku.com/articles/zerigo_dns

That's enough for today.

Tomorrow I'll go through the steps of deciding on which open source technologies I'll use, and why.

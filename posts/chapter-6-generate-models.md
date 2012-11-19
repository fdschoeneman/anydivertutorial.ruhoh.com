---
title:
date: '2012-11-13'
description:
categories:
---


, sign up.  Sign up isn't normally too difficult, and I suppose we could do it without tests.  But requiring confirmation of a user's email address is a bit more complex.  And testing it manually isn't very fun.  We'd have to do it in development mode, and then go through the sign up steps and then in order to test confirmation we'd have to go into our development log to see if the confirmation email went out.  Instead we can write a feature which functions as a test, and should allow us to develop quicker:




To scaffold or not to scaffold?
-------------------------------

I know people say only newbies use scaffolds in Rails.  Well fuck them.  I like using scaffolds.  And I will not be made to feel inadequate for liking them.

`$ rails g scaffold wine vintage `
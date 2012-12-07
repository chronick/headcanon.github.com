---
layout: post
title: "Inserting the last command's argument into the current one in OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Just discovered this today.

Enter a command in OSX terminal with an argument, a good example to try is:<br />

      mkdir foo

then press ```option - . ``` (option-period). 

That will insert in the word 'foo', or the argument of the last command you just typed in. You'll have to make sure you have the 'use option as meta key' box checked in terminal settings. If you dont want to do that, you can also use Esc as a meta key.

This is especially useful if, like me, you want to execute a number of different commands with the same arguments but are pissed off at continually holding down the left arrow key or some other kind of keyboard gymnastics.
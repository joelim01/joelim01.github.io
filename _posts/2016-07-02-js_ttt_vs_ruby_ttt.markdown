---
layout: post
title:  "JS TTT vs Ruby TTT"
date:   2016-07-02 19:07:56 -0400
---


This post is more of a rambling record of a realization than anything else. I've been working on Ruby, SQL, ActiveRecord and Sinatra most recently in the Learn curriculum, so my head is pretty full of new information regarding databases, persisting data, passing values between scopes and all of that jazz.

Today my spouse, who is also learning to code (the family that codes together...?) asked for some help building (rubber ducking with feedback?) a visual "click" driven Tic Tac Toe JS project. Having already completed a similar assignment in Ruby, I agreed since I thought maybe I could be of some help.

We ended up spending almost an hour going through the code, refactoring, editing, and tinkering, but we were both having a hard time deciding how and when to pass the data around to the various functions that needed to check for a draw, a win, populate the board with x's and o's etc. Should there be board variables associated with "x" and "o" to track locations? Could the board hold information about which mark was occupying which space? How do you get to that data when you need to update it? How do you interact with it for computer player logic?

For me, after being in Ruby land with clear objects and associated reader/writer functions (attr_accessor, you're the tops), I was feeling a little untethered. I mean you don't just want to go wild and assign 20 global variables, do you?! That seemed a little crazy.

It took a while, and it seems obvious now, but in the case of this particular Tic Tac Toe game, we had to stop thinking about storing the data in JS variables to manipulate it and to populate the DOM. Once we started thinking about the page as the source of truth it became clear that the HTML elements, attribues and values were our data store, we just had to write the code that could write them and read them.

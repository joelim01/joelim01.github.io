---
layout: post
title:  "CLI Data Gem - Boutique Hotel Edition"
date:   2016-06-16 21:15:41 -0400
---

> In this lesson you're going to build a RubyGem that provides a CLI interface to an external data source. Your code will be packaged as a RubyGem and install a CLI for the user. The CLI will be composed of an Objected Oriented Ruby application.
> 
> REQUIREMENTS
> Package as a gem
> Provide a CLI on gem installation.
> CLI must provide data from an external source, whether scraped or via a public API.
> Data provided must go at least a level deep, generally by showing the user a list of available data and then being able to drill into a specific item.

Let’s just get right down to the nitty-gritty of this little gem, shall we? I’m going to ask myself four questions. 

*What did I do?*
*What should I keep doing?*
*What should I start doing?*
*What should I stop doing?*

Personally I find the first hardest to answer, which is why I already have an item under the Start heading. Start documenting during production. This is a lesson I’ve already learned, just a new context. Anyway…

**What did I do?
Tl;dr: Just read the bold.**

I looked at the example and code to try to understand what’s what.
I read through the rubygems guide, understanding maybe 50%.
I didn’t fully understand how to make a gem before starting, but I took educated guess that fully understanding wasn’t a prerequisite to getting started.
**I fumbled a bit on what data set to choose** (originally I had thought alternate side parking rules). I spent some time researching what was available (did you know you can get info via an app, an email list, a twitter account, and a mailing list in addition to 311?) only to find that the data set was flat. Just dates and associated holidays/events. Not sure what I was expecting…**but I didn’t dwell on it.** 
**I chose a dataset with a good structure.** It had 60+ items, it had multiple layers (categories and details). The categories provided a smart way to divide up the long list of items and serve them to the user without unnaturally paginating the results.
  **…and inconsistent data.** A lot of fields were occasionally empty or missing altogether when parsing the site with nokogiri. C’est la guerre.
**I tried to make a loose narrative out of the program:**
*   Scrape the site for categories -> Category Objects
*   Scrape the site for hotels in each category -> Hotel Objects
*   Assign Hotels to a Category, and a Category to a Hotel. -> Has many relationship
*   Scrape the site for Hotel details -> Array of details to be added to a Hotel object
*   Display welcome and Categories -> Start method
*   Ask the user for input for Category navigation -> Category Nav
*   Ask the user for input for Hotel Navigation -> Hotel Nav
*   Ask the user to continue? -> Repeat function

**I sketched the Category and Hotel objects** by scraping the site with nokogiri to find out what information was available and consistent enough to be meaningful.
Then I got the Category and Hotel objects fully fleshed out so the scraper populated them with data, and assigned them to one another (after a momentary lapse when I was just assigning the names and not the objects)
**From there it was CLI logic, styling, word wrapping. **This got pretty messy rather quickly.
**And gemifying!**  I’m not going to talk about my technical problems while building the gem. I’m still recovering. But I do feel way more confident about making gems now, so there is that.

Phew, if you’re still here, hold on, just a little more…

**What should I keep doing?**
Well, I think not getting hung up on what data to scrape for this exercise at least was good. I’m usually terrible when it comes to finding the “right” thing. I found a reason to like a particular data set and went from there. (The structure of the data really lent itself to CLI)
I think making a narrative helped me a lot. I got really lost at the beginning because I dove in without having a plan.  I took a step back and tried to give myself some structure with the narrative and some pseudocode.

**What should I start doing?**
Take notes while coding! I don’t know why I haven’t been taking good notes. I find random code scribbles on envelopes and the backs of magazines. This stuff is gold when it comes to ruminating on your own process.
Comment my code while writing it. Use it as a form of rubber-ducking to check myself.
Keep track of things that I get working but still puzzle me (I had a moment with send) or code snippets I could use again later.
Keep better track of places where I need to refactor.

**What should I stop doing?**
Real talk.
I should stop tinkering over and over with code that doesn’t work like I expect it to until it starts working. If I am in flow I need to leave it and come back and if I am not I need to isolate what is making the code act differently than I expect.  I always intend to come back to it but when I don’t I’m not doing myself any favors



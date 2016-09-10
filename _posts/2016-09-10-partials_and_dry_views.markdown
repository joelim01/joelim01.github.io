---
layout: post
title:  "Partials and DRY Views"
date:   2016-09-10 14:56:16 -0400
---

I've been working on my Learn.co Rails assessment on and off for a while now and when I started I felt like I had a shallow but working knowledge of nested forms, Action View, partials, collections and templates. It was a goal of mine to solidify these concepts through this project. One of the overarching themes I wanted to make sure I would tackle, as a way of forcing myself to work with these topics, was DRY (don't repeat yourself) code and shared partials.

Let's back up a bit... 
I decided to not re-invent the wheel but to re-use the domain from my Sinatra assessment (a supper club reservation system): 

Users can create, edit, and delete their own reservations for dinners as well as leave comments for dinners and their individual dishes.
Admins can CRUD all dinners, dishes, ingredients and reservations.

I made the decision early on to separate Admin functionality into its own controller. While it made it conceptually easier to work on the admin functions this way, it had the potential to introduce a lot of duplicate view code, which I felt was actually a good thing considering that I wanted to actively work on shared partials.

I am still new to Rails, but I figured at this point I had three options for my views:

1. Hardcoding all the HTML and Ruby in individual views: This would lead to a very WET app since the nested nature of the resources and two roles would just drown me in code. Not appealing!

2. Creating a few "master" partials: Lets say that I have a Dinner Index View showing all the dinners' dates and chefs. After each dinner, depending on the current user's role, the user would see a link to either Sign Up, Reserve, or Edit.

![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/9-10-16/Screen%20Shot%202016-09-10%20at%202.02.10%20PM.png)
![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/9-10-16/Screen%20Shot%202016-09-10%20at%202.04.19%20PM.png)
![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/9-10-16/Screen%20Shot%202016-09-10%20at%202.04.46%20PM.png)

Sounds like it could be reasonable to put that amount of logic in a view. Maybe. 

So, you might ask, *"Why not take that approach?"*

Well, take a gander at this...

![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/9-10-16/Screen%20Shot%202016-09-10%20at%202.17.27%20PM.png)

What happens when this partial wants to start moving around to other views in the application? Views that have their own logic about actions to display? Now we are starting to incorportate some pretty messy conditionals to determine which user role, which view, dinner dates in the future?, past?, already reserved? Yikes...

*"Ok, back up,"* you might say. *"You're forgetting about helper methods!"* Sure, it could go in a helper and we could just pass a flag to the partial to tell the partial which version to render:

`render partial: 'dinners', collection: @dinners, locals: {actions_for: **role from helper**}`

While this is a far better solution, what happens when we start nesting partials while rendering collections because we have nested associations? We'd have to pass ALL the local variables we need down the chain of partials from the first partial to the last. And cover all the possible versions in our partial. I'm not sure how you feel about this scenario (or how you might reimagine it in a much better iteration!), but as it stands I wasn't convinved.

*"So what's the last option?"* So glad you asked!

3. Rails is magical. Template Inheritance is pretty cool. From the rubyonrails.org guide: 

> Layout declarations cascade downward in the hierarchy, and more specific layout declarations always override more general ones. Similar to the Layout Inheritance logic, if a template or partial is not found in the conventional path, the controller will look for a template or partial to render in its inheritance chain.

Which means that you can let Rails handle at least some of the logic for you. 

Let's say we break out our actions into a separate partial called 'dinner_actions'. Let's also say that we divide up our actions according to the controller they'll serve. So in the previous example, the dinner/index dinner actions stay with the dinner/index.html file, and the reservations/index dinner actions stay with the reservations/index.html file. (If this is confusing, and it can be, look at the tree below):

![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/9-10-16/Screen%20Shot%202016-09-10%20at%203.08.12%20PM.png)

Step (1) We call our 'shared/dinners/dinners' partial from 'dinners/index' or 'reservations/index' as we would in the first example, but instead of packing all our logic into this controller, we instead include a call to render our dinner_actions partial:

`render partial: 'dinners_actions', locals: {dinners: dinners}`

Notice there is no leading directory structure and that we need to pass our collection or local value from our first partial to the second.  Step (2) Rails will look inside of 'shared/dinners' (the location of the partial folder), not find a matching partial, and then fall back one step in the heirarchy. This one step back is determined by the file calling the partial (so either 'dinners/' or 'reservations/') which means that we automagically don't have to worry about writing any logic related to which view the partial is in. Step (3) Rails will call the correct 'dinner_actions' just by 'failing' back a level in the search path.

Which I think is pretty nifty... don't you think?





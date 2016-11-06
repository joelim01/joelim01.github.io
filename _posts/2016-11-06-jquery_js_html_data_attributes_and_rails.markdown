---
layout: post
title:  "jQuery, JS, HTML data attributes, and Rails"
date:   2016-11-06 14:54:30 -0500
---

Rails with a jQuery/JS Front End Project Post:

First, let me admit to jumping into adding some jQuery to my Rails Dinner Party app (see my last post for details!) without planning thoroughly. I've been around the block often enough to know that spending a little time upfront can save you a lot of headache later on... 

Well, being excited to dig in, I did not save myself any headaches. I did learn a lot, though, so I thought I'd break down what could trip you up if you don't think things through.

Now that you're combining HTML, Rails, and JS, you've got a good bit going on. Three different languages at least (not to mention some CSS, jQuery, and whatever else you might be throwing in there). 

You're just adding a few *small features* to an already complete app. Do you really need standalone .js files in your asset pipeline? Why not use a script tag? That sounds harmless...

It could be tempting. There is no question where the JS lives that is attached to a specific view which is nice for you later, and nice to whomever might be in the code after you. You can also write some nice dynamic JS functions by evaluating <%= %> variables inline. 

While I found myself starting with inline scripts I eventually pulled my JS out into separate files. Why did I do this? First as my JS grew (and grow it did) bouncing between three languages got pretty ugly.

DISCLAIMER: PLEASE DON'T DO THIS! THIS IS UGLY!!!
![BadCode](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/11-6-2016/Screen%20Shot%202016-11-06%20at%202.55.56%20PM.png)

And very soon after I started to suspect that my code was the source of the not so pleasant odor in the room, I also realized that in order to make this lovely script dynamic, I had placed the code in a partial... and it was repeated at least 10 times in the HTML of the page. Yeah. Nope. Time to take out the garbage.

After a little thought it was actually quite easy to clean this up. First, give the HTML element a little intelligence about what it refers to with a data attribute:

`<%= link_to "See reservations", {}, :data => { :dinnId => dinners.id } %>`

This allowed me to do something like this:

```
function viewRes(event) {
    event.preventDefault();
    var dinnerId = event.target.dataset.dinnid
    var promise = getReservation(dinnerId);
```

Which pulls the ID directly from the event. A good start! Note that retreiving the value of a data attribute requires `target.dataset` and not `target.data()` which is the function that lets you set the attribute.

Next you just need to attach your function...

![Far more satisfying](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/11-6-2016/Screen%20Shot%202016-11-06%20at%203.09.54%20PM.png)

That's a lot more satisfying, isn't it?

 












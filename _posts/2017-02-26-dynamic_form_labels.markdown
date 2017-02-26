---
layout: post
title:  "Dynamic Form Labels"
date:   2017-02-26 10:30:39 -0500
---


I'm working on the first iteration of an app and while I can't get specific about the exact cases involved, I've got an interesting technical problem. The app will be used by multiple groups of people who all have different terms for the same thing (think form inputs). So, for instance, you might call the person who oversees the work you do a boss, a supervisor, or a superior depending on which group you belong to. 

Of course, those words are all pretty closely related to one another and could be fairly easily deciphered if you encountered a term that didn't match what you were expecting to see. But when terms start to drift further apart or when two separate form fields use terms that are similar but subtly different, you start to encounter a good amount of confusion. 

The request, then, is that each group's administrator can edit the name of the form fields in place so that every member of their group can see the label that they will be expecting.

Initially I was skeptical that I could achieve this with any sort of efficiency. I was thinking about it as a data model problem and since I was hoping to use Postgres and didn't want to stray into the land of non-relational databases (also, while a non-relational DB could maybe have solved the problem, it seems like a terrible reason to use one) I needed to figure out another solution. 

Thinking about it further, it became clear that this was actually a front-end problem and not a backend problem as long as the fields in the models always mapped 1-to-1. The solution I am currently working out in my head looks a bit like this:

1. The form fields and the database model needs to map 1-to-1. You can't store something related to the color of an object in the form field related to shape. If you try, you're going to have a bad time. I need to figure out if it is possible to prevent someone from trying to do this somehow...
2. The names in the database model will be a generic, used to access data in the DB.
3. There will be a separate data object that will contain the customized names of the fields. The keys will match the generic names of the DB models. The values will contain a default name that can be customized.

So, we'd have something like this:

```
person = {name: "Margaret", least_favorite_thing: "squeeky doors", favorite_band: "The Doors."}
person_labels = {name: "Name", least_favorite_thing: "Pet Peeve", favorite_band: "Rocks Out To"}
```

Then we'd need to associate the person_labels with the group that they belong to, and make sure that whenever a person who belongs to that group is looking at the form, they see the custom labels.

It might not be the most elegant solution, but it seems like it will work and that it can be used in multiple front end contexts, which is the current goal.

While it could be possible to hard-code these terms into the front end for each group, new groups might be added in the future, or groups might alter their terminology. The requirement I have been given is for flexibility and I am searching for the best way to provide it.


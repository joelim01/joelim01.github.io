---
layout: post
title:  "Linked Lists"
date:   2017-02-05 01:23:09 +0000
---


Data structure!

I've recently encountered several code challenge problems referring to linked lists. Unlike arrays, which (depending on your language implementation) are static data structures, linked lists store an ordered set of data and are dynamic.

Linked lists can be singly-linked or doubly-linked.  
In a singly-linked list, you'll typically have access to the head node, which contains a reference to the second node, which contains a reference to the third, and so on and so forth until the last node which does not point to a subsequent node.
A doubly-linked list has the same structure, but it also contains links from the end of the list back to the head, allowing traversal in both directions.

One of the problems I encountered involved determining if a linked list contained a cycle, or, a place where a node in the list refers to a node that occurs before it in the list, causing an infinite loop while iterating through the list. 

You might be tempted to set some sort of limit to the number of steps taken through the list but since linked lists can contain an arbitrarily large number of elements (and just by looking at the head, one cannot know how many) this is not a viable solution.

We could brute force it by storing all values in the linked list and checking to see if we've encountered a particular node in the past but that would eat up quite a bit off additional storage.

So how can we accomplish this in the most efficient manner...

Well, if two trains were to travel on a track, one leaving just after the other, with the lead train running twice as fast as the trailing train... what would happen?

If the trains were on a track with a beginning and end, the lead train would simply reach the end, but if there was a loop... collision!

So we send two iterators down the loop... one traveleing one item at a time, and another traveling two items at a time (item.next and item.next.next). If the faster traveler reaches the end or overlaps the slower traveler, (fast.value === slow.value) then you know you've got a cycle.

While encountering linked lists in Ruby feels pretty rare, these types of questions are actually pretty fun to solve, simply from a problem solving perspective.

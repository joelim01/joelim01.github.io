---
layout: post
title:  "Binary Trees"
date:   2017-01-27 13:33:55 -0500
---

Data structure!

This week while brushing up on CS concepts I learned about a new data structure... the Binary Tree. 

Your vanilla Binary Tree has some properties that define it.

1) The Binary Tree is composed of nodes. Each node has a value and points to at most two other nodes, a left and right node. It can point to 1 or 0 nodes as well.

So, something like this:

```
root = { value: 10, left: {value: 3, left: {value: 5, left: nil, right: nil}, right: nil}, right: {value: 6, left: nil, right: nil}}
```

looks a bit like this:

<img src="https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/1-27-17/BinaryTree.png" alt="BinaryTree" style="width: 300px;"/>

2) There is a single head node, called the root.  
*This tree is unsorted.*

Once you sort the tree, things get a little more interesting. Here's a sorted and balanced tree:

```
root = { value: 6, left: {value: 4, left: {value: 3, left: nil, right: nil}, right: {value: 5, left: nil, right: nil}}, right: {value: 10, left: {value: 7, left: nil, right: nil}, right: {value: 13, left: nil, right: nil}}}
```

<img src="https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/1-27-17/BalancedSortedBinary.png" alt="BinaryTree" style="width: 300px;"/>

The rules here:  
1) Each node may have 0,1 or 2 child nodes.  
2) Nodes with values less than the root value are placed to the left and values greater than root are placed on the right.  
3) Balance is maintained by keeping the height of the tree (the depth of the leaves) within the same level, or no more than 1 level apart. 

As you might imagine, this speeds up searching for values in the sorted tree over a brute force sequential array search ie:

```
array = [some unsorted array of integers]
val = some_integer

def findVal(val, array)
  for i in (0..array.length-1)
    return true if array[i]==val
  end
  return false
end
```

One coding challenge I recently had to complete required determining if a tree was a Binary Search Tree (a sorted tree (meeting the requirement where left child nodes are less than parent nodes and right nodes are greater than parent nodes) where ALL values to the right of the root node are greater than the root node and ALL values to the left of the root node ares less.

That's a mouthful. Here is an example of a sorted but non-BST tree...

<img src="https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/1-27-17/NotBST.png" alt="BinaryTree" style="width: 300px;"/>

The 2 left-leaf node (leaves are nodes which have no children) doesn't violate the rule that because it is on the left it must be less than its parent but since it falls to the right side of the root, it must be greater than 6. Nope. Not a BST.

Armed with this, we can come up with an interative strategy for determining if a tree is a BST:


![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/1-27-17/IterativeBSTcheck.png)

And finally a recursive one...

![](https://dl.dropboxusercontent.com/u/455813290/Blog%20Images/1-27-17/recBST.png)

There's so much to learn about data structures but perhaps the most important thing I learned was that, at least in Ruby, BST's are actually much slower than Ruby's built in Hash... so while you can nerd out with me, there's probably not a whole lot of cases when you'll want to implement it in the vanilla fashion.


				






---
layout: post
title: "How Can I Work With HTML Collections?"
date: 2017-07-20 08:52:36 -0400
comments: true
categories: HTML Vanilla Javascript Array Iterate
---

## What are HTML Collections?

In vanilla javascript, when selecting elements from the DOM, either through `document.getElementById`, `document.getElementsByClassName`, `document.getElementsByName`, `document.getElementsByTagName`, or `document.getElementsByTagNameNS` the return value is an HTML collection. This is important because even though it looks like an array, it does not have the same prototypical methods. Also, and more importantly they are live DOM objects, meaning that if you modify them, you are directly modifying the DOM.

## What do we get for free?

First off, the ability to modify the DOM directly is very useful but also a great place for bugs to slip in, so be careful in how you use it.

We also get access to one property and a several methods:
1. HTML collections have a 'length' property that can be called with `collection.length`, and it returns the number of elements in the collection.
2. HTML collections have an 'item()' method that can be called with `collection.item(index number of the item)`, and it returns the element at that index. If the index doesn't exist, it will return 'null'.
3. HTML collections have a 'namedItem()' method that can be called with `collection.namedItem(id or name)`, and it returns the element in the collection with the corresponding 'id' or if that doesn't find anything, with the corresponding 'name.'

## What don't we get for free?

array methods
how to iterate?
how to convert into an array
difference between item and bracket notation (null vs. undefined)

## Conclusion

A question that has come up a few times for me while working with vanilla javascript is when to work with an HTML collection as a collection vs. converting it to an array and working with it that way. I think this comes down to two considerations. First, is your goal to modify the DOM for some or all of the elements in the collection? If so, working with it as a collection seems more direct. Second, if what you need to do can be accomplished in a simple 'for loop', then great, no need to convert it to an array. Otherwise, converting it to an array will probably save more headaches than the line of code necessary to convert it.

Good luck and happy coding!

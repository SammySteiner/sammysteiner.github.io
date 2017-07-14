---
layout: post
title: "Listening to Javascript: Adding Events"
date: 2017-07-14 15:03:14 -0400
comments: true
categories: Javascript addEventListener Code Challenge React
---

## Getting good at listening

This seems to keep coming up in code challenges so I wanted to write about it. Usually this is a piece of a larger challenge, but sometimes these questions are really basic. Sometimes these questions get weird, like in a totally real interview I had:

Interviewer: I want a button to create an alert on the screen.
Me: Ok. Does this button submit a form, and is that what the alert is for? Should the alert display a message depending on where the user clicked or the state of the page?
Interviewer: No.
Me: Ok, great. I've got my laptop right here, I can do this in the console on a live webpage, maybe wikipedia.
( I've already got the wikipedia page for Dolphins open and I'm pressing 'option' + 'command' + 'j' )
Interviewer: No. On paper. And with all the HTML for a webpage, with the header and body information.
Me: Ok, ( a little deflated )

So let's review how to add an event listeners in javascript and see if we can find some more depth here too. Read to the end for some bonus event listener things with React.js.

## Adding event listeners

We need a few things in order to add an event listener. First, what do we want to attach the listener to? In vanilla javascript, we can user our `document.getElementBy...` functions to select a specific tag, class, name, or id. Second, we'll need to know what kind of event to listen to, ie. a 'click', 'submit', 'mouseover', or something else. There's a great list of all the events [here](https://www.w3schools.com/jsref/dom_obj_event.asp). Finally, we need a function. In the interview mentioned above, it was a simple alert message, however, we can do a lot here. We can even call named functions. Ok, the real last thing is whether we want our event propagation to capture or bubble, we'll get into that a bit later.

Ok, we're ready to start coding this up.

## It's console time

Let's add some listeners to the wikipedia page for [Dolphins](https://en.wikipedia.org/wiki/Dolphin), they're super smart animals and we should be listening to them.

Open up the console and type:
```
document.getElementById('mw-content-text').addEventListener('click', function(){alert('hi')})
```
Now click any of the interesting dolphin information. Yay, we get an alert with our message.

Note: if we try and remove the event listener with `document.getElementById('mw-content-text').removeEventListener('click', function(){alert('hi')})
`, that won't work because the remove event listener method only works with named functions. Our alert function is an anonymous function. But don't worry! If you want to get rid of that event handler, you just need to refresh the page.


## Who's up first: propagation

If we add the following listeners to our page, what do you think will happen when we click a paragraph?

```
document.getElementById('mw-content-text').addEventListener('click', function(){alert('hi')}, true)
document.getElementsByClassName('mw-body')[0].addEventListener('click', function(){alert('hello')}, true)
document.getElementById('mw-content-text').addEventListener('click', function(){alert('why, hello there')}, false)
document.getElementsByClassName('mw-body')[0].addEventListener('click', function(){alert('salutations')}, false)
```
When you click you'll see:
1. 'hello'
2. 'hi'
3. 'why, hello there'
4. 'salutations'

The reason for this is that events  bubble by default, which means we start with the most deeply nested element's event, and go up the chain from there. When we set the capture variable to 'true', we instead start with the outermost DOM element and work our way down. We also see from this experiment that events that are captured take precedent over those that aren't. You can experiment by implementing these listeners in different orders, but the outcome will be the same.

## Alternatives ( and why not to use them )

You can also do this inline in your HTML element with an 'onclick' variable set to a function. This is not ideal as it could take your website longer to render, which would negatively impact the user experience.

Alternatively, you can add an inline listener in javascript with:
```
document.getElementById('mw-content-text').onclick = function(){alert('this works!')}
```

However, this will overwrite any other listeners written this way and this method doesn't allow you to control the propagation. Bonus question: if you add this to the dolphins page, what will the new order be?

## We want to see the HTML

For closure, this is what a simple html page would look like if it just had a button that created an alert:

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Code Challenge</title>
  </head>
  <body>
    <button type='btn' id='alert'>Alert Button</button>
  <script>
    document.getElementById('alert').addEventListener('click', function(event) { alert('hello') } )
  </script>
  </body>
</html>
```

This was pretty basic, but there are much more fun code challenges out there. One I tried recently that was tricky and fun is this one from [Trello](https://taco-spolsky.github.io/). Have fun and happy coding.

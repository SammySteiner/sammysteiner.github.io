---
layout: post
title: "Dynamic Forms in React: Part 2"
date: 2017-07-05 11:14:22 -0400
comments: true
categories: Javascript React Nested Dynamic Forms
---

## Recap Part 1

In [Part 1]('https://sammysteiner.github.io/blog/2017/07/02/dynamic-forms-in-react/'), we talked about simple forms in React. Namely, we talked about controlling inputs, updating state, and handling submissions. All of that assumes our form is static and we know the shape of our state. 'Shape' in this context means the data contained in the state object, the keys, the types of values, etc.

In part two, we're going to talk about a more complex form. This form needs a way to update itself, and because we want to control the inputs, the form needs a way to update the state. Finally, when we send off our state object to an API, we need to know how to deconstruct that state regardless of how many phases and actions a user added to our form.

Here's our gif from Part 1 to demonstrate the kind of form interaction we're going for:
![Dynamic form gif](/assets/goalForm.gif)

## Designing the Form UX

When writing code in react, I find it helpful to start with the design I want to implement. Starting from the design makes it really easy for me to think through the components I want to build, now to nest them, and what props they'll need to work with.

The first part of the form i pretty standard. Each goal should have a title and a description. I find it really helpful to think about the description as the motivation behind the goal. ie. "I want to go to the gym twice a week to get in shape for the summer." or "I want to go get ready to run a half marathon by September." I find it's really helpful to break down big goals into manageable bits of work, and give myself a set amount of time to finish my task. Deadlines are my friend!

This tells me I want a button in my form to add a "phase". A phase should have a timeframe, a number of days within which I need to complete this task, and a button to create actions. Each time I press this button, I need a new input for a description of an action. So we are creating dynamic inputs inside dynamically created inputs in a form with controlled inputs. That was a pretty scary problem to start with. But I knew my next step and it seemed like a fun thought challenge. How do I model this data?

## Modeling the Data



## Sudo Code for the Form

## Coding it all up

## Conclusion

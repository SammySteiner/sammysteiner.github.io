---
layout: post
title: "Dynamic Forms in React: Part 2"
date: 2017-07-05 11:14:22 -0400
comments: true
categories: Javascript React Nested Dynamic Forms
---

## Recap of Part 1

In [Part 1](https://sammysteiner.github.io/blog/2017/07/02/dynamic-forms-in-react/), we talked about simple forms in React. Namely, we talked about controlling inputs, updating state, and handling submissions. All of that assumes our form is static and we know the shape of our state. 'Shape' in this context means the data contained in the state object, the keys, the types of values, etc.

In part two, we're going to talk about a more complex form. This form needs a way to update itself, and because we want to control the inputs, the form needs a way to update the state. Finally, when we send off our state object to an API, we need to know how to deconstruct that state regardless of how many phases and actions a user added to our form.

Here's our gif from Part 1 to demonstrate the kind of form interaction we're going for:
![Dynamic form gif](/assets/goalForm.gif)

## Designing the Form UX

When writing code in react, I find it helpful to start with the design I want to implement. Starting from the design makes it really easy for me to think through the components I want to build, now to nest them, and what props they'll need to work with.

The first part of the form i pretty standard. Each goal should have a title and a description. I find it really helpful to think about the description as the motivation behind the goal. ie. "I want to go to the gym twice a week to get in shape for the summer." or "I want to go get ready to run a half marathon by September." I find it's really helpful to break down big goals into manageable bits of work, and give myself a set amount of time to finish my task. Deadlines are my friend!

This tells me I want a button in my form to add a "phase". A phase should have a timeframe, a number of days within which I need to complete this task, and a button to create actions. Each time I press this button, I need a new input for a description of an action. So we are creating dynamic inputs inside dynamically created inputs in a form with controlled inputs. That was a pretty scary problem to start with. But I knew my next step and it seemed like a fun thought challenge. How do I model this data?

## Modeling the Data

The process of defining my state took a couple iterations to determine the best way to structure my data so it would play nicely with my database and wouldn't require more looping than necessary. I started with what was going to be static, since that was pretty simple.

```
  state = {
    title: '',
    description: '',
    repeat: false,
    // Some way to show all our goals
  }
```

Great that's three quarters of our state defined! We're 75% done with this.

We're going to want to have a list of goals, in order so we know which comes first, second, etc. That means we want an array of goals. Each element of our goal array is going to need a phase length and a list of actions. That means each element of our goal array is going to be an object with two keys, "phase" and "actions". Actions is going to be a list of descriptions, that sounds like an array.

Writing that out gives us this:

```
goals: [
  {
    phase: '',
    actions: []
  }
]
```

When we start filling this up with goals, our state will eventually look like this:

```
  state ={
    title: as a string,
    description: as a text area,
    repeat: true of false boolean,
    goals: [
      {
        phase: length as a number,
        actions: [
          description for action 1, description for action 2, etc.
        ]
      },
      {
        phase: length as a number,
        actions: [
          description for action 1, description for action 2, etc.
        ]
      },
      etc.
    ]
  }
```

Ok, that looks reasonable! Now that we know what we want the state to look like, and how it will be modified at each step of our user's journey through the form, we can start thinking through the code to make that happen.

## Sudo Code for the Form

We're just going to sudo code this for the blog because the actual code gets very long, and a bit hairy because I decided to put it inside a modal. If you want to check out the actual code, you can find a link to my repo [here]('https://github.com/SammySteiner/habits-client').

```
  <form onSubmit={send our form data to the API}>
    <input type='text' name='title' value={this.state.title} onChange={update the state with this change}/>
    <input type='text' name='description' value={this.state.title} onChange={update the state with this change}/>
    <input type='textArea' name='repeat' value={this.state.description} onChange={update the state with this change}/>
    <input type='checkbox' checked={this.state.repeat} onChange={update the state with this change}/>
    <button type='button' onClick={ call our add phase function }>Add Phase</button>
    <input type='submit' value='submit' name='Create Goal'/>
    < render all the Goal components based on the goals in the state >
  </form>

  addPhaseFunction(){
    copy the state
    add a new element to the goals array with the object we described above
  }
```

Then we need to import a goal component that maps over each element of the goals array and displays a phase input that is controlled by the value of the phase key in the object of the goals array. We also need to pass that input a function to update the phase in the state on change.

Additionally our goal component needs to show a button similar to the "Add Phase" button, but this one will say Add action. It only needs to update the state by pushing an empty string onto the actions array in the  object.

Finally, our goal component needs to map over the action components and display in input for each of them. It then needs to use that element in the actions array to control the state and pass that input a function to update that state.

This should update our state dynamically, add goal objects to our goals array, add action descriptions to the actions array of it's associated goals in the goals array. Our form component will keep updating every time the state is set by showing the new components with the new buttons and inputs they are mapping over. And Voila, it works!

## Conclusion

There are some pretty cool things you can do with react. This form was a really fun challenge, and I'm really proud of what I built and I learned a lot in the process. If you want to check out the app in action, you can find it [here]('habits-sammy-steiner.herokuapp.com') hosted on heroku for free, so I apologize for the loading time.

Thanks for checking it out!

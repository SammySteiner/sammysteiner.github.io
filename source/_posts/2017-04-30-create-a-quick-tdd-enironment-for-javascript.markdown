---
layout: post
title: "Create a quick TDD environment for javascript"
date: 2017-04-30 20:32:44 -0400
comments: true
categories: "Flatiron School TDD Test Driven Development Javascript Mocha Jasmine"
---

## Getting started
Our goal is to get a project up and running quickly. If you haven't done this before, first we want to create a project directory, create a github repository, and then link the two. Here are the basic steps:

1. Navigate to your development folder in Terminal.
2. Type `mkdir "project-name"` in terminal to create a project folder.
3. Type `cd "project-name"` in Terminal to navigate into your project directory.
4. Type `git init` in Terminal to set up your directory as a git repository.
5. Create a readme file by typing `touch README.md` in Terminal.
6. Add and commit your new repository by typing `git add .` and  `git commit -m "first commit"`.
7. To create a github repository, login to [github](https://github.com) in your browser and navigate to [github/new](https://github.com/new) to create a new repository.
8. Once there, give your repository a name, use the same one as your project directory, and leave the "Initialize this repository with a README" option unchecked.
9. Click "Create Repository".
10. Back in Terminal, type `git remote add origin git@github.com:your-git-username/your-repository-name.git` to set up the connection between the folder on your computer, and the repository in github.
11. Then type `git push -u origin master` to push the files, ie. your readme, from your project folder to github.

Now that your project is up and running, it's time to create a testing environment.

## Jasmine versus Mocha
There are two very popular testing libraries for Javascript, Jasmine and Mocha. Both are great, and weighing the pros and cons of each is enough for another blog post. Some people have already written great articles on the topic here:

1. [Kevin Groat](http://www.techtalkdc.com/which-javascript-test-library-should-you-use-qunit-vs-jasmine-vs-mocha/)
2. [Marco Franssen](https://marcofranssen.nl/jasmine-vs-mocha/)

I'm more familiar with Mocha, so we're going to go with that.

## Setting up Mocha
First we need to create our NPM dependencies, with a package.json file. There is a great walkthrough built in for us by typing `npm init` in Terminal. Just follow the promts and include a name, repo, and some other metadata in the form. That will create a package.json file in your project folder. Don't forget to add/commit your changes frequently.

Create a mocha dependency in your project package.json file by typing `npm install --save mocha` in Terminal. Create a test file with `atom test/test.js`. I'm using Atom, but you can replace that with your editor of choice.

This is where it can get pretty hairy. There is a lot to install to get up and running. I would advise starting off with a simple test and some simple code just to have something to test against. We also need to require the files with our code and assign it to a variable, as well as require any other mocha assertion libraries you want to use. My basic test.js setup looks like this:

In test.js:
```ruby
var expect = require('expect.js');
var myCode = require('../index.js')

describe ('test', function() {
  it('returns "Hello, world!" as a string', function() {
    expect(myCode.test()).to.be("Hello, world!")
  })
})

## All other tests below
```

In terminal: `npm install --save expect.js`

Now we're ready to start writing our functions. If you want to build out this application, you can move all your inclusions and exporting responsibilities to a root.js file in your test folder. That will allow you to write your functions normally and branch out to other .js files without things getting to messy. But more on that in another blog.

to pass our first test, add the following code to index.js:
```ruby
var test = function() {
  return "Hello, world!"
}
```

But we're still not passing our test yet. With mocha, it's not enough to just include it in the test.js file, we need to export them from our index.js file as well. We do this with some general code at the bottom of our file that we update with each additional function. Here's what we need at the bottom of index.js to export our first test:

```ruby
if(typeof exports !== 'undefined') {
    exports.test = test
}
```

Now you should be passing your first test. Hurray! Now that you're up and running. The [mocha documentation](https://mochajs.org) on how to write more complex tests is a great resource.

---
layout: post
title: "What's going on with Find, Map, and Collect in Ruby"
date: 2017-03-21 16:21:57 -0400
comments: true
categories: "Flatiron School Ruby Find Collect Map Array Methods"
---

## When should we use each vs. map or hash on Ruby Arrays?

If you're like me, you've done some weird things with the _each_ method in Ruby arrays. One thing I used to do was create a new array in my method, iterate over the array passed in to the method as an argument and shovel the return value into my new array. That is a useful code smell to look out for when refactoring. But that is unnecessary since we have the _map_ and _collect_ methods! I didn't understand how to use those methods, when to use them instead of _each_, and what they would return.

FYI for the rest of the post, I'm going to use "_map_" instead of _map_ and _collect_ because _map_ and _collect_ are identical. That is, they do the exact same thing. The only difference is that "map" is 3 characters to type but "collect" is more descriptive as to what’s going. You can think of it like we're collecting our new array elements and returning that collection as an array.

If you're trying to do all that with _each_ but adding on some _if_ statements to determine what you're shoveling, you should look at some other methods like _find_, _select_, and many others!

## Yield is awesome!

First, lets talk about what these iterator methods are doing. In order to dig into that, we need to discuss the _yield_ keyword. _Yield_ is a way to pass a block of code into an method with the arguments. For example, to demonstrate how _yield_ interrupts a method to call on a separate block:

```ruby
def interrupt_this_method(number)
	puts "thing one"
	puts yield(number)
	puts "thing two"
end

interrupt_this_method(3) {|i| i * 2}

#=>
‘thing one’
6
‘thing two’
```
The _yield_ stops the method from putting the second statement until it has a chance to call whatever argument we passed into the method with the arguments in the block on the argument we pass into _yield_. We can use _yield_ with a looping method like _while_ to emulate our _each_ and _map_ methods to get under the hood.

## What is going on with Each?

Now we’ll write our own _each_ method, without using “_each_.” Aside from giving us some insight into what’s actually happening with our _each_ method, it will show us how much time our higher level methods are saving us.

```ruby
def generic_each_array_method_with_iterator(array)
  i = 0
  while i < array.length
    yield array[i]
        i = i + 1
  end
  array
end
```

So far, we have a generic method that takes an array as an argument, as well as a block of code, sets the variable i to 0 and operates our code on each element of our array, adding 1 to the i variable, between each iteration, and stopping the _yield_ when we’ve hit all the elements. We then return the original array that was passed in. We can use this to pass any block into our method with any array. Let’s give it a try:

```ruby
generic_each_array_method_with_iterator([1, 2, 3, 4, 5]) {|num| puts num * 2}
#=>
2
4
6
8
10
[1,2,3,4,5]
```

That’s exactly what we get when we hard code the block into our method using _each_.

```ruby
def times_two(array)
  array.each {|num| puts num * 2}
end

times_two([1, 2, 3, 4, 5])
#=>
2
4
6
8
10
[1,2,3,4,5]
```

Pretty cool, right! The _each_ method allows us to user the elements in our array, and return the original array. As you can see, the _each_ method is really easy to write, but a lot less flexible than the _yield_ method. That is fine because we don’t want to keep writing code strings to pass in with our arguments every time we want to use a method.

## What's going on with Map?

Let’s take a look at how _map_ works. This is our generic _map_ method that relies on _yield_, instead of the explicitly calling on _map_.

```ruby
def generic_map_array_method_with_iterator(array)
  new_array = []
  i = 0
  while i < array.length
    new_array << yield(array[i])
        i = i + 1
  end
  new_array
end
```

The difference here, from our generic _each_ method, is that we’re creating a new array at the start of our method. Then, we shovel the results of our yielded block into the new array instead of just executing those blocks. At the end of the method, we return the new_array instead of the original array. So for the following argument and block this is what we’ll get:

```ruby
generic_map_array_method_with_iterator([1,2,3,4,5]) {|num| num * 2}
#=>  [2, 4, 6, 8, 10]
```
That’s exactly what we get when we hard code our block into a map/collect method

```ruby
def multiply_array_elements_by_two(array)
  array.map {|num| num * 2}
end

multiply_array_elements_by_two([1,2,3,4,5])
#=> [2, 4, 6, 8, 10]
```

Sweet!

### TL;DR

_Map_ and _collect_ are identical methods. If you want to perform some action on variables, call other methods depending on our array elements, etc., use _each_. If you want to return a new array based on some manipulation of your array, use _map_.

Happy coding!

---
layout: post
title: "Building a Game: Generate Unique and Random X and Y Coordinates"
date: 2017-06-04 12:39:18 -0400
comments: true
categories: Flatiron School Ruby Rails Algorithm
---

## Why would I need some random coordinates?

I want to build a game. This game is going to have a dungeon, represented by tiles on a game board, and it will be randomly populated with a door, a key, a bunch of monsters and the player's starting position. My database schema is expecting an 'x' and 'y' coordinates for each of these, but to make my game work, I need to make sure that none of these start off on the same square. It wouldn't be very challenging if the key and door could appear on the same spot. It wouldn't be fun if the player spawned on the same tile as a monster and lost the game immediately! There are lots of reasons you might want to generate a number of unique 'x' and 'y' coordinates for a grid of a given size, this would work for all of them.

## What do I want my result to look like?

We're going to start simple and build out more complexity as we get things working. As I said, I need a coordinate for the door, a key, the player and some monsters, that's a minimum of four coordinates and we need a board that is two squares by two squares to fit everything. In this case, I'm using all the possible coordinates, so I want my final outcome to look something like this hash of info:
```
game_coordinates = {
  player: {x: 1, y: 1},
  door: {x:1, y:2},
  key: {x:2, y:1},
  monster: [
  {x: 2, y: 2}
  ]
}
```

Let's zoom in on the first problem we want to solve. Getting ourselves the list of unique 'x' and 'y' coordinates. This sounds like we want to use an array, which we would ultimately need to iterate over to assign values. For now, our array should look like this:
```
unique_coordinates = [[1,1], [1,2], [2,1], [2,2]]
```

We will use this array later to assign the coordinates to the door, key, player, and monster. Let's break down what we are seeing. This isn't one simple array, it's an array of arrays. Each of the nested arrays represents a unique set of 'x' and 'y' coordinates. We can also have two arrays for the x and the y values:
```
x_values = [1, 1, 2, 2]
y_values = [1, 1, 2, 2]
```

If we look a little closer, the arrays shouldn't both be in order like that. In our unique_coordinates array, they 'x' values repeat then increment while the 'y' values increment then repeat. So we want values that look like this:
```
x_values = [1, 1, 2, 2]
y_values = [1, 2, 1, 2]
```

## Let's hard code this to get started

Now that we have the basic building blocks for our data structure, let's put it together assuming we have the unique values we need. In our two by two grid, with the x_values and y_values arrays, all we need to do is a simple loop to get our unique_coordinates array, and pull off four coordinates at random:
```Ruby
unique_coordinates_generator =
x_values.each_with_index.map do |x, index|
  [x, y_values[index]]
end.sample(4)
# >>> [[1,1], [1,2], [2,1], [2,2]]
```

Perfect! Just note that if you're following along, your coordinates may be in a different order, that's exactly what we want. They should all be unique.

To assign each of these to our game board objects, we can do this by assigning each a value from the unique_coordinates_generator. This isn't the prettiest way to do this, but it will work.
```Ruby
player = unique_coordinates_generator[0]
door = unique_coordinates_generator[1]
key = unique_coordinates_generator[2]
monsters = [ unique_coordinates_generator[3] ]
```

this looks great. The next step is to abstract this to handle grids of any size, with more than four coordinates to accommodate multiple monsters.

## Abstraction

To state our problem here more simply, we want to create our x_values and y_values arrays for grids larger than two by two. That means, the size of the grid would need to be an argument. Additionally we can infer from the fact that we need to accommodate a variable number of monsters that the number of monsters will also be an argument. So the skeleton of our code should look something like this:
```Ruby
def coord_generator(grid_size, number_of_monsters)
  # some code that creates:
  # x values from 1 to the grid size, where we get a 1 the grid_size number of times, followed by a 2 the grid_size number of times, etc.
  # y values that count from 1 to the grid_size number for the grid_size number of times.
  # then we map over the arrays like we did before.
  # finally we sample the array with the number of monsters
end
```

Now that we've stubbed that out in sudo code. It is really easy to translate that to code using some simple ruby operators:

```Ruby
def coord_generator(grid_size, number_of_monsters)
    x_values = ((1..grid_size).to_a * grid_size).sort()
    y_values = ((1..grid_size).to_a * grid_size)
    unique_coordinates = x_values.each_with_index.map do |x, index|
      [x, y_values[index]]
    end
    unique_coordinates.sample(number_of_monsters)
  end
```

## Conclusion

We've done it! We started with a scary sounding requirement, "generate a number of random and unique 'x' and 'y' coordinates for a grid of a given size." We brainstormed about how we wanted the data to look and we divided it into simpler and simpler parts. We broke that down into basic procedural tasks with a hard coded example. Then we abstracted away each of the hard coded parts with information from the requirements, until we produced a general and powerful piece of code.

Good luck and happy coding!

---
layout: post
title: "Bringing Terminal Applications to Life: CLI animations with Ruby"
date: 2017-04-13 17:46:02 -0400
comments: true
categories: "Flatiron School Ruby CLI Terminal Animation ASCII"
---

## Animation in the Terminal?!

While working on a command line application, with [Scott Harrison](https://github.com/szrharrison), we decided to add some flair with an animation in the terminal. While looking for inspiration, and to see if it is even possible, we found this amazing, and very long example. Just type `telnet towel.blinkenlights.nl` into your terminal to connect with a telnet server that broadcasts Star Wars Episode IV!

The project we were working on was pulling data from  [NYC Open Data](https://data.cityofnewyork.us/Social-Services/NYC-Wi-Fi-Hotspot-Locations/a9we-mtpn), specifically from the database for free public wifi locations, and using a google maps gem for rails, called geocoder, to help find the closest free wifi access point. While looking for a suitable animation, we found this on [gify](https://giphy.com/gifs/jordan-hill-wifi-qzczxN15LFGY8/), and we knew we had to make it happen.

![Wifi clip from GIPHY](../images/giphy.gif)

Aside for the fun of it, why would you want to run an animation in a terminal application? We found our application hit some delays when looking for the user's IP address and while hitting the google maps api. A neat animation works as a loading screen, while that happens in the background. That isn't what we did, but that would be a great project for refactoring.

## The plan

Here's a checklist; we'll go into detail below:

1. Find a clip
2. Extract individual frames
3. Convert each frame to ASCII
4. Add the folder with your slides to your project
5. Build an iterator method to loop through the frames

## Building your files

First, you'll need a clip that you want to animate, it can be saved as a gif, or even in a video format. We downloaded ours from the link above and it is formatted as an mp4 file. Then, you'll need to extract the frames from your clip. There is a nifty terminal tool to do this, called [ffmpeg](https://ffmpeg.org/about.html), that you can read more about. You'll need to make some decisions about where to start stop and stop your extraction, and many frames you want per second. Our command looked something like this: `ffmpeg -i wifinder_animation.mov -r 5 image-%02d.jpeg`.

After that it gets a bit tedious, you have to convert each image to ASCII art. Thankfully there are lots of free tools online that will do this for you. If we were dealing with a longer clip, we would have looked for a way to convert the entire folder in one go, but we just uploaded each frame to [this site](http://www.text-image.com/convert/ascii.html). Pro tip: name your files based on where you want your clip to start and stop, starting with something like "1" and counting up, so you can easily iterate through the images.

Then just add the folder with your beautiful ASCII art frames to your repo and get ready to build an animation!

## Building an animation

To get started, we're going to write an animation method that will loop through our images in the animation folder one time.

```Ruby
def animation
  i = 1
  while i < 60
    print "\033[2J"
    File.foreach("ascii_animation/#{i}.rb") { |f| puts f }
    sleep(0.03)
    i += 1
  end
end
```

First create a variable and set it to 1, then we'll start our iteration. For a 60 frame animation, we'll run our loop while our variable is less than 60. Then we'll clear the window by moving the cursor to the top right of the terminal window with `print "\033[2J"`. Then we'll loop through our animation folder, named "ascii_animation" in this example, using our variable to indicate the file name, 1.rb, 2.rb, 3.rb, etc, printing each line of our image to the terminal. Then we'll let the image stay on screen for .03 seconds 'sleep(0.03)'. Finally we'll increment our iterator and begin the loop again.

If we want to run our animation multiple times, we can just put a loop method before our method call, like this: `3.times { animation }`

Finally, if you want to freeze frame your animation somewhere in the middle, instead of the potentially boring last frame in your file, you can start another loop, this time ending at your target frame.

```Ruby
def animation_with_a_great_final_frame
  2.times do
    i = 1
    while i < 60
      print "\033[2J"
      File.foreach("ascii_animation/#{i}.rb") { |f| puts f }
      sleep(0.03)
      i += 1
    end
  end
  i = 1
  while i < 30
    print "\033[2J"
    File.foreach("ascii_animation/#{i}.rb") { |f| puts f }
    sleep(0.03)
    i += 1
  end
end
```

That's it! Now you can enjoy our sweet CLI wifi animation.

## Showtime!

![WiFinder gif](../images/wifinder_animation.gif)

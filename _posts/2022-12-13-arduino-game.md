---
title: The classic Simon game on the Arduino Uno
toc: true
toc_sticky: true
excerpt: Or, what I thought would be a simple game turned out to be quite the hassle.
header:
    overlay_image: assets/images/posts/arduino-game/header.webp
    overlay_filter: linear-gradient(rgba(50, 50, 50, 0.7), rgba(50, 100, 200, 0.7))
short: arduino-game

gallery1: 
  - image_path: assets/images/posts/arduino-game/gallery_1_1.webp
    url: assets/images/posts/arduino-game/gallery_1_1.png
    title: "New circuit design"
  - image_path: assets/images/posts/arduino-game/gallery_1_2.webp
    url: assets/images/posts/arduino-game/gallery_1_2.png
    title: "Circuit schematic"

gallery2: 
  - image_path: assets/images/posts/arduino-game/gallery_2_1.webp
    url: assets/images/posts/arduino-game/gallery_2_1.png
    title: "New circuit design"
  - image_path: assets/images/posts/arduino-game/gallery_2_2.webp
    url: assets/images/posts/arduino-game/gallery_2_2.png
    title: "Circuit schematic"
---
# The premise
For one of my classes, I had a group project where we had to design a prototype of a toy to help children with motor disabilities. And the toy that we decided to create was...

The **Light-a-phone**! Or, a xylophone that would light up for each key that you hit. Along with the xylophone was a game that would test your memory skills by having you repeat the pattern that the xylophone plays. 

{% include picture.html img="1" ext="jpg" alt="A sketch of the Light-a-phone: A rectangle with 5 smaller rectangles within it." caption="Sketch of the Light-a-phone" %}


Seems fairly simple, right?

# Technical details
Figuring out the components that we needed to make this device and putting them together was fairly simple. Our inputs would be signals from buttons, and outputs would be LEDs and a passive buzzer to play the corresonding note. 

We attached different resistors to the buttons, and used a single analog pin to measure the output from the buttons &mdash; the different resistances meant that each button would give a unique reading from the analog pin.

{% include gallery id="gallery1" layout="half" %}

What actually turned out to be the most frustrating part of this project was programming the game.

# The game
The logic behind the game was fairly simple:
```
1. Generate a random pattern of n notes.
2. For i from 1 to n:
    a. Play i notes of the pattern.
    b. Get player input of up to i notes.
    c. If the input doesn't match the pattern, the player loses.
3. If the player hasn't lost, the player has won.
```
The tricky part ended up being 2b &mdash; **getting the buttons that the player pressed**.

## First frustration
The readings from the analog pin weren't always constant. In fact, each button almost always gave off a range of signals. Okay, fair enough. A simple fix would be to allow a range of values when checking if the button pressed matches the key in the pattern, which worked for a bit.

```c++
// Example of range of values:
928
930
929
919
930
929
...
```

But then, something strange started happening. There were times when I would reupload the code to the Arduino, and suddenly the range of signals that each button gave off would change. To this day, I am still not sure why this happened, but it was probably due to some loose wire connections. Regardless, I just increased the range of accepted values and called it a day.

## More button shenanigans
Another issue with the buttons was that one press of the button did not just result in sending one value to the analog pin. Rather, in the time that a person would press and release the button, there would be 10 or more readings from the analog pin for that button. 

{% include picture.html img="2" ext="png" alt="Serial monitor showing when the button is pressed or not." caption="The number of readings in less than a second." %}

This was an issue because I had to check whether the button pressed matched the note in the pattern, and if a button was still being pressed, it would still technically be correct, but the program would think it should have been the next note in the pattern.

A little annoying, but not too hard to fix. The program would just have to ignore any button inputs that were in the same range as the previous input. 

<small>(To account for the case that would break this where a pattern would have the same note back to back, I changed the pattern generation so that each note was different from the previous.)</small>

## Press and Release
Then I realized that the buttons would sometimes produce values in between 0 and their normal value whenever they were just beginning to be pressed/released. 

{% include picture.html img="3" ext="png" alt="Button analog values." caption="Analog readings of a button when it's pressed/released." %}

Again, not too bad to fix. The program would just have to ignore any analog values that were not within the range of each button (and I would have to hope that these in-between values didn't fall within other buttons' ranges).

# The result...
And after all of this, the Light-a-phone only worked about half of the time. ☹

Since we didn't want to just hope that it would work whenever we let the class test our prototype, we decided to change the design: **No more analog inputs.** We were just going to connect each button to a digital pin.

{% include gallery id="gallery2" layout="half" %}

# Final Product
Behold, the Light-a-phone:

{% include video id="v3gg4H-HAEo" provider="youtube" %}

And when you win the game, there's a special surprise:

{% include video id="nAz0GBkVrkQ" provider="youtube" %}

[Here's the source code.](https://github.com/i-am-mai/Lightaphone)
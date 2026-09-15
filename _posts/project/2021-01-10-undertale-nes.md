---
layout: post
title: Undertale for the NES
published: false
---
The date on the post is when the project was completed in 2021, but this post is being written in 2026.

{% youtube "https://www.youtube.com/watch?v=YcA3_L7Kndo" %}

[Undertale for NES cart download]({{ site.url }}/assets/downloads/undertale-nes/napstablook.nes)

# Intro

Back during lock-down I became interested retro game development and ended up making a little demo, a fight from the indie-game [Undertale](https://undertale.com/) remade (or "de-made") to work on the NES.

It’s not a huge project, but I had fun learning 6502 assembler and the quirks of the NES.

# Resources

I used [this project](https://github.com/gregkrsak/first_nes) as a basis, it provided the setup for compiling a NES cartridge and some of the basics which are the same for every game (clearing memory, getting controller input, etc.).

Fortunately, an NES compatible version of the boss fight music had already been created by [VinylCheese](https://youtu.be/l0ciHXyXu_0) and the enemy sprite is from the original game. To play the music, I used an existing driver which - honestly - I have no idea where I got it from, and probably there are better options nowadays so don't take this list of resources as instructional.

For learning resources, the [Undertale Wiki](https://undertale.wiki/w/Napstablook/In_battle) provided information on how the fight worked mechanically (but I also played through it many times to figure out the exact effects of every action). 

The [NES Dev Wiki](https://www.nesdev.org/wiki/Nesdev_Wiki) was my primary resource NES development information.

I used [Mesen](https://www.mesen.ca/), which is supposed to be the most accurate NES emulator and has useful debugging tools.

# Entering the Matrix

<img class="inline-image" src="/assets/images/thematrix034.jpg">
<div class="inline-caption">No UI/UX developers in the apocalypse apparently...</div>

A program called CC65 is used to compile 6502 assembly, it is also capable of compiling C code, but I wanted to use assembly language as a learning experience. Though retro software development is often charcterised as more "hardcore", it seems that in reality complexity has expanded in both directions. Higher level and easier languages become available, but at the same time, the lower levels have become more complicated, and the "stack" as a whole has become taller. 6502 assembly is closer to the hardware, but because the hardware is less complex than modern hardware I think it works out to about the same difficulty as something like C. The experience reminds me of graphics programming: there's more friction, and I had to adapt to a new mental model than what I use for C-like languages.

# State Machine Architecture

To approximate object oriented programming on the more limited platform, I created two state machines. First, a state machine for the overall game state, essentially what menu is currently open. Second, a state machine for each sprite, which includes functionality for player movement and Napstablook's tear attack.
To implement a state machine in 6502 assembly, a jump table is used. There is a static list of 2 byte addresses to each state function. The [RTS trick](https://www.nesdev.org/wiki/RTS_Trick) to jump program execution to the current states function.

# Graphics

<img class="inline-image" src="/assets/images/charmap.png">
<div class="inline-caption">The CHR ROM</div>

All the graphics fit in one sprite sheet (correct term?)
256 characters
4 colours
Palette swapping
Potential for reduction

# Conclusion

Pretty cool
Will not work on this further because conceptually it is actually too feasible
Comparted to something like halo for the 2600, there is a lot less appeal
---
layout: post
title: Heldritch Rush
tag: highlight
image: /assets/images/thumb_heldritch.png
---

In July 2025, I was invited to join the Sloth Doctor Games to work on Heldritch Rush. My contract with Sumo Digital was coming to an end and I was entering my final year of university. To me, Heldritch Rush was an opportunity to work in a team and continue developing my Unreal Engine 5 and C++ skills.

Heldritch Rush is a top-down on-rails shooter. It had been a successful [DARE](https://www.abertay.ac.uk/academic-faculties-and-graduate-school/faculty-of-design-informatics-and-business/dare-academy/) game, and now Sloth Doctor Games were rebuilding it and expanding its scope for a commercial release.

For more information about the game see [the Steam page](https://store.steampowered.com/app/5035910/Heldritch_Rush/)

I parted ways with the project in September 2026, so I'm writing the article as an account of the technical work I did while working on it.

## Spline Rail System

The first system I developed for the game was a spline base movement. My work was based on a prototype by [Charlie Wake](https://www.linkedin.com/in/charlie-wake-6338142bb/), I rebuilt it from the ground up.

The requirements were roughly as follows: 
- The designer must be able to control the players path through the level
- The designer must be able to specify where "events" happen as the player progresses through the level
- It must be possible for the player to take multiple paths

("Events" refers to camera changes, enemy spawning for example)

Taking advantage of the built-in spline component in Unreal was a given, but it only provides foundational spline editing functionality.
We must consider how to make the player move along the spline, I think there are two methods: 
- manipulating the players position directly to slide them along.
- using pathfinding to navigate from point to point.

The method used in the original Heldritch Rush DARE game was 

## Weapons System

## Level Visibility

Room cutout
Hiding floors

## Input and User Interface

Common UI
Input rebinding

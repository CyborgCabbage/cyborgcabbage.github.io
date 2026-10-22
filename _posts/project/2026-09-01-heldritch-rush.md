---
layout: post
title: Heldritch Rush
tag: highlight
image: /assets/images/thumb_heldritch.png
sortby: b
---

{% youtube "https://www.youtube.com/watch?v=evr0UuN-tJI" %}

In July 2025, I was invited to join Sloth Doctor Games to work on Heldritch Rush. My contract with Sumo Digital was coming to an end and I was entering my final year of university. To me, Heldritch Rush was an opportunity to work in a team and continue developing my Unreal Engine 5 and C++ skills.

Heldritch Rush is a top-down on-rails shooter. It had been a successful [DARE](https://www.abertay.ac.uk/academic-faculties-and-graduate-school/faculty-of-design-informatics-and-business/dare-academy/) game, and now Sloth Doctor Games were rebuilding it and expanding its scope for a commercial release.

Check out Heldritch Rush on [Steam](https://store.steampowered.com/app/5035910/Heldritch_Rush/)

I parted ways with the project in September 2026, so I'm writing the article as an account of the technical work I did while working on the Heldritch Rush team.

## Spline Rail System

The first system I developed for the game was spline base movement (in an on rails shooter, the rail is kind of important). My work was based on a prototype by [Charlie Wake](https://www.linkedin.com/in/charlie-wake-6338142bb/), I rebuilt it from the ground up.

The requirements were roughly as follows: 
- The designer must be able to control the players path through the level
- The designer must be able to specify where "events" happen as the player progresses through the level
- It must be possible for the player to take multiple paths

("Events" refers to camera changes or enemy spawning for example)

### Rail Actor

Taking advantage of the built-in spline component in Unreal was a given, but it only provides foundational spline editing functionality.
I had to consider how to make the player move along the spline, I think there are two methods: 
- manipulating the players position directly to slide them along.
- using pathfinding to navigate from point to point.

The method used in the original Heldritch Rush DARE game (I believe) was interpolating from point to point. 
I elected to use pathfinding for the following reasons:
- it is more in-line with how character movement conventionally works, and therefore is well-trodden ground in the engine and naturally integrates with physics and animation systems
- the designer doesn't have to be overly precise with how they place spline points, because with pathfinding the player is garunteed to walk along the ground naturally

Fortunately, Unreal provides a PathFollowingComponent which can be added to the player controller, making it quite easy to implement pathfinding based movement.

The biggest issue is that the pathfinding may fail, perhaps a physics object or enemy blocks the path when they shouldn't and the player can't progress. In these situations, the player is automatically teleported to the next point. It is a matter of level design and play testing to make sure these teleports aren't needed.

### Rail Point Actor

The spline movement prototype had all the events in one array on the rail actor, but I thought this would not be ergonomic to use for levels of the size we wanted to make. So, I added rail point actors to divide events based on which spline point they occur at.

Rail points are attached to the rail actor, each spline point can have one rail point. The rail actor automatically moves rail points to their spline point, where-ever it is in the level. Rail points have a billboard sprite to make them easy to select in the editor and also use a text component to list all their events so a designer can understand the event flow of the level at a glance.

(the rail point sprite and text components are only included editor builds 😉)

Another important feature for usablity is that rail points don't need to be adjusted when spline points are added or removed. This was possible by using spline metadata to adjust the indices of the rail points automatically. I used [this guide](https://mintbanjo.com/articles/unreal-engine-5-spline-point-metadata/) to help with the spline metadata setup.

### Events

So, each rail point has a list of events but how do these work?

I defined a parent class for all events, it has the functions:
- OnRailEventBegin(...)
- OnRailEventTick(...)
- OnRailEventEnd(...)

(each of these can be overwritten in Blueprints, allowing new events to be scripted easily)

When the player gets to a rail point, the player stops and processes the events in order. "Tick" is only called on the currently processed event.

For events with a duration, they can call the protected function "EndRailEvent()". For events that are supposed to have no duration, there is a flag which when enabled causes an event to end instantly. When dealing with instant events, the system has will actually process multiple in the same tick. This is necessary to prevent the player from stopping briefly when they are not supposed to.
An example series of events:
- begin camera movement
- start voice clip
- spawn enemy wave
- wait for 1 second
In one tick, the first two events will executed and the enemies will be spawned. The wave event ends when all enemies have been killed, there would then be 1 second delay before the player moves on.

{::comment}

## Weapons System

## Level Visibility

### Room Cutout

### Hiding Floors

Overlap volumes are used to define which actor 

## Input and User Interface

Common UI
Input rebinding

## Play Testing
{:/comment}
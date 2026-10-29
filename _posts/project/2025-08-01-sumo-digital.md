---
layout: post
tag: highlight
title: Sumo Digital Placement (1 year)
image: /assets/images/thumb_sumo.png
sortby: a
---

[ensure]: {% post_url /blog/2026-10-23-ensure %}

In August 2024, I embarked on a one-year work placement at Sumo Digital Sheffield. While at Sumo, I worked on two unannounced Unreal Engine projects. 

Reflecting on my time at Sumo, I am very grateful for the opportunity I was given when placements like these are so scarce in the industry. I got experience in AAA game development processes and was able to develop my skills in Unreal Engine, gameplay programming, writing documentation, systems programming, UI programming... everything basically! I got to collaborate with skilled people from across disciplines, and I started thinking more about what my work could enable others to do.

## Production

Across both projects, I worked with Sumo's production tools: Perforce (version control), Jira (task management), Swarm (code review), Unreal Game Sync (CI/CD). It took me a bit to adjust to code review, but I like coding standards and writing clean code (anything to reduce the infinite possible implementation options) so by the end of my placement I was managing to write stuff that usually required no changes.

## Project D5
{% include image.html url="/assets/images/project_d5_header.jpg" description="Header from the public Project D5 Steam page" %}

I worked on Project D5 from August to December 2024. At the time it was unannounced, but fortunately it has since gone through [public beta testing](https://store.steampowered.com/app/3734070/Project_D5/), so I feel more able to talk about it in detail.

During my time working on Project D5 I was on the Game Modes team. Project D5 is a party game and so supports a variety of game modes.

I worked on multiple game modes. After I had become familiar the production processes and shown I could complete programming tasks without needing much oversight, I was given "ownership" over the one of the game modes which meant I was responsible for implementing all bug fixes and changes needed for it. Later I took on another, when a change to the narrative required all the menus and game mode logic to be adjusted.

I contributed bug fixes, UI implementations, and gameplay tweaks where-ever they were needed under the purview of the game modes team. For example: distributing scores to players, changing how teams were assigned, and how the game mode transitions from through its various phases (e.g. countdown, gameplay, next round, flipping teams, etc). I also implemented new gameplay actors that were needed.

I fixed an issue with incorrect trigger box behaviour which had been affecting every trigger box in the game for a while. Sumo has an internal library of UE utilities shared across projects, the trigger box implementation in this library was sometimes keeping actors in its internal overlapping actor list when they shouldn't be and this caused inconsistencies in "OnBeginOverlap" and "OnEndOverlap" events.

{% include image.html url="/assets/images/project_d5.jpg" description="Screenshot from the public Project D5 Steam page" %}

## Unannounced Project

I was given the opportunity to move to a different project in the new year, and I went for it because I wanted to get as wide a breadth of experience as I could while on my placement.

On the new (to me) project, I joined the 3Cs team (character, camera and control). 
My first responsibilities were implementing debug features for the character, including flying, teleporting, and logging inputs and character state into the Rewind Debugger. 
After that, the remainder of my time on the project was focused on player movement and combat abilities. 
Implementing new abilities (in both categories), fixing bugs, adding settings for designers and hooks for sound effects.

I was also tasked with creating a system for enabling and disabling player abilities, so they could be gradually unlocked during the tutorial. The design I went with was a World Subsystem which stored a list of capabilities as Gameplay Tags. It was automatically initialised with a list of default capabilities for the level (stored in the World Settings) and they could be further added or removed using trigger boxes. Being a subsystem, any game system can check what capabilities were enabled easily. I like this design because it is mostly just "dumb" data storage; to add new capabilities, you just create a new gameplay tag.

## Reflection

I found that I was able to rise to the challenges I encountered at Sumo, but there are still some things I need to work on if I want to advance in my career.

Something I intend to do differently in the future is take more initiative. While at Sumo, I really focused on completing Jira tasks. Broadly, staying focused is a good thing, but if I want to grow as a games programmer I need to have a bit more independence. An example of something I was thinking about and could have taken initiative on is [the assert situation in Unreal][ensure].

Another point I've been thinking a lot about is note taking, I did not make any personal record of what I was doing while at Sumo so now it is difficult to remember many of the details. Writing this page is in-part practice for taking better notes at my next position. [Tim Cain](https://www.youtube.com/watch?v=P0NKR9R0XTg) (the creator of Fallout) took lots of notes throughout his career and is able to tell so many stories, I really want to be in that position in the future.
---
layout: post
title: Braise Satan
image: /assets/images/thumb_braise_satan.jpg
---
{% youtube "https://www.youtube.com/watch?v=NZhl5vbofhI" %}

[Braise Satan on Steam](https://store.steampowered.com/app/3295990/Braise_Satan/)

In the Abertay game development courses there is a large interdisciplinary project in 3rd year; teams of 8 with members of each discipline collaborate to make a game over the course of the term.

# Game Concept

Each team is connected with a real game studio which gives them requirements for the game to be developed. We worked with [Team Terrible](https://www.teamterriblegames.com/) who asked us to make a "Job simulator with a twist" (a job simulator is game like Papers Please or Power Wash Simulator). 

After brain-storming we decided to create a cooking game set in hell. The gameplay would be similar to Overcooked, however single-player, from a first person perspective, and with combat against enemies mixed in. After some further brain-storming we settled on the name "Braise Satan" as a pun on "praise Satan" and cooking technique "braising".

# Initial Work

This was the first time I had used Unreal Engine 5 and worked in team for an extended period of time, so I learned a lot about how to collaborate. In terms of programming, I worked in both C++ and Blueprints. Some things I was the primary contributor for were the cooking mechanics, the recipe system, and scoring.

{% include image.html url="/assets/images/braise_satan_early_build.png" description="An early build, with basic cooking mechanics and imp behaviour implemented" %}

I implemented an object interaction system using an interface and the Physics Handle Component. Objects can also be snapped together, which is implemented using a custom Lock Point Component. When an object is released while overlapping a Lock Point it becomes attached to the lock point in a parent child relationship.

{% include image.html url="/assets/images/braise_satan_interaction.png" description="The advanced emergent gameplay possible with physics based item interaction" %}

As I worked on the item interaction and cooking, the other programmer Aaron implemented the Imp AI (it spawns, steals an ingredient, and then runs away). Getting art assets into the engine isn't a priority at this point, but Finn and Stuart were able to design and model the Imp (respectively) pretty quick so we got that in.

# Final Build

After a lot of work over the term, we finished the game! It has three rounds, at the beginning of each round you get given a card with a recipe: first is fries, then eyeballs on toast, and finally an Imp-meat burger. There was no big single thing I worked on, it was just a bunch of small things adding up: UI, input, animation blueprints, etc.

{% include video.html url="/assets/videos/braise_satan_burger.mp4" description="Putting together a burger (poorly)" %}

Of course, you can also really see the difference getting all the art in has made. I'm still really impressed with the art people on our team, they're all linked at the bottom of the page.

{% include video.html url="/assets/videos/braise_satan_mince.mp4" description="Get minced idiot" %}

Some little features I like:
- Once lava has covered the floor, you can still play and finish the course by jumping around on the counter tops
- You can punch and kick while looking at your recipe, like a distracted driver
- To turn the music off, you throw the gramophone into the lava
- Because items on Lock Points are snapped together, you can just put ingredients on the pans hanging on the wall (I consider this a feature, not a bug).

# DARE Academy

Our team decided to take our project further and participate in the DARE Academy program. In DARE, you work mostly full-time for a month on your game and then at the end are judged based on various criteria; some games are created from scratch during DARE but you can also use it to "update" an existing game. Our focuses were making a better tutorial, redesigning the level (with a unique model for the devil), and adding some new ingredients (the stupid little slug, as seen in the trailer).

{% include image.html url="/assets/images/braise_satan_hell.png" description="New level overview" %}

We (programmers) struggled quite a bit with getting everything done, the new level required gates and trigger boxes to control the players movement, and we also wanted to control how many ingredients the player could take down stairs. I also had to implement automatic restocking for the pantry, to stop the player from running out of ingredients and getting soft-locked (which was possible in the previous build). 

We were able to finish, but I think we underestimated the compounding effect of our objectives: adding features, adding a better tutorial, improving the polish. The features you add have to fit into the tutorial, and polishing includes new features as well as old ones. I also ran into some technical blockers, but those were more a function of not understanding Unreal as well as I do now than anything.

We didn't win DARE in the end, but a couple of the DARE teams got to show their games at EGX in London which was really cool, I don't think the university do that anymore which is a real shame. On the personal side of things, I really got a lot out of working in a team like this. Previously I had struggled, feeling like because I couldn't make a game solo then I must be inadequate, but working on Braise Satan shifted my perspective to realise the tremendous value of collaboration.

# The Team

- [Finn Link](https://www.linkedin.com/in/finn-link-8aba582b1/) - Producer & 2D Artist
- [Will McDonald](https://www.linkedin.com/in/will-mcdonald-games/) - Game Designer
- [Phil Smith](https://www.linkedin.com/in/philip-smith-9767872ab/) - 3D Animator
- [Sophie Maxwell](https://www.linkedin.com/in/sophie-maxwell-91a550245/) - 3D Artist
- [Stuart Mackay](https://www.linkedin.com/in/stuart-mackay-4414b8227/) - 3D Character Artist
- Travis Denton - 3D Artist (not in DARE)
- [Jeremy Wong](https://www.linkedin.com/in/jjeremywong/) - 3D Environment Artist (only in DARE)
- [Aaron Walker](https://www.linkedin.com/in/aaron-walker-5270a4335/) - Programmer
- [Finlay Laidlaw](https://youtu.be/dQw4w9WgXcQ) - Programmer
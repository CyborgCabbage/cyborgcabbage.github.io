---
layout: post
title: AURIONNAUT
---

[AURIONNAUT on itch.io](https://cyborgcabbage.itch.io/aurionnaut)

I decided to go with a space combat game for a few reasons:
- I wanted to make a 3D game.
- “Space” is a simple environment that shouldn’t require many models or textures.
- I had just seen some videos about Elite (1984) and it was very inspiring to me.

When picking a physics engine I saw that PhysX was a popular choice and it had a lot of documentation.

Things I would do differently:
- I think it would have been better to work out a simple UI framework, rather than doing everything manually.
- I used a state machine for switching between the main menu, in flight, the space station menu, etc; this lead to issues down the line and I needed to create some singletons with awkward lifetimes because of it. A state-stack - which I have used in previous projects - would have been better. 
- The challenge required me to stick with the style of the base project that was provided, I took this to mean that I should create abstract interfaces for audio and physics, as the base project had done for its systems. If I were making this project of my own volition I probably wouldn’t have done this, and used the time I saved to add more features.

Perhaps if I had done the above things, I would have been able to add features more efficiently and had a little extra time to add gameplay features like upgrades and enemies with different stats.
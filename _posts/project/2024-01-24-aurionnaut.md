---
layout: post
title: AURIONNAUT
---

{% youtube "https://www.youtube.com/watch?v=GGP0daoy_pI" %}

[AURIONNAUT on itch.io](https://cyborgcabbage.itch.io/aurionnaut)

This project was created for the [IntoGames Search for a Star](https://gradsingames.com/search-for-a-star/search-for-a-star-programming-finalists-2024/) competition, in the category of programming.

The competition was open ended in terms of what game you could make, I decided to go with a space combat game for a few reasons:
- I wanted to make a 3D game.
- “Space” is a simple environment that shouldn’t require many models or textures.
- I had just seen some videos about Elite (1984) and it was very inspiring to me.

# Gameplay

The gameplay of my game is inspired heavily by elite, it is basically a stripped down version.

The player does two things:
- Trading at space stations
- Travelling between space stations and (possibly) fighting pirates

Unlike Elites huge universe, I wanted to limit the game to only the solar system. 

# Architecture

We were given a basic framework with input and 2D DirectX rendering. The project specification was open ended in terms of what game we could create, but we were supposed to stick to the provided architecture. In the provided architecture, implementation details were abstracted with interfaces (IGraphics, IInput, IShader, etc.), I expanded on this by implementing interfaces for physics, audio, and sprite-based font rendering.

The backends I had made before had not used this level of abstraction, they used graphics APIs and libraries directly or a library like SFML. It was interesting to try and define a minimal API the satisfies my needs, and then iterate on it over the course of the project. I abstraction obviously isn't as useful as it could be because there is only one platform and one implementations, but creating an intermediary does help manage the separation of game and engine code.

## Physics

I chose PhysX as my physics engine, it is popular and has lots of documentation. The API wasn't dissimilar to Box2D which helped (I suppose there are only so many ways to build a physics API). 

First, I tried normal space physics, no air resistance or linear damping. Though realistic, this
isn’t very fun. The player spends a lot of time accelerating and decelerating, it isn’t conducive to
dog fighting/combat. I enabled linear and angular damping, which indeed made the ship feel
more agile and responsive (my narrative excuse is that whatever the pirates are using to stop you from
hyper-driving away also creates a “drag-field” in the area).

But there was still an issue, turning felt a bit “loose”. I guessed that this was because in real
life, a banking turn “pushes” off the air to redirect velocity in the new direction. To emulate this, I
take the ships local forward velocity from the previous frame, subtract it, and add it back in the
new direction. This makes so that the ships forward velocity (forward from the ships
perspective) follows their rotations.

## User Interface

[DirectXTK](https://github.com/microsoft/DirectXTK) which implements batched font and sprite drawing in a way that I was able to integrate into the engine framework relatively easily. Due to the time constraints, I opted not to create a UI framework, instead UI logic was implemented on a case-by-case basis, working out the layout with maths.

Doing things without a framework let me get the UI off the ground faster, but by the end of the two weeks I was regretting the decision. Still though, I was able to implement the main menu and space station menus. Though I wasn't able to to implement some of the extra space station features I had originally wanted, like a space ship repair/upgrade menu and a casino.

A menu feature I like is the loading bar when you are travelling to a station. It is simple, but the fact that a pirate raid can come at any point during the loading brings some tension into it. The loading bar also takes longer for more distant journeys which works well for game feel.

## AI

There is only one kind of enemy in the game: the pirate ship.

The idea of creating a 3D manoeuvring space ship AI is daunting, but there are factors that simplify the problem:
- There are no obstacles in the environment, so no pathfinding is needed
- There is a single predetermined target, the player

With these constraints in mind I implemented a simple state machine:
- IN: The ships orients towards the player. Once it gets within ten metres it switched to OUT.
- OUT: Once the ship is 100 metres away from the player, switch to TURN.
- TURN: Pitch up until the player is in front (past the forward plane), switch to IN

The ship is also always thrusting forward and will shoot the player if they are in line. 

This state machine worked well enough for my small game, though I have no idea how to approach this kind of AI with a more complex environment. I am also relying on the fact that the player will usually be moving, if they sit still the AI will slam straight into them (but who knows, maybe this is how real space pirates do it).

## Trading

The player can buy and sell a number of trade goods. Each good has a series of variables governing its market behaviour. 

{% highlight cpp %}
class TradeGood {
public:
	TradeGood(const std::wstring& Name, bool Sensitive, float HalfLife, float BasePrice, float PriceVariance, float PriceFrequency, const std::wstring& Description);
	std::wstring Name;
	bool Sensitive;
	int HalfLifeDays;// (not implemented)
	float PriceBase;
	float PriceVariance;
	float PriceFrequency;
	std::wstring Description;
};
{% endhighlight %}

## Reflection

I think the game turned out fun! It is obviously not a complete experience, and the pirates can be annoying to fight, but you can make some trades and shoot some pirates and there is a decently rewarding challenge in it.

Things I would do differently:
- I think it would have been better to work out a simple UI framework, rather than doing everything on a case-by-case basis.
- I used a state machine for switching between the main menu, in flight, the space station menu, etc; this lead to issues down the line and I needed to create some singletons with awkward lifetimes because of it. A state-stack - which I have used in previous projects - would have been better. 
- The challenge required me to stick with the style of the base project that was provided, I took this to mean that I should create abstract interfaces for audio and physics, as the base project had done for its systems. If I were making this project of my own volition I probably wouldn’t have done this, and used the time I saved to add more features.

Perhaps if I had done the above things, I would have been able to add features more efficiently and had a little extra time to add gameplay features like upgrades and enemies with different stats.
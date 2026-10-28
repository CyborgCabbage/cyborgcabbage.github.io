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

## Gameplay

The gameplay of my game is inspired heavily by elite, it is basically a stripped down version.

The player does two things:
- Trading at space stations
- Travelling between space stations and (possibly) fighting pirates

Unlike Elites huge universe, I wanted to limit the game to only the solar system. 

## Architecture

We were given a basic framework with input and 2D DirectX rendering. The project specification was open ended in terms of what game we could create, but we were supposed to stick to the provided architecture. In the provided architecture, implementation details were abstracted with interfaces (IGraphics, IInput, IShader, etc.), I expanded on this by implementing interfaces for physics, audio, and sprite-based font rendering.

The backends I had made before had not used this level of abstraction, they used graphics APIs and libraries directly or a library like SFML. It was interesting to try and define a minimal API the satisfies my needs, and then iterate on it over the course of the project. I abstraction obviously isn't as useful as it could be because there is only one platform and one implementations, but creating an intermediary does help manage the separation of game and engine code.

I chose PhysX as my physics engine, it is popular and has lots of documentation. The API wasn't dissimilar to Box2D which helped (I suppose there are only so many ways to build a physics API). 

## User Interface

[DirectXTK](https://github.com/microsoft/DirectXTK) which implements batched font and sprite drawing in a way that I was able to integrate into the engine framework relatively easily. Due to the time constraints, I opted not to create a UI framework, instead UI logic was implemented on a case-by-case basis, working out the layout with maths.

Doing things without a framework let me get the UI off the ground faster, but by the end of the two weeks I was regretting the decision. Still though, I was able to implement the main menu and space station menus. Though I wasn't able to to implement some of the extra space station features I had originally wanted, like a space ship repair/upgrade menu and a casino.

A menu feature I like is the loading bar when you are travelling to a station. It is simple, but the fact that a pirate raid can come at any point during the loading brings some tension into it. The loading bar also takes longer for more distant journeys which works well for game feel.

## Trading

{% include image.html url="/assets/images/aurionnaut_market.png" description="Station trading menu" %}

When a player arrives at a station they have the opportunity to buy and sell a number of trade goods. The players goal is to find something with a low price at this station, buy it, and travel to a station where it can be sold at a higher price, making a profit. To enable this gameplay, each good has a series of variables governing its market behaviour. 

{% highlight cpp %}
class TradeGood {
public:
	TradeGood(const std::wstring& Name, bool Sensitive, float HalfLife, float BasePrice, float PriceVariance, float PriceFrequency, const std::wstring& Description);
	std::wstring Name;
	bool Sensitive;// (not implemented, would have made some goods more attractive to pirates)
	int HalfLifeDays;// (not implemented, would have made goods depreciate in value over time)
	float PriceBase;
	float PriceVariance;
	float PriceFrequency;
	std::wstring Description;
};
{% endhighlight %}

The variables are used to generate the price of a good at a particular station at any given time. Each station has differently seeded price which creates price variation across the solar system.

{% highlight cpp %}
int PlanetarySystem::Station::GetGoodPrice(TradeGood& good, int minutesBack) {
	double totalTime = PlanetarySystem::GetInstance()->TotalTime;
	float noiseX = std::fmod(floor(totalTime / 60.0) - minutesBack, 100'000.0) * good.PriceFrequency;
	float noiseY = std::hash<std::wstring>{}(good.Name) & 0xffff;
	float riskFactor = 1.0 + Pirates * 0.2;//Pirates can cause the cost of everything to be up to 20% higher
	return (good.PriceBase + good.PriceVariance * PriceNoise.GetNoise(noiseX, noiseY)) * riskFactor;
}
{% endhighlight %}

A 2D Perlin noise generator is used, pulled from the [FastNoiseLite](https://github.com/Auburn/FastNoiseLite) library. The X axis represents the passage of time, and a hash is used to get a unique Y value for each trade good. In hindsight, it would make more sense to use a unique 1D noise generator for each trade good, also the "std::fmod(...)" was meant to prevent precision issues (as time increased) but I don't think it is actually necessary.

Each station has a pirate value [0, 1] which defines the extent to which pirates are present in the area. It is more risky to trade in areas with more pirates so the player should be rewarded for doing so. I think 20% is actually too low of an increase, I was concerned about balance but the pirate combat is hard enough that a more significant reward would make sense.

Finally the, "PriceBase" is summed with the random variation from the price noise, which is scaled by "PriceVariance" so some goods stay closer to their base price. Similarly, "noiseX" is scaled by "PriceFrequency" which makes some goods more volatile over time than others.

Overall it is a primitive system, but it has just enough in it to do its job in the game. Of course, a market simulation like this could be expanded in basically any direction. If I were to work on it further I would implement the "Sensitive" and "HalfLifeDays" values I specified, to create more "personality" in how each trade good behaves. I also had wanted to make some stations suppliers or high-demand users of certain goods based on narrative reasons, like the gas giants being a supply of fuel and Earth being a supply of artifacts.

## Travel

{% include image.html url="/assets/images/aurionnaut_travel.png" description="Station travel menu" %}

The travel menu presents a list of possible destinations (stations) and information about travelling to each. The important factors are the fuel cost (1 tonne per astronomical unit) and the percentage chance of encountering pirates (the average of the risks of the origin and destination station).

An idea I had but didn't implement was a 2D map of the solar system, the code actual contains a simulation of planetary positions so it would just be a matter of rendering.

## Combat

{% include image.html url="/assets/images/aurionnaut_combat.png" description="Pirate raid" %}

### Physics

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

### Enemy Behaviour

There is only one kind of enemy in the game: the pirate ship.

The idea of creating a 3D manoeuvring space ship AI is daunting, but there are factors that simplify the problem:
- There are no obstacles in the environment, so no pathfinding is needed
- There is a single predetermined target, the player

With these constraints in mind I implemented a simple state machine:
- IN: The ships orients toward the player. Once it gets within ten metres it switched to OUT.
- OUT: Once the ship is 100 metres away from the player, switch to TURN.
- TURN: Pitch up until the player is in front (past the forward plane), switch to IN

The ship is also always thrusting forward and will shoot the player if they are in line. 

This state machine worked well enough for my small game, though I have no idea how to approach this kind of AI with a more complex environment. I am also relying on the fact that the player will usually be moving, if they sit still the AI will slam straight into them (but who knows, maybe this is how real space pirates do it).

## Reflection

I think the game turned out fun! It is obviously not a complete experience, and the pirates can be annoying to fight, but you can make some trades and shoot some pirates and there is a decently rewarding challenge in it.

Things I would do differently:
- I think it would have been better to work out a simple UI framework, rather than doing everything on a case-by-case basis.
- I used a state machine for switching between the main menu, in flight, the space station menu, etc; this lead to issues down the line and I needed to create some singletons with awkward lifetimes because of it. A state-stack - which I have used in previous projects - would have been better (in this model, menus/game-states are pushed and popped). 

Perhaps if I had done the above things, I would have been able to add features more efficiently and had a little extra time to add gameplay features like upgrades and enemies with different stats.
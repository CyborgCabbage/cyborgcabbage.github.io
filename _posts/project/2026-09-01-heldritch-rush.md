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

{% include image.html url="/assets/images/heldritch_spline.png" description="Section of rail with 3 Rail Point Actors, the spline can be easily edited" %}

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
- the designer doesn't have to be overly precise with how they place spline points, because with pathfinding the player is guaranteed to walk along the ground naturally

Fortunately, Unreal provides a PathFollowingComponent which can be added to the player controller, making it quite easy to implement pathfinding based movement.

The biggest issue is that the pathfinding may fail, perhaps a physics object or enemy blocks the path when they shouldn't and the player can't progress. In these situations, the player is automatically teleported to the next point. It is a matter of level design and play testing to make sure these teleports aren't needed.

### Rail Point Actor

The spline movement prototype had all the events in one array on the rail actor, but I thought this would not be ergonomic to use for levels of the size we wanted to make. So, I added rail point actors to divide events based on which spline point they occur at.

Rail points are attached to the rail actor, each spline point can have one rail point. The rail actor automatically moves rail points to their spline point, where-ever it is in the level. Rail points have a billboard sprite to make them easy to select in the editor and also use a text component to list all their events so a designer can understand the event flow of the level at a glance.

(the rail point sprite and text components are only included editor builds 😉)

{% include image.html url="/assets/images/heldritch_outline_splines.png" description="Rail Actors and Rail Points in the outliner" %}

Another important feature for usablity is that rail points don't need to be adjusted when spline points are added or removed. This was possible by using spline metadata to adjust the indices of the rail points automatically. I used [this guide](https://mintbanjo.com/articles/unreal-engine-5-spline-point-metadata/) to help with the spline metadata setup.

{% include video.html url="/assets/videos/heldritch_rail_update.mp4" description="Showing adding, repositioning, and removing spline points" %}

### Events

So, each rail point has a list of events but how do these work? I remembered the triggers and modifiers in the Input Mapping Context, they allow for the user to select from drop down of classes and then configure each one they instanced. This seemed like the perfect design for the event list.

{% include image.html url="/assets/images/heldritch_input_mapping_context.png" description="Input Mapping Context, see the trigger array?" %}

Looking at the engine code, this is accomplished by created a {% ihighlight cpp %}TArray{% endihighlight %} of a base class. The array also needs the {% ihighlight cpp %}UPROPERTY(Instanced){% endihighlight %}. This specifier causes the property editor to create new instances of a class instead of selecting from existing instances.

{% highlight cpp %}
UCLASS(NotPlaceable, PrioritizeCategories=("Rail"))
class HELDRITCHRUSH_API AHrRailPoint : public AActor
{
	GENERATED_BODY()

public:
	// [...]
	
	UPROPERTY(EditInstanceOnly, Instanced, BlueprintReadOnly, Category = "Rail")
	TArray<TObjectPtr<UHrRailEvent>> Events;

	// [...]
};
{% endhighlight %}

The base class of the rail event needs to have {% ihighlight cpp %}UCLASS(EditInlineNew){% endihighlight %} to be used with {% ihighlight cpp %}UPROPERTY(Instanced){% endihighlight %}. 

{% highlight cpp %}
UCLASS(Blueprintable, Abstract, EditInlineNew, CollapseCategories)
class HELDRITCHRUSH_API UHrRailEvent : public UObject
{
	GENERATED_BODY()
public:
	virtual UWorld* GetWorld() const override final;
	void OnRailEventBegin(AHrGameplayPlayerController* Controller);
	void OnRailEventTick(AHrGameplayPlayerController* Controller, float DeltaTime);
	void OnRailEventEnd(AHrGameplayPlayerController* Controller);
	UFUNCTION(BlueprintPure)
	bool IsEndingEventInstantly() const;

	UPROPERTY(EditDefaultsOnly, Category = "Rail Event")
	bool bStartsInstantlyEnded = false;
protected:
	// [Blueprint and native versions of event functions]
	UFUNCTION(BlueprintCallable)
	void EndRailEvent();
};

{% endhighlight %}

When the player gets to a rail point, the player stops and processes the events in order.
The begin, tick, and end functions contain a rail events logic. These can be easily overridden in Blueprints to script new types of rail event. Events can either be instant or have a duration
Events with a duration call the protected function "EndRailEvent()" to end. Events that have are instant enabled the flag {% ihighlight cpp %}bStartsInstantlyEnded{% endihighlight %}. 

When processing instant events, the Player Controller will process multiple in the same tick. This is necessary to prevent the player from stopping for a tick, and it guarantees multiple instant event effects will all be applied in the same tick.

For example, a series of events may go something like this:
- Begin camera movement
- Start voice clip
- Spawn enemy wave
- Wait for 1 second

In one tick, the first two events will executed and the enemies will be spawned. The wave event ends when all enemies have been killed, there would then be 1 second delay before the player moves on.

{% include image.html url="/assets/images/heldritch_rail_events.png" description="Series of rail events in inspector" %}

### Branching

A key feature we wanted compared with the original DARE game was the ability for the level designer to introduce a branch in player's path. When the player comes to one of these branches, they stop and press a key to select a direction. This is a system that took a bit of iteration to get right, but I believe I was able to come to a good design.

Firstly, the Unreal spline component doesn't support branching, you get one contiguous spline in each component. That being the case, I opted to have multiple Rail Actors to represent the different paths. This does introduce a minor problem: where the player should start in the level is now ambiguous. To solve this, each rail has a Boolean for specifying whether it is the spawn rail or not. In our game mode class, {% ihighlight cpp %} void RestartPlayer(AController* NewPlayer) {% endihighlight %} is overridden and the player is spawned at the start of the rail.

{% highlight cpp %}
void AHrGameplayGameMode::RestartPlayer(AController* NewPlayer)
{
    AHrRail* SpawnRail = nullptr;
    for (TActorIterator<AHrRail> It(GetWorld()); It; ++It)
    {
        AHrRail* Rail = *It;
        if (Rail && Rail->SplineComponent && Rail->SplineComponent->GetNumberOfSplinePoints() > 0)
        {
            if (Rail->bIsSpawnRail)
            {
                SpawnRail = Rail;
                break;
            }
            if (SpawnRail == nullptr)
            {
                SpawnRail = Rail;
            }
        }
    }
    // [Then spawn player at SpawnRail if it is not nullptr]
}
{% endhighlight %}

Using the event system I set up before, I can add a branch event. It has a map of directions to destinations; when the event is triggered a UI widget is created to show the options and the player can select their target direction.

There are three different types of destination: 
- {% ihighlight cpp %}Continue{% endihighlight %} which simply lets the player continue on their current rail, past the branch event
- {% ihighlight cpp %}RailAndIndex{% endihighlight %} where the designer specifies a target rail actor and a target spline index on that rail
- {% ihighlight cpp %}RailPoint{% endihighlight %} where the designer specifies a target rail point

{% ihighlight cpp %}EditConditionHides{% endihighlight %} is used so that only the relevant configuration variables are shown for each destination type.

{% highlight cpp %}
UENUM(BlueprintType)
enum class EHrBranchDirection : uint8
{
	Up,
	Down,
	Left,
	Right
};

UENUM(BlueprintType)
enum class EHrBranchMode : uint8
{
	Continue,
	RailAndIndex,
	RailPoint
};

USTRUCT(BlueprintType)
struct FHrBranchParameters
{
	GENERATED_BODY()

	UPROPERTY(BlueprintReadWrite, EditAnywhere)
	EHrBranchMode Mode = EHrBranchMode::Continue;
	
	UPROPERTY(BlueprintReadWrite, EditAnywhere, meta=(EditCondition="Mode==EHrBranchMode::RailAndIndex", EditConditionHides))
	AHrRail* RailActor = nullptr;

	UPROPERTY(BlueprintReadWrite, EditAnywhere, meta=(EditCondition="Mode==EHrBranchMode::RailAndIndex", EditConditionHides))
	int32 RailIndex = 0;
	
	UPROPERTY(BlueprintReadWrite, EditAnywhere, meta=(EditCondition="Mode==EHrBranchMode::RailPoint", EditConditionHides))
	AHrRailPoint* RailPoint = nullptr;
};

UCLASS(Abstract, Blueprintable)
class HELDRITCHRUSH_API UHrBranchRailEvent : public UHrRailEvent
{
	GENERATED_BODY()

public:
	// [...]
    
	UPROPERTY(BlueprintReadOnly, EditAnywhere, meta=(ForceInlineRow))
	TMap<EHrBranchDirection, FHrBranchParameters> Branches;
};
{% endhighlight %}

The branch event logic is mostly implemented in Blueprints, but a few functions are done in C++. This is probably not the great technical design, doing the class in Blueprints entirely makes for sense because rail events are more on the scripting side (simpler logic that is iterated often). I think I just like looking at written code more, to me it is more digestible, personal flaw of mine honestly.

## The End

Okay I'll leave it there for now; there are other systems I made or contributed to: the enemy spawning system, the weapons system, level visibility system, our input and user interface setup using Common UI, and more. 

This post focuses on something I was the primary contributor for, but I'll also highlight my compatriots: 
- [Cameron Baird](https://www.linkedin.com/in/cameron-baird02/) who worked on the project before me and has now out lasted me; he has done lots of stuff but in relation to this post he made the camera system which integrates with the rail system using a camera rail event.
- [Kieran McDermott](https://www.linkedin.com/in/kieran-mcdermott-120924159/) who worked on weapon logic and made a rail event for the tutorial, along with a bunch of other bits and pieces across the project.
- [Tanapat Somrid](https://www.linkedin.com/in/tanapat-somrid-265098204/) who started working on the project just after I left.

And that's just the programmers! A more exhaustive list of everyone who works on Heldritch Rush can be found [here](https://slothdoctorgames.wixsite.com/game-developer-websi).
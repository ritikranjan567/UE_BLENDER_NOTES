# Unreal Engine 5.6 C++ Notes

> A compact reference for Unreal Engine 5.6 C++ development.

---

# Table of Contents

- Geometry Brushes
- Creating C++ Classes
- AActor Basics
- Logging
- Live Coding
- Member Variables (UPROPERTY)
- FVector
- Moving Actors
- Downloading Assets
- Basic Level Setup
- Components
- Box Components
- UCLASS Metadata
- Referencing Other Actors
- Delegates & Events
- UFUNCTION
- Useful Shortcuts

---

# Geometry Brushes (BSP)

Two brush types are available:

- **Additive** → Adds solid geometry.
- **Subtractive** → Removes geometry from additive brushes.

Combining both allows quick level prototyping.

Example:

```
Additive Cube
      |
      +-- Subtractive Cylinder

Result:
A room with a circular hole.
```

Useful for:

- Rooms
- Hallways
- Windows
- Doors
- Quick prototypes

---

# Creating a New C++ Class

From the editor:

```
Tools
    → New C++ Class
        → Choose Parent Class
```

Example inheritance:

```
AActor
    ↑
AMyActor

UActorComponent
    ↑
UMoverComponent
```

---

# AActor Basics

The base gameplay class.

Common functions:

```cpp
virtual void BeginPlay() override;
virtual void Tick(float DeltaTime) override;
```

## BeginPlay

Runs once when gameplay starts.

```cpp
void AMyActor::BeginPlay()
{
    Super::BeginPlay();

    UE_LOG(LogTemp, Display, TEXT("Game Started"));
}
```

---

## Tick

Runs every frame.

```cpp
void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}
```

Always multiply movement by **DeltaTime**.

Bad:

```cpp
Location.X += 100;
```

Good:

```cpp
Location.X += 100 * DeltaTime;
```

This makes movement FPS independent.

---

# Actor Location

Get current position:

```cpp
FVector Location = GetActorLocation();
```

Set position:

```cpp
SetActorLocation(NewLocation);
```

Example:

```cpp
FVector NewLocation = GetActorLocation();
NewLocation.Z += 100;

SetActorLocation(NewLocation);
```

---

# Rotation

Rotate every frame.

```cpp
FRotator RotationToAdd = RotationVelocity * DeltaTime;

AddActorLocalRotation(RotationToAdd);
```

Example:

```cpp
RotationVelocity = FRotator(0, 45, 0);
```

Rotates 45 degrees per second around Yaw.

---

# Logging

Use `UE_LOG`.

Basic:

```cpp
UE_LOG(LogTemp, Display, TEXT("Hello World"));
```

Print float:

```cpp
float Value = 12.0f / 5.0f;

UE_LOG(LogTemp, Display,
       TEXT("Value = %f"), Value);
```

Print integer:

```cpp
int32 Score = 100;

UE_LOG(LogTemp, Display,
       TEXT("Score = %d"), Score);
```

Print FString:

```cpp
FString Name = "Kaan";

UE_LOG(LogTemp, Display,
       TEXT("Name = %s"), *Name);
```

Print bool:

```cpp
bool bDoorOpen = true;

UE_LOG(LogTemp, Display,
       TEXT("Door Open = %s"),
       bDoorOpen ? TEXT("True") : TEXT("False"));
```

Print pointer:

```cpp
UE_LOG(LogTemp, Display,
       TEXT("Pointer = %p"), Pointer);
```

---

# Live Coding

Enable **Live Coding** (bottom-right of the editor).

Allows compiling `.cpp` changes without restarting.

Recommended workflow:

✅ Live Coding

- CPP implementation changes

Rebuild from Visual Studio when:

- `.h`
- `.hpp`
- UPROPERTY
- UFUNCTION
- UCLASS

change.

Also perform a full rebuild occasionally for stability.

---

# FString

UE5 uses **FString** instead of `std::string`.

```cpp
FString Name = TEXT("Player");
```

Appending:

```cpp
Name += TEXT(" One");
```

Length:

```cpp
Name.Len();
```

---

# Member Variables (UPROPERTY)

Declare in the header.

Editable in editor:

```cpp
UPROPERTY(EditAnywhere)
float Speed = 200.0f;
```

Read only:

```cpp
UPROPERTY(VisibleAnywhere)
int32 Coins = 10;
```

Common specifiers:

| Specifier | Meaning |
|------------|----------|
| EditAnywhere | Editable in editor |
| VisibleAnywhere | Visible only |
| BlueprintReadWrite | Accessible from Blueprint |
| BlueprintReadOnly | Read only in Blueprint |

Example:

```cpp
UPROPERTY(EditAnywhere, BlueprintReadWrite)
float Health = 100;
```

---

# FVector

Represents a 3D vector.

```cpp
FVector Position;
```

Example:

```cpp
FVector A(100, 0, 0);
FVector B(0, 100, 0);

FVector C = A + B;
```

Useful functions:

Distance:

```cpp
float Distance =
    FVector::Dist(Start, End);
```

Normalize:

```cpp
Direction = Velocity.GetSafeNormal();
```

Length:

```cpp
Velocity.Length();
```

Equals:

```cpp
A.Equals(B);
```

Dot product:

```cpp
float Dot = FVector::DotProduct(A, B);
```

Cross product:

```cpp
FVector Cross = FVector::CrossProduct(A, B);
```

---

# Moving an Actor

Simple movement:

```cpp
FVector NewLocation =
    GetActorLocation() +
    Velocity * DeltaTime;

SetActorLocation(NewLocation);
```

---

# Prevent Overshoot

Useful for moving platforms.

```cpp
FVector Direction =
    PlatformVelocity.GetSafeNormal();

FVector NewStart =
    StartLocation +
    Direction * MoveDistance;

SetActorLocation(NewStart);

StartLocation = NewStart;

PlatformVelocity = -PlatformVelocity;
```

---

# Interpolation

Smooth movement.

```cpp
CurrentLocation =
    FMath::VInterpConstantTo(
        CurrentLocation,
        TargetLocation,
        DeltaTime,
        Speed);
```

Useful for:

- Doors
- Platforms
- Cameras

---

# Downloading Assets

```
Epic Games Launcher

↓

Fab Library

↓

Claim Assets

↓

Add to Project
```

---

# Basic Level Setup

An empty level is completely dark.

Add:

- Directional Light
- Sky Light
- Sky Atmosphere
- Volumetric Clouds
- Exponential Height Fog

---

## Directional Light

Acts as the Sun.

Rotate:

- Pitch → Sunrise/Sunset
- Yaw → Sun direction

---

## Sky Light

Provides ambient lighting.

After changing the environment:

```
Click

Recapture
```

to update lighting.

---

# Useful Shortcut

Press:

```
F8
```

Detach camera during Play mode.

Very useful for debugging.

---

# Components

Composition is preferred over deep inheritance.

Main component types:

## ActorComponent

No transform.

Used for:

- Health
- Inventory
- AI
- Damage

---

## SceneComponent

Has transform.

Can be attached.

```
Root

├── Camera

├── Mesh

└── Spring Arm
```

Get owning actor:

```cpp
AActor* Owner = GetOwner();
```

---

# Box Component

`UBoxComponent`

Derived from `USceneComponent`.

Provides:

- Collision
- Trigger volumes
- Transform

Invisible during gameplay.

Example:

```cpp
UPROPERTY(EditAnywhere)
UBoxComponent* Trigger;
```

---

# UCLASS Metadata

Example:

```cpp
UCLASS(
    ClassGroup=(Custom),
    meta=(BlueprintSpawnableComponent)
)
class MYGAME_API UMover : public UActorComponent
{
    GENERATED_BODY()
};
```

`BlueprintSpawnableComponent`

means the component can be added inside the editor.

Without it, the component may not appear in the **Add Component** list.

---

# Referencing Another Actor

Expose an actor reference:

```cpp
UPROPERTY(EditAnywhere)
AActor* TargetActor;
```

Access one of its components:

```cpp
UMover* Mover =
    TargetActor->FindComponentByClass<UMover>();
```

Always check for `nullptr`.

```cpp
if (Mover)
{
    Mover->Move();
}
```

---

# Include Order

Always include your own headers before the generated header.

Correct:

```cpp
#include "Mover.h"

#include "MyComponent.generated.h"
```

Wrong:

```cpp
#include "MyComponent.generated.h"

#include "Mover.h"
```

`generated.h` should **always be the last include**.

---

# Delegates & Events

Delegates are Unreal's event system.

Example:

```
Player enters trigger

↓

Overlap Event Fires

↓

Door Opens
```

---

## Binding Events

Usually inside constructor or `BeginPlay`.

```cpp
OnComponentBeginOverlap.AddDynamic(
    this,
    &UTriggerComponent::OnOverlapBegin);

OnComponentEndOverlap.AddDynamic(
    this,
    &UTriggerComponent::OnOverlapEnd);
```

---

## Overlap Begin

```cpp
UFUNCTION()
void OnOverlapBegin(
    UPrimitiveComponent* OverlappedComp,
    AActor* OtherActor,
    UPrimitiveComponent* OtherComp,
    int32 OtherBodyIndex,
    bool bFromSweep,
    const FHitResult& SweepResult);
```

Example:

```cpp
void UTriggerComponent::OnOverlapBegin(...)
{
    UE_LOG(LogTemp,
           Display,
           TEXT("Player Entered Trigger"));
}
```

---

## Overlap End

```cpp
UFUNCTION()
void OnOverlapEnd(
    UPrimitiveComponent* OverlappedComp,
    AActor* OtherActor,
    UPrimitiveComponent* OtherComp,
    int32 OtherBodyIndex);
```

---

# UFUNCTION

Registers a function with Unreal Engine's reflection system.

Example:

```cpp
UFUNCTION()
void OpenDoor();
```

Blueprint callable:

```cpp
UFUNCTION(BlueprintCallable)
void OpenDoor();
```

Blueprint event:

```cpp
UFUNCTION(BlueprintImplementableEvent)
void DoorOpened();
```

---

# Common Unreal Macros

```cpp
UCLASS()
```

Marks a class for reflection.

```cpp
UPROPERTY()
```

Exposes variables to Unreal.

```cpp
UFUNCTION()
```

Exposes functions to Unreal.

```cpp
GENERATED_BODY()
```

Generates reflection boilerplate.

---

# Common Types

| Type | Description |
|-------|-------------|
| FVector | 3D Vector |
| FRotator | Rotation |
| FTransform | Position + Rotation + Scale |
| FString | String |
| FName | Optimized immutable name |
| FText | Localized UI text |

---

# Example: Simple Moving Actor

```cpp
void AMovingPlatform::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    FVector Location = GetActorLocation();

    Location += Velocity * DeltaTime;

    SetActorLocation(Location);
}
```

---

# Example: Rotating Actor

```cpp
void ARotatingActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    FRotator Rotation(0, 45 * DeltaTime, 0);

    AddActorLocalRotation(Rotation);
}
```

---

# Example: Opening Door on Overlap

```cpp
void UTriggerComponent::OnOverlapBegin(...)
{
    if (DoorActor)
    {
        UMover* Mover =
            DoorActor->FindComponentByClass<UMover>();

        if (Mover)
        {
            Mover->Move();
        }
    }
}
```

## NOTE   
**Difference between constructor and begin play:**  
Constructor is called when the object is created therefore it is called before the game starts. The values set by it
can be seen in the editor. BeginPlay is called when the game starts and is used to initialize values that are not set in the editor.

---

# Deletegate/Events
- An event that is fired when something happens, like a player entering a trigger box component.
- can connect functions to these delegates and these functions will be called when the event is fired.

*BoxComponent* has delegate called `OnComponentBeginOverlap` that is fired when another actor enters the box component.
Also has similar delegate called `OnComponentEndOverlap` that is fired when another actor leaves the box component.

Below is an example of how to bind functions to these delegates.
```cpp
OnComponentBeginOverlap.AddDynamic(this, &UTriggerComponent::OnOverlapBegin);
OnComponentEndOverlap.AddDynamic(this, &UTriggerComponent::OnOverlapEnd);
```
Usually binded in the constructor or in the `BeginPlay` function.
Here is example of both function signatures for the overlap events.

```cpp
void UTriggerComponent::OnOverlapBegin(UPrimitiveComponent* OverlappedComp, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	ToggleMoverShouldMove(true);
}

void UTriggerComponent::OnOverlapEnd(UPrimitiveComponent* OverlappedComp, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)
{
	ToggleMoverShouldMove(false);
}
```

For any object to be detected by the overlap events, it must have a collision component and in properties
Turn on "Generate Overlap Events" in the collision component.

# Actor Tags
Used to add tags to actors in the level. Can be used to identify actors in the level. For example, if you want to find all actors with a specific tag.
Tags can be added to object in the editor or in code. In editor properties, action section, there is a "Tags" property where you can add tags. In code, you can use the `Tags` property of the actor.

`ActorHasTag(FName Tag)` Checks if the actor has a specific tag.

Below is an exmaple of adding tags to actor via code:
```cpp
Tags.Add(FName("MyTag"));
```

# Creating components in code

- First include the header file for the component you want to create. For example, if you want to create a box component, include "Components/BoxComponent.h".
```cpp
USceneComponent* RootComp;
RootComp = CreateDefaultSubobject<USceneComponent>(TEXT("RootComp"));
```
Now the root component is create, then need to set the root component
```cpp
SetRootComponent(RootComp);
```
Similarly trigger component can be created like this:
```cpp
TriggerComp = CreateDefaultSubobject<UTriggerComponent>(TEXT("TriggerComp"));
// now attach it to the root component
TriggerComp->SetupAttachment(RootComp);
```
Attaching basically dragging droping in editor
Now if you create a blueprint of this actor, you will see the root component and the trigger component attached to it.  
*By doing so these components can be easily accessed in the cpp code, whereelse these components will not be readily available if
attached/created using the blueprint editor. You will have to use `FindComponentByClass` to get the component.*

# Collision System
For an actor/mesh in collision settings props, there is `Collision Presets` property. It has differenet settings for collision. For example, "BlockAll" will block all other actors, "OverlapAll" will overlap all other actors, "NoCollision" will not collide with anything. There is also a custom option where you can set the collision settings for each object type. For example, you can set the collision settings for "Pawn" to block, and for "WorldStatic" to overlap.   

`Collision Enabled` property can be set to "NoCollision", "QueryOnly" or "PhysicsOnly". "NoCollision" will not collide with anything, "QueryOnly" will only generate overlap events, and "PhysicsOnly" will only generate physics collisions.
It also has 'Collision Enabled' by default with both Query and Physics enabled.

`Object Type` property can be set to "WorldStatic", "WorldDynamic", "Pawn", "PhysicsBody", "Vehicle", "Destructible", "Water" or "UI". 
This is used to determine what type of object this actor is. For example, if you set it to "Pawn", it will be treated as a pawn by the engine.

`Collision Responses` property can be set to "Ignore", "Overlap" or "Block". 
This is used to determine how this actor will respond to other actors. 
For example, if you set it to "Ignore" for "Pawn", it will ignore all pawns.
Overlap with "Pawn" will generate overlap events with pawns, and block with "Pawn" will block all pawns.

# Input System

Inside `Input` folder there is Actions folder that has list of actions for the input and in the root
there is `InputMappingContext` IMC which is a collection of input action and their input bindings.  
For example, there is an action called "Jump" and it is bound to the space bar. When the space bar is pressed, the "Jump" action will be triggered.

In Actions folder you should create new actions like 'IA_Interact'. And on default input mapping context, this
new action can be mapped to required key.

## Connecting New Action with C++ function
Two step process
- Register the mapping context
- connect cpp function with the action, as input action by themsalves do not do anything, they need to be
connected to a function that will be called when the action is triggered.

In player controller class below code can be found to link Input context
```cpp
/** Input Mapping Contexts */
UPROPERTY(EditAnywhere, Category="Input|Input Mappings")
TArray<UInputMappingContext*> DefaultMappingContexts;

/** Input Mapping Contexts */
UPROPERTY(EditAnywhere, Category="Input|Input Mappings")
TArray<UInputMappingContext*> MobileExcludedMappingContexts;
```
And in `BP_PlayerController` blueprint, in the properties, you can see the 
`DefaultMappingContexts` and `MobileExcludedMappingContexts` properties. You can add the input mapping context to these properties.   
`SetupInputComponent()` method is implemented in player controller class, 
and in this method, the input mapping context is registered with the input system.  

Actions can be found in the Character class as UPROPERTY like below
```cpp
/** Jump Input Action */
UPROPERTY(EditAnywhere, Category ="Input")
UInputAction* JumpAction;

/** Move Input Action */
UPROPERTY(EditAnywhere, Category ="Input")
UInputAction* MoveAction;

/** Look Input Action */
UPROPERTY(EditAnywhere, Category ="Input")
class UInputAction* LookAction;

/** Mouse Look Input Action */
UPROPERTY(EditAnywhere, Category ="Input")
class UInputAction* MouseLookAction;
```

`UPROPERTY(EditAnywhere, Category ="Input")` means that the property can be edited in the editor and
it will be shown in the "Input" category in the details panel.

Now create/define the fuction be executed when the action is triggered.
And in `SetupInputComponent()` of the Character class method, bind the action to the function like below
```cpp
// Jumping
EnhancedInputComponent->BindAction(JumpAction, ETriggerEvent::Started, this, &ADungeonEscapeCharacter::DoJumpStart);
EnhancedInputComponent->BindAction(JumpAction, ETriggerEvent::Completed, this, &ADungeonEscapeCharacter::DoJumpEnd);

// Moving
EnhancedInputComponent->BindAction(MoveAction, ETriggerEvent::Triggered, this, &ADungeonEscapeCharacter::MoveInput);

// Looking/Aiming
EnhancedInputComponent->BindAction(LookAction, ETriggerEvent::Triggered, this, &ADungeonEscapeCharacter::LookInput);
EnhancedInputComponent->BindAction(MouseLookAction, ETriggerEvent::Triggered, this, &ADungeonEscapeCharacter::LookInput);

// Interact
EnhancedInputComponent->BindAction(InteractAction, ETriggerEvent::Started, this, &ADungeonEscapeCharacter::Interact);
```
There are three types of trigger events, `Started`, `Triggered` and `Completed`.

# Line Traces/Raycasts
Used to detect objects in the world by casting a ray from a start point to an end point.
If the ray hits an object, it will return information about the hit, such as the location, normal, and the actor that was hit.

Easy to miss as it is just a line.
A `Shape Trace` is a better option as it is a shape with thickness.

Should all use specified trace channel. Then the trance will only hit objects with that trace channel when 'Block'.

It is very similar to overlap events.

# Creating custom trace channel
Project Settings -> Engine Section -> Collision -> Trace Channels -> New Trace Channel

Now in editor for Actors this new channel is visible and configurable in the collision settings.

# UWorlds

It is the top level object that gets loaded and it contents all the objects in the level. 
It is the game world itself. Line tracing and shape tracing is done in the world.
It is also used to spawn actors and get all actors of a specific class.
Line tracing method: `LineTraceSingleByChannel` and `LineTraceMultiByChannel` for single and multi hit respectively.
Shape tracing method: `SweepSingleByChannel` and `SweepMultiByChannel` for single and multi hit respectively.

To get the world object, use `GetWorld()` method. It is available in all actors and components and anywhere in the
unreal engine code.
```cpp
UWorld* World = GetWorld();
World->SweepSingleByChannel();
```
As `SweepSingleByChannel` needs a collision shape in args need to create a new collision shape for it as well.
Below is code to create a sphere collision and draw a debug sphere:
```cpp
FCollisionShape InteractionSphere = FCollisionShape::MakeSphere(InteractionRadius);

// Draw debug sphere
DrawDebugSphere(GetWorld(), End, InteractionRadius, 20, FColor::Blue, false, 5.0f);
DrawDebugSphere(GetWorld(), Start, InteractionRadius, 20, FColor::Green, false, 5.0f);
```


To draw a debug line using cpp code:
```cpp
DrawDebugLine(GetWorld(), Start, End, FColor::Red, false, 5.0f);
```

Trace Code example:
```cpp
FCollisionShape InteractionSphere = FCollisionShape::MakeSphere(InteractionRadius);
FHitResult HitResult;
UWorld* World = GetWorld();
bool HasHit = World->SweepSingleByChannel(HitResult, Start, End, FQuat::Identity, ECC_GameTraceChannel2, InteractionSphere);

if (HasHit) {
	AActor* HitAcctor = HitResult.GetActor();
	if (HitAcctor->ActorHasTag(FName("CollectableItem"))) {
		// TODO: Hit actor is a collectable item, so we can pick it up
	}
	else if (HitAcctor->ActorHasTag(FName("Lock"))) {
		// TODO: Hit actor is a lock, so we can interact with it
	}
}
```

# Unreal Engine Casting

Above code HitActor is of type AActor, therefore we won't be able to access the methods of
ACollectableItem or ALock. Therefore we need to cast the HitActor to the specific type of actor we want to interact with.
```cpp
Cast<ACollectableItem>(HitAcctor) // returns a pointer to ACollectableItem if HitActor is of type ACollectableItem,
otherwise returns nullptr.
```


# Best Practices

- ✅ Use `DeltaTime` for frame-independent movement.
- ✅ Prefer composition (`UActorComponent`) over deep inheritance.
- ✅ Always check pointers for `nullptr`.
- ✅ Keep `generated.h` as the last include.
- ✅ Use `UPROPERTY()` for editor-visible variables.
- ✅ Use `UFUNCTION()` for delegates and Blueprint integration.
- ✅ Rebuild from Visual Studio after changing header files.
- ✅ Use Live Coding for `.cpp` changes only.
- ✅ Prefer `FString`, `FName`, and `FText` over STL types when interacting with Unreal systems.

---

Happy Unreal Engine development! 🚀
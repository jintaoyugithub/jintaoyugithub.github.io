---
title: Programming and Scripting in UE
katex: true
mathjax: false
date: 2024-05-04 23:04:30
tags:
cover_image: /imgs/coverImgs/ue.png
---

>:warning: Note :warning:：UE的体量真的是非常大，之前断断续续看过一些教程视频，但是很多地方都不太了解，所以借着这次机会通读一下UE的文档，目前我所看的文档是对应**5.2版本**的，如果之后有变动会在后面写明。这一篇主要是关于**UE Programming**的内容，有关**UE渲染系统**的文档阅读笔记应该在之后几篇之中。

## GameObject in UE

### [UObject](https://dev.epicgames.com/documentation/en-us/unreal-engine/objects-in-unreal-engine?application_version=5.2)

The base class for objects in Unreal

- no constructor arguments
- can't compile without the default constructor
- only values and subobjects set up, no functionality in construction
- initialization of **Actor** and **Actor Component** shoule be in **BeginPlay()**
- nerver use **new/delete** to create a **UObject** since they're memory managed by UE's GC system
- **do not** possess any built-in update ability, need to use **FTickableGameObject** class specifier and then implement **Tick()** function to solve this

:exclamation: Since UObject is part of the UE refelction system, it includes some benefits like **Garbage collection, Reflection, Serialization, Network replication and so on**.


**Class Default Object(COD)**: a default template object contained in each **UCLASS**.
- **GetClass()** to get UCLASS from an object instance

### [Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine?application_version=5.2) 

- objects that can be placed into a level
- **do not** have transforms themslves, rely on the transforms of their components
- base ob AActor c++ class
- containers that hold components
- :bookmark:[lifecycle](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-actor-lifecycle?application_version=5.2) 
- be able to tiked(updated) every frame, call **Tick()** function automatically
- are not garbage collected, need to explicitly destroyed by calling **Destroy()** 

### [Components](https://dev.epicgames.com/documentation/en-us/unreal-engine/components-in-unreal-engine?application_version=5.2)

All components are derived from **UActorComponent** and they play a very important role in the game play since everything the player sees or interacts with in the game world is ultimately the work of some type of Components, for example, they're **the only way** to:
- render meshes and images
- implement collision
- play audio
- and so on

#### 3 major components

**Actor Components**
- class UActorComponent
- no transform, no geometry representation
- mainly for abstract/non-physical behaviors like movement, attribute management
- **do not** update by default, try following to turn on update for actor components
```c++
// in constructor
PrimaryComponentTick.bCanEverTick = true;
// in constructor or elsewhere, call
PrimaryComponentTick.SetTickFunctionEnable(true);
```
- :bookmark:need to manually create render and physics state

**Scene Components**
- class USceneComponent, derived from UActorComponent
- have transform but no geometry representation
- be able to update by implementing **TickComponent** function
- need to manually create physics state
- have the ability to attach to one another(same for its children)

**Primitive Components**
- class UPrimitiveComponent, derived from USceneComponent
- have transform and have geometry representation
- mainly for render and perform physics simulation
- be able to update by implementing **TickComponent** function
- automatically create render and physics state

#### Visualization Components

To display components that do not have visual representation, you just need to:
- create any regular component
- call **SetIsVisualizationComponent** function of that component

:warning: Note :warning:: in order to make packaged builds won't be affected by these components, the code should be inside of preprocessor checks again **WITH_EDITORONLY_DATA** or **WITH_EDITOR**, for example:

```c++
void UCameraComponent::OnRegister() {
#if WITH_EDITORONLY_DATA
	if (AActor* MyOwner = GetOwner())
	{
		// ...
		if (DrawFrustum == nullptr)
		{
			DrawFrustum = NewObject<UDrawFrustumComponent>(MyOwner, NAME_None, RF_Transactional | RF_TextExportTransient);
			DrawFrustum->SetupAttachment(this);
			DrawFrustum->SetIsVisualizationComponent(true);
			// ...
		}
	}
	// ...
#endif
}
```

## [Classes in UE](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-classes-in-unreal-engine?application_version=5.2)

**Two major game play classes**:
- prefix is A: spawnable objects
- prefix is U: like objects, which cannot directly instaced into the world
- basic format will be like:
```c++
UCLASS([specifier, ...], [meta(key=value), ...])
class ClassName : public ParentName {
    // must be at very beginning of the class body
    GENERATED_BODY();
}
```
:warning: Note :warning:: More info about **UCLASS** please go to [UCLASS](#UCLASS-macro)

### References in class

#### Asset References 

:warning: Preferred mehtod is to use **Blueprint** to configure asset properties.

Hardcoded references are supported while asset references in class ideally do not exist. You can use **ConstructorHelpers::FObjectFinder** to find a specified objects in content packages.

#### Class Referneces

Most cases, **StaticClass()** will work to get a UClass

#### Component and Sub-Objects references

Generally create component(by using **CreateDefaultSubobject<Component>()**) subobjects and attach them to actor's hierarchy can also be done in **constructor**, in order to ensure the components are always properly create, destroyed and garbage-collected, **we should store the pointer to these components in the class as a UPROPERTY**.

```c++
UCLASS()
class AWindPointSource : public AActor
{
	GENERATED_BODY()
	public:

	UPROPERTY()
	UWindPointSourceComponent* WindPointSource;
};

AWindPointSource::AWindPointSource()
{
	// Create a new component and give it a name.
	WindPointSource = CreateDefaultSubobject<UWindPointSourceComponent>(TEXT("WindPointSourceComponent0"));
}
```

### Unreal Header Tool

a certain header file structure for UHT to perform preprocessing step in order to harness the functionality provided by UObject.

```c++
#pragma once
 
#include 'Object.h'
#include 'MyObject.generated.h'

UCLASS()
class MYPROJECT_API UMyObject : public UObject
{
	GENERATED_BODY()
};
```

- MyObject.generated.h file should be in the last #include
- [UCLASS()](#UCLASS-macro)
- MYPROJECT_API: expose the class to other modules
- GENERATED_BODY(): sets up the class to support the infrastructure required by the engine

### Properties

1. Integer as **Bitmasks**: unreal support us with many features for better enums utilizing, there are many ways you can defines enum as **bitmasks enums**:
- **Meta tag "Bitflags"** in **UENUM** and **Bitmask** in **UPARAM** 
- **ENUM_CLASS_FLAGS()** macro on a defined enum
- **BitmaskEnum = "name of a enum"** meta tag in **UPROPERTY** to let a variable to use enum and exposed in the editor

You can read some **Bitmask enums examples** in the last part.

2. String: 4 main string in unreal
- FString: dynamic array of chars
- FName: an **immutable case-insensitive** string
- FText: localized text, for example, display different languages based on the user setting
- TEXT() macro: TCHAR type for characters, it's the most uses

3. [Properties specifiers and metadata specifiers](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-uproperties?application_version=5.2) 


### Struct

### Functions

## Reflection System

### C++ Refelction System

### [Unreal engine refelction system](https://dev.epicgames.com/documentation/en-us/unreal-engine/reflection-system-in-unreal-engine?application_version=5.2)

**UCLASS** macro:
- makes UObject visible to UE
- support [Class and Metadata Specifiers](https://dev.epicgames.com/documentation/en-us/unreal-engine/class-specifiers?application_version=5.2) 
- tag custom classes derived from UObject
- make custom classes awared by [UObject Handling System](#UObject-Handling-System) 

### [UObject Handling System](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-object-handling-in-unreal-engine?application_version=5.2)

**UCLASS, UPROPERTY, UFUNTION** make classes, properties and functions can be awared by unreal engine and implement a lot of features under-the-hood for them:
- automatic properties initialization
- automatic references updating
- serialization
- garbage collection

### [Garbage Collection System](https://unrealcommunity.wiki/garbage-collection-36d1da)

## Things that might be new to you

1. **Bitmask** in unreal engine

8bit to store maximum 256 enums, data base is better when the enum values are greater than some threshold

bitflag enums, for example, in a tile game you can represent 8 different combination with 8 bitflat enums, in c++ code, you can make 64 differenct combinations when using uint64. And bitflag can be also used as a constrains on different types of tiles, like it doesn't make scene ocean tiles are the neighbour of desert tiles.

use hexadecimal notion to specify the value

example: damage on different human part






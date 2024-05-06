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


## Classes in UE

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

### Struct

### Functions

## Reflection System

### C++ Refelction System

### [unreal engine refelction system](https://dev.epicgames.com/documentation/en-us/unreal-engine/reflection-system-in-unreal-engine?application_version=5.2)

**UCLASS** macro:
- makes UObject visible to UE
- support [Class and Metadata Specifiers](https://dev.epicgames.com/documentation/en-us/unreal-engine/class-specifiers?application_version=5.2) 
- tag custom classes derived from UObject
- make custom classes awared by [UObject Handling System](#UObject-Handling-System) 

### [UObject Handling System](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-object-handling-in-unreal-engine?application_version=5.2)

### [Garbage Collection System](https://unrealcommunity.wiki/garbage-collection-36d1da)

## UE Smart Pointer Library


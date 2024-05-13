---
title: Unreal Engine Rendering and Graphics
katex: false
mathjax: false
date: 2024-05-05 15:01:54
tags:
cover_image: /imgs/coverImgs/ueren.png
---

## notes from lumen inside unreal live stream

Mesh Distance Fields
- sparse representation

Surface cache
- capture mesh at low resolution into **atlas** 
- nanite renders into all the cards in parallel 
- lumen have to work with nanite when using high-poly assets
- the whole scene will be computed direct lighting + low quality gi gather for multi-bounce

optimization
- merge all mdf into global distance field
- merge surface cache into voxel lighting
- detail trace for first 2 meters, and then gdf, but don't work for scene that have too much overlapping

hardware ray tracing
- nanite provide a proxy geometry version that hardware ray tracing can work on that
- cost a lot when do dynamic mesh, like skin, since they deformed. we have to calculate the ray traicng structure every frame(BVH)
- better to have less overlap mesh

half a ray per pixel??? but indoors nedd 200+ for quality

2 solution:
- irradiance fields
- screen space denoiser

lumen use screen space radiance caching: select some amount of pixels, shoot a bunch of ray from them to get a stable light, and interpolate the result with nanite mesh.

common screen space denoiser won't work in ue5 since they want high detailed mesh, spd will reuse the hit point with same normal, but ue5 will say no since each pixel have different normal as they're quite detailed.


Emissive
- can't replace light with emissive mesh
- large dim emissive and light source + small dim emissive will work

Base color:
- base color has a huge impace on GI, the brighter the better
- indirected lit surface, as the lighting will bounce from all direction.....

in reflection, hardware ray tracing really shines, but soft ray traicng in lumen has the pretty similar effect compare with hardware ray traicng



what's world space radiance caching?

what's atlas?

what's dfao?

what's instance static mesh?

what's final gather?

process of propagating the light from the lumenScene to your final pixel on the screen

what's Material Parameter Collections?


## notes from nanite inside unreal live stream

nanite is completely gpu driven pipleline
- scene is fullly in gpu memory
- sparsely updated where things changed, which mean only the changes will uploaded to gpu, not every frame
- all vertex/index data store in a large resource
- call drawcall function for the whole scene not individual object, since we don't want to call drawcall function on that large amount of triangles, we can use this method to call drawcall only once.
- triangle cluster culling, group triangles into clusters, and do culling for each clusters

Decouple visibility from material

why? watch nanite inside unreal video for more(locate to 1:01:00)

solution
- using deferred materials
- 1 drawcall each material


got a problem with sub-linera scaling with triangles

cluster hierarchy on a cluster basis to decide the lod a each cluster

nanite try to rendering pixle size triangel, but tiny triangles are terrible for typical rasterizer, solution to this is software rasterizer

hardware rasterizer for big triangel, software rasterizer for tiny triangels

16k x 16k shadow maps for every light



### What's
- megascans asset
- RVT
- per view on gpu























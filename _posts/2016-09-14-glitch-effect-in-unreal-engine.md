---
layout: post
title:  "UI distortion in Unreal Engine"
tags: [ opensource,glitch,shader,ue4 ]
---

In [Breached](http://breached-game.com/) we have a lot of digital noise/glitch effects. While we mostly used the wonderful ["Sci-Fi and Glitch Post-Process"](https://www.unrealengine.com/marketplace/sci-fi-and-glitch-post-process) package, I’ve wanted to add a bit of uniqueness to UI noise effect and assembled a material function for distorting UI texture UVs.

![](/assets/images/posts/ue-glitch-1.png)

It uses a bunch of procedural noise generators, panners and a texture mask to apply specific distortion pattern. 

![](/assets/images/posts/ue-glitch-2.png)

The function turned out to be quite flexible, so I’ve thought it could be useful to share it. You can grab archive with the function and texture mask here: [DistortionEffect.zip](https://drive.google.com/open?id=11wTOVe6jKvPIKUR_51Md6lHByAz5gtsc)

Just drop the unzipped folder to UE project and the function should become available as "DistortUV" node.

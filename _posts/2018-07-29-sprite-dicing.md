---
layout: post
title:  "Sprite Dicing"
tags: [ opensource,2D,atlasing,optimization,unity ]
featured_image_thumbnail: 
featured_image: /assets/images/posts/dicing-1.webp
featured: true
---

Just finished working on an editor extension for Unity which allows to split up a set of large sprite textures into small chunks, discard identical ones, bake them into atlas textures and then seamlessly reconstruct the original sprites at runtime for render.

This technique allows to significantly reduce build size, in cases when multiple textures with identical areas are used.

Consider a visual novel type of game, where you have multiple textures per character, each portraying a different emotion; most of the textures space will be occupied with the identical data, and only a small area will vary:

![](/assets/images/posts/dicing-2.png)

These original five textures have total size of 17.5MB. After dicing, the resulting atlas texture will contain only the unique chunks, having the size of just 2.4MB. We can now discard the original five textures and use the atlas to render the original sprites, effectively compressing source textures data by 86.3%.

![](https://i.gyazo.com/7f79936fc714abcc342ae348478b9c8e.gif)

The project is available on GitHub under MIT license: [github.com/Elringus/SpriteDicing](https://github.com/Elringus/SpriteDicing).

**By the way, in case you’re developing a visual novel, [take a look at my visual novel engine](https://u3d.as/1pg9), which uses this extension.**

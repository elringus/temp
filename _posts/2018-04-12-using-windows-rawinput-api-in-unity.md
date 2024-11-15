---
layout: post
title:  "Using Raw Input API in Unity"
tags: [ opensource,control,event,native,keyboard ]
---

While working on a broadcast solution that used Unity to render and control a virtual character, I’ve faced a problem of capturing keyboard input events while the application is not in focus. It turned out that it’s not something doable using Unity’s input system, so I’ve assembled a C# wrapper over the Windows RawInput API to hook directly to the native input events.

![](/assets/images/posts/raw-input.png)

Project on GitHub (MIT license): [github.com/Elringus/UnityRawInput](https://github.com/Elringus/UnityRawInput).


---
author: aborres
comments: true
date: 2017-02-15 23:47:07+00:00
layout: post
link: http://aborres.com/post/pbr-material-editor-update-2/
slug: pbr-material-editor-update-2
permalink: /post/pbr-material-editor-update-2/
feature-img: "assets/images/header_mi.png"
title: 'PBR: Material Editor – Update 2'
thumbnail: "assets/images/opengl_logo.png"
wordpress_id: 333
categories:
- Games
- Graphics
- OpenGL
- Post
- Project
- PBR-Material-Editor
tags: [ games, Graphics, OpenGL, post, projects, Rendering, teesside, uk, university ]
---

![OpenGL]({{ "/assets/images/opengl_logo.png" | absolute_url }})

[In my first post](/post/pbr-material-editor-update-introduction/), I introduced a schedule where I planned all the big tasks in my time from now to May. For this second week, I planned to be working on the Render Engine. I am producing an API independent library where the rendering API can be decided at the start of the application and can be even switched at runtime. I am using some concepts that [BGFX](https://github.com/bkaradzic/bgfx) introduces in its code; however, I am adapting its design to my necessities and modifying concepts where I think this library is wrong, at least in their usability. At this phase, I have decided to implement the OpenGL code inside the engine. The API works with a Renderer (abstract class) where all the graphics code is implemented and every inherited Renderer (depending on the active graphics library) has to implement those methods to work with the engine. The design of the Renderer is API independent, this means, the Renderer abstracts concepts such as binding (for the OpenGL machine state, or object oriented from DirectX). Each Renderer has to keep all its resources referenced and has to define all its constants. The communication of this API with the external user is made by Handlers. I wanted to keep these handlers to the minimum to enforce the design to be no API dependent so these handlers only declare an integer identifier which allows the graphical system to identify them into their state. From the engine perspective, the engine works with the classical rendering display list concept. I have called a class GPU and that class contains all the available rendering commands. All these commands are queued into a rendering buffer and the rendering thread will consume them as soon as it can. Once one frame is generated the engine starts to generate the next one. When the rendering thread has finished drawing the current frame it will check if it should start rendering the next one or if it should wait until the frame is processed. The generation of all these tasks is made from the engine thread and from all the threads that the engine uses in his processes. I am testing the engine with 4 offset frames between both systems and it is producing good results; however, I am planning to test different delays and to introduce even more frames. In this video, I am testing some features of the scene graph and I have added textures to the default material.

<iframe width="560" height="315" src="https://www.youtube.com/embed/uldAOysWHAc?rel=0&amp;showinfo=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Currently, I have implemented and tested all these elements on the engine:
  * Mesh
  * Shader
  * Material
  * Uniform
  * Texture
  * Camera

Most of these concepts will probably change with the development of the engine but right now I have enough functionality to continue working, developing and testing. For example, I am thinking of introducing instancing into the mesh part or uniform buffers for the material/uniform part and the camera is still in changes because of the composer integration. The first version of uniforms in the engine was to declare uniforms as global entities by name and set their values by material; however, I am still considering to change this feature into different uniforms by material, allowing them to replicate the same name in different shaders. Also, I have been working on optimizations in the rendering part to increase and clean the interaction between the internal API and the external engine.

I am also working on framebuffers but I have not finished this feature yet because I am still deciding the correct way to integrate their functionality into the rendering pipeline. My idea at this point is to give the cameras the ability to render the scene into a given frame buffer and give the framebuffers functionality to work with their materials and generated textures. One important feature that I have introduced this week into the engine is the composer. A composer is an organization manager to short and generate the rendering pipeline. The composer allows the user to define how the scene should be rendered, in which order and allows to set up framebuffers to generate effects and post-processes. I have decided to integrate the composer concept in two different ways:

**Scripting**: By default the engine loads and executes a [LUA Script](https://www.lua.org/) where all the objects of the scene, resources, cameras, framebuffers, etc. Are available to produce the rendering pipeline. This script can be modified by the user of replaced by another different script. This is an example of a possible and simple composer:

![Composer LUA]({{ "/assets/images/2018-04-07-lua_composer.png" | absolute_url }})

**Callback**: The second option is to implement one callback where the engine provides all the resources to work the same way the scripting system does. The callback is implemented in C++ and replaced in the engine main class.

Although my main task was to work on the rendering engine I have produced more functionality, in most cases, because I needed that functionality to test the rendering features. This week I have integrated LUA as scripting language and I have wrapped most of the actual useful functionality, this includes: GameObject, Material, Mesh, Transform, Camera, Render, Engine and some global functionality. I have finished most of the actual component functionality and I have integrated into the engine [IMGUI](https://github.com/ocornut/imgui) as graphical interface library. I will start developing the editor in the following weeks.

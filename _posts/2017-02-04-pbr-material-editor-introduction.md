---
author: aborres
comments: true
date: 2017-02-04 20:20:55+00:00
layout: post
link: http://aborres.com/post/pbr-material-editor-introduction/
slug: pbr-material-editor-introduction
permalink: /post/pbr-material-editor-update-introduction/
thumbnail: "assets/images/2018-04-07-engine_schedule.png"
title: 'PBR: Material Editor - Introduction'
feature-img: "assets/images/header_mi.png"
wordpress_id: 294
categories:
- Graphics
- OpenGL
- Post
- Project
- PBR-Material-Editor
tags: [ Graphics, OpenGL, post, projects, Rendering, teesside, uk, university ]
---

One of the most important modules in the Games Development Career in Teesside University (UK) is the Computing Project. Every year, students from the final year are asked to develop their own programming project alone. This is a good opportunity for us to investigate in those topics we care about the most. Some of us are working on artificial intelligence projects, others are working on procedural generation (for example: generating planets with different biomes) and some of us, like me, are working on computer graphics. I have decided to develop a new 3D engine from scratch and my idea is to develop a PBR material editor over this engine.

## Engine

The first thing I have done is to design how I want the engine to be. As my real project is a material editor I could have selected a commercial engine (Unity Engine, Unreal Engine 4, etc.); however, I have decided to develop my own engine. I want to work in a context where I know how everything works, where I can introduce performance measures in all the hot spots and where I can work comfortably in my tool. Developing a 3D engine is also a complex task and I have decided to do things right. Here is one list of features that I have developed or I am planning to introduce into the engine:

  * Multi-threading
  * Multiplatform Engine
  * Multiple Rendering APIs
  * Component System
  * Scripting
  * Model Loading
  * Scriptable Rendering Pipeline
  * Texture Loading/Generation
  * And much more.

## Physically Based Rendering (PBR)

![PBR Comparison](http://www.meta3dstudios.com/wp-content/uploads/2015/08/PBR-Example-e1440106098549.png) 

Physically based rendering is a collection of techniques, applied to computer graphics, with the finality of producing materials with real physical properties. The real aim of PBR is to mimic the real light interaction over the objects in the real world. PBR is being introduced in most graphical engines because of its excellent results (with high-performance cost) and it is more plausible appearance. Metallicity or roughness are common parameters used to define the visual appearance of materials; however, each company has its own PBR model with different parameters, textures and configurations. I thought this could be a good opportunity to investigate and learn about this actual topic and I thought the best way to learn could be to develop multiple PBR models and compare their characteristics, in terms of visual appearance and in performance. Therefore, as soon as I have ended developing my rendering engine I will start working on a tool to load models, to write/load materials and run them in real time with modern light techniques (IBL/PBIBL/SH) and compare their visual results. Also, I want to introduce a metrics system into the engine to compare rendering timings between these models.

My plan is to share here weekly updates about the state of the project. I don't really have much time to work relaxed on the project so I am planning to crunch from now to the presentation day. I am not really overwhelmed with other modules; however, I work every day so I have to spend half of my days with my company work. Even so, I have planned all my tasks in time and at this moment I am really advanced with the project.

![Schedule]({{ "/assets/images/2018-04-07-engine_schedule.png" | absolute_url }})

---
author: aborres
comments: true
date: 2017-02-19 23:49:21+00:00
layout: post
link: aborres.com/post/pbr-material-editor-update-3/
slug: pbr-material-editor-update-3
permalink: /post/pbr-material-editor-update-3/
feature-img: "assets/images/header_mi.png"
thumbnail: "assets/images/FBX_Review_Splash.png"
title: 'PBR: Material Editor – Update 3'
wordpress_id: 350
categories:
- Games
- Graphics
- OpenGL
- Post
- Project
- PBR-Material-Editor
tags: [ Deferred, games, Graphics, OpenGL, post, projects, Render, Rendering,
        teesside, uk, university ]
---

![FBX]({{ "/assets/images/FBX_Review_Splash.png" | absolute_url }})

## Rendering

This week I have been working in multiple areas I thought should be done before continuing developing the main aspects of the application. For this third week I was supposed to be working on the component system of the engine; however, I finished most of the components two weeks ago. I designed the system on the combination of a common abstract class for all components and templates for their integration and process. All the components have different phases of update and these phases are processed by the engine (this allows me to encapsulate the component information and to dispatch threads with the components update task. From the perspective of the GameObjects, they are allowed to contain as many components as necessary but their interaction is contained inside the engine. This allows the user to quickly and easily work with the components he needs in every object. However, I have been expanding the functionality of these components. Now the rendering process is processed by the cameras (it seemed a logic interaction between cameras and rendercomponents). As I was expanding and working with the render I decided to test my graphics API and integrate deferred rendering inside the engine and the process was surprisingly good. The connection and interaction of all the different needed parts were clear and easy to code and at the same time allowed me to test my API design.

![Deferred Rendering 4 textures]({{ "/assets/images/2018-04-07-Deferred.png" | absolute_url }})

However, I took advantage of this situation and I adapted some parts in the process to make them work more accordingly
with the engine design. For example, one of the areas I worked on more was the framebuffer integration. Framebuffers are
designed to easily work with textures, materials and other buffers but with the new adaptation of the rendering (with
the cameras performing the actual draw) I needed to adapt them to be drawn by a camera or to allow cameras to draw their
info inside them. Also, I tested the past week new integration (the composer). The composer allowed me to quickly
implement and perform tests over my rendering pipeline. The actual composer has three phases and each phase is performed
differently for each type rendering object. In this simulation, I tested the C++ composer version because at this point
I do not need the advantages of the scripting until I have some kind of interface working on the engine which allows me
to reload scripts in real time. After all this work I started working on advanced rendering techniques such as PBR or
IBL; however, most of those techniques require of 3D texture or cubemaps; therefore, my next step was to support into
the engine this kind of textures. I made this tweaking my rendering API, now cubemaps are supported inside the texture
functions and the way the API expects those textures is a block of aligned memory where all the 6 textures are sort by
the OpenGL convention. To test them, I implemented an example of skybox into the [engine](https://www.youtube.com/embed/fjxkg-Gaufg).

##  IO

Sooner or later, I knew that I was to need some kind of Input/Output management and this week was the time to integrate it. Now all the input and output tasks into the engine are controlled by a manager. This manager has different functionality to load different files (models, textures, texts, etc.), release them or even reload them. The system reserves memory blocks for each resource and keeps them until somebody releases them from memory. This allows the engine to not perform unnecessary loads of the same resource and avoids losing time and memory computing the same resource. In a close future, I am planning to introduce into the engine reference counting for the resources. The way I am planning to integrate this system is creating my own ["shared_ptr"](http://www.cplusplus.com/reference/memory/shared_ptr/) which will count the number of references to the same resource and once one resource lose all its references will notify the engine to "garbage collect" that resource and erase it from memory. I am already working with my own memory system and the plan is to replace the actual system with the combination of these pointers and a memory manager, one I have worked with before is [TLSF](http://www.gii.upv.es/tlsf/) with really good results. I will probably integrate this manager into my engine at some point, even in the LUA state, and all the memory will be managed through this util.


## Model loading


One of the most common model formats is [OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file). OBJ stores all the
information of the model inside a text file. All the vertex info is stored in plain text, dividing the information in
vertex, normals, uvs, and index. This makes OBJ a really simple format to parse and integrate into the engine. One
simple parser would read each line, it would check and separate the line depending on the line info and store the
information into memory. This format even supports material loading; however, most of the material definitions are using
old shading parameters and they could not be really useful in some engines. OBJ is widely used between artists; however,
the actual requirements are leaving aside this format. Currently, [FBX](https://en.wikipedia.org/wiki/FBX) is becoming
the standard. FBX supports multiple information inside itself. FBX is more than a model format, these files include
inside them full information to import and export full scenes (models, animations, lights, cameras, textures, and much
more). FBX is being developed by Autodesk, the company producing [3ds
Max](http://www.autodesk.com/products/3ds-max/overview) and [Maya](http://www.autodesk.com/products/maya/overview), the
two most used modeling programs. Nevertheless, the documentation of this SDK is really outdated, there are no real
examples of use and the wide support of features of this library ends into a relatively complex task to integrate this
format. This lack of information with the huge size of the kit (+1GB) can scare some developers and probably the best
option is to develop an external tool to parse and export models into a custom format and make the engine work with that
custom extension. This also improves the performance in loading models and optimizes the size of these models into the
project. I ended integrating the full SDK into the engine; however, I will probably remove it and I will develop an
external tool for this. Here is an example of a model being loaded in my engine with drag & drop system: [link](https://www.youtube.com/watch?v=fjxkg-Gaufg)

## Editor

Finally, one of the things I have done this week was to separate the actual editor from my test area. I have created a new project and I have moved all my test code into that project. This way I am leaving the editor project free of engine tests. Also, I have been working on some editor features. I have already created the main class for the future editor and I have integrated the GUI draw into the engine main draw. At first, I am going to use IMGUI for the GUI design but maybe I will change this library for others with more/different features. Next week, I will be working on PBR and IBL.

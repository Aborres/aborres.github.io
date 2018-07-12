---
author: aborres
comments: true
date: 2017-02-27 22:41:44+00:00
layout: post
link: aborres.com/post/pbr-material-editor-update-4/
slug: pbr-material-editor-update-4
permalink: /post/pbr-material-editor-update-4/
title: 'PBR: Material Editor – Update 4'
wordpress_id: 364
categories:
- Post
- PBR-Material-Editor
tags:
- games
- Graphics
- OpenGL
- post
- projects
- Rendering
- teesside
- uk
- university
---

![Model Loading]({{ "/assets/images/2018-04-07-modelloading4.jpg"}})

At this point of the development, the need of a test area to work with models and textures and play with some of their aspects has been growing. Furthermore, according to my schedule, I was supposed to be working on editor features; therefore, this week I have been designing the editor and its features. The first thing I did was to integrate into the editor the drag and drop functionality. At this point, all files dropped into the screen are collected into the editor.


### The Catalog

![Catalog Window]({{ "/assets/images/2018-04-07-catalog.png"}}){: .align_image_left }The catalog collects all the items dropped into the editor. The items are sorted and show different options depending on their type, this means, fbx, dae, obj or any kind of model will have an add button to add that model into the current scene. The objects will hang directly on the root node and would be modified from other windows of the editor. Also, I am working on mouse movement and interaction on the screen and right now I have keyboard movement using the classical WASD keys with the second button of the mouse pressed and I am working on a camera arc rotation system using the wheel mouse button. I would also like to introduce into the editor mouse picking to be able to select different models and edit their properties without having to search for them in the scene window. Other kinds of items such as textures, scripts, fonts, etc. Will show their information and properties on the screen with a little preview image on the catalog window and the user will be allowed to modify some parameters (depending on the item) from more advanced window options. For example, at some point, I am planning to introduce texture channel modifying or even to be able to modify, write and save scripts and shaders to test them directly from the editor. Also, I have to introduce some functionality to allow the user to reload shaders, scripts and composer (if the user is using the scriptable composer version) to re-import all those items into the editor and, this way, avoid unnecessary reloads of the entire program.


### Scene!

![Scene]({{ "/assets/images/2018-04-07-graph.png"}}){: .align_image_right }
The scene window shows all the current objects in the scene. By default, the scene has a root node and a camera node. The camera node is the actual camera which is rendering the scene into the screen. The root node is the main node of the scene and all objects added to the scene will be "children" of this node. The name of the nodes comes from the model file, if the artist named the objects of his file all of them will appear in this window. The skeletal hierarchy appears on this screen as well; however, I am not planning to introduce animations or the vertexes weight unless I have enough time at the end. One thing I would like to introduce into this window is the ability to modify the hierarchy and to allow the user to reorder nodes, to change the relationship of the nodes or even to delete specific nodes or hierarchies.


### Inspector!

<p class="align_center" markdown="1">
![Inspector Window]({{ "/assets/images/2018-04-07-inspector.png"}})
</p>
The inspector window is probably the most important window on the editor. The inspector shows all the information of the currently selected object. If the current object is a node this window will show all the information relative to this node:
 
  * Transform: The transform section shows all the information of the node relative to its position in the world. I am basing my user API in the idea of Unity's style; therefore, this window will allow the user to modify the position, rotation and scale of the object.
 	
  * Render: The Render section shows the information about how the actual node is being rendered. If one node does not show a render section that is because that node is only part of the scene graph.

    * Material: This section shows the actual shaders that are being used to render the model. In a near future, the material will allow the user to select different materials, load custom shaders and work with uniforms.

    * Mesh: This section shows information about the geometry of the mesh.
 	
  * Camera: The camera section shows information about the configuration of the camera. From this section, the user will be allowed to modify parameters such as FOV, near plane, far plane, aspect ratio and type of projection (perspective / orthogonal).

### Extra Features

On top of the screen, I have placed the main bar with some useful menus and information to the user. From that bar the user will be allowed to load/save gameobjects, work with the screen windows and modify some configuration tools and visual aspects. Finally, I am currently working on the implementation of uniforms and textures into the engine. The idea is to be able to create uniforms in the materials and set their values manually from the editor to change the visual aspect of the models, also, I am planning to introduce a texture editor to modify some aspects of the textures.


### Flat Buffers

[![FlatBuffer Example]({{ "/assets/images/2018-04-07-Flatbuffers.png" | absolute_url
}})](https://google.github.io/flatbuffers/){: .align_image_left }

Once I started working with the editor I noticed that I was going to need some way of Saving/Loading information of the engine. At first I decided to implement my own file management system, probably saving raw data binaries into memory; however, I decided that something more flexible and extensible would be necessary for the engine. [Flat Buffers](https://google.github.io/flatbuffers/) is a powerful API developed by google to serialize information with a really high performance. Flat Buffers is oriented to be used in video games, allowing developers to quickly integrate and develop save/load systems into their games. This API works using a system of schemes. A scheme represents the structure and information that a specific buffer must keep. In this example, the scheme is declaring a Material Buffer which will contain the information in form of strings for the shaders of the material and an array of uniforms. At the same time, "uniforms" is an array of tables which defines the name of the uniform and its value. It is necessary to define one scheme for each different buffer we want to serialize. Once we have defined our schemes, Flat Buffers provides of a compiler which transforms our schemes into C++ headers. These headers work as a specialization of templates to our specific types (schemes). I am using this API in its C++ version; however, FlatBuffers works with other languages such as Java, C#, GO, Python, JavasScript, PHP or C.

The way I have integrated this functionality into the engine is by using a templatized serializer which receives an instance of an object and a path and exports a buffer with all the serialized information. One useful feature of this API is that it allows us to extend the schemes to contain more information and old generated buffers will still work with the same serialization code.

---
author: aborres
comments: true
date: 2017-05-11 20:21:23+00:00
layout: post
link: aborres.com/post/pbr-material-editor-update-10/
slug: pbr-material-editor-update-10
permalink: /post/pbr-material-editor-update-10/
title: 'PBR: Material Editor – Update 10'
wordpress_id: 547
categories:
- Post
- PBR-Material-Editor
tags:
- Orange Engine
---

![PBR Gun]({{ "/assets/images/2018-04-07-rotating_gun.gif"}})

I have been away from the blog for some weeks; however, in these three weeks, I have been finishing what I had left to do for my final project. [In the last post](http://aborres.com/post/pbr-material-editor-update-9/), I talked about the final IBL implementation I was producing. At that time, I had finished the radiance and irradiance sphere convolution in terms of shading; however, now I have integrated that convolution inside of the editor. I have introduced many different tools inside of the editor this week, figure 1 shows most of the debugging or lighting tools.


![Editor Tools]({{ "/assets/images/2018-04-07-engine_tools.jpg" | absolute_url }})
*Figure 1: Editor Tools*


The window in the middle shows the Environment Editor. This tool allows to load hdri images or crop skybox textures and convolute them to generate IBL image information (as explained in the previous post). The irradiance and radiance maps (IBL textures) represent a really important part of the static lighting inside of PBR models. Thanks to this tool it is possible to quickly generate and convolute different environments and even to save their textures to disk for future usage. The Render Debug screen and the statistics also provide useful information about models and scene characteristics (gBuffer) and the actual engine performance, for further performance statistics it is possible to access the engine statistics tool.

I had some other tasks in my TODO list and all of them are done. Now it is possible to change the material of the models from the default list or even to load custom shading and I have also integrated the possibility to save and load entire scenes directly from the editor. The custom format I am using stores all the information every model needs to be rendered (Shaders, Uniforms, Textures and Geometry info). Depending on the scene, the size of the textures and the amount of geometry info it could take relatively long loading times; therefore, I am planning to split the loading system to be able to split individual exported scenes and load each object parallel using the task manager system, Figure 2 shows a full scene loaded completely from disk at once with my custom format.

![Vangogh Room]({{ "/assets/images/2018-04-07-vangogh.gif" | absolute_url }})
*Figure 2, Van Gogh room*

I have also introduced some other extra features that I thought could be interesting to implement inside of the engine. As Figure 2 shows, I have implemented the typical FPS camera movement inside of the editor. Figure 3, shows the implementation of an arc ball rotation, something quite useful when comparing and reviewing visual aspects. These two movements are not complex by their selves; however, the actual implementation was trickier than I expected. In [Post 5](http://aborres.com/post/pbr-material-editor-update-5/), I introduced a small commentary about the troubleshooting in the camera lookAt implementation inside of the engine. At that time I worked with these two movements; however, they produced some unexpected troubles. The scenegraph core of the engine at that point was based on Euler angles to compute the rotations inside of the pipeline. Euler angles provide a human interface that can be easily used; however, this system can produce gimbal lock problems. In a quick explanation, Euler angles work defining three different amounts of rotation for each 3D axis (XYZ). The problem comes when you can represent the same rotation as rotating one axis or rotating the other two. This creates a consistency problem when defining rotations than can end with the alignment of two different axis. [This video](https://www.youtube.com/watch?v=zc8b2Jo7mno) illustrates deeply the gimbal lock problem.

![Camera Orbit]({{ "/assets/images/2018-04-07-camera_orbit.gif" | absolute_url }})
*Figure 3, Arc ball camera movement*

The solution to this problem was the introduction of Quaternions inside of the engine. Quaternions are a 4D
representation of rotations inside of the space. Quaternions are defined as ```xi + yj + zk + s = q``` where x, y and z represent complex numbers. Quaternions are not as understandable as Euler angles are; however, they are away from inconsistency problems. At this point of the development, to change the data pipeline of one of the most important complex components can be considered as an error; however, the error was to not introduce them in the beginning. At the end, I have changed the rotations pipeline and now all the rotations in the engine are processed as quaternions.

Finally, I have also finished some tasks from my "I would like TODO" list. I have ended producing a deep comparison between the most used PBR functions (BRDFs). I based my research on the normalization functions from [Graphic Rants](http://graphicrants.blogspot.co.uk/2013/08/specular-brdf-reference.html) for the UE4 PBR modelization. Soon, I will publish the report I have produced as memory of the whole project; however, I will continue working on the engine, the next step will be the introduction of DirectX12 and a custom shaders compiler to introduce the use of a specific syntax that can compile to every graphical API introduced inside of the engine.

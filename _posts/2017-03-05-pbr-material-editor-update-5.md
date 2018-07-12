---
author: aborres
comments: true
date: 2017-03-05 16:31:26+00:00
layout: post
link: http://aborres.com/post/pbr-material-editor-update-5/
slug: pbr-material-editor-update-5
permalink: /post/pbr-material-editor-update-5/
title: 'PBR: Material Editor – Update 5'
wordpress_id: 407
categories:
- Graphics
- OpenGL
- Post
- Project
- PBR-Material-Editor
tags:
- games
- Graphics
- OpenGL
- post
- projects
- Render
- Rendering
- teesside
- uk
- university
---

![BugVsFeature]({{ "/assets/images/bugvsfeature.jpg"}})

This fifth week I was supposed to be working on the initial implementation of PBR and IBL; however, this week I have
been pretty busy with some work/university stuff, even so, I have been working on some other tasks I was needing to
complete before the realistic material implementation. One of the principal problems I have been messing with is a bug
in the FBX importer. As I said in the [Update 3]({{ site.baseurl }}{% link _posts/2017-02-19-pbr-material-editor-update-3.md %}) FBX is a scene format that supports multiple formats and orientation and translations, this creates a difficult task even only to import a model. During this week I noticed that it was not loading properly the texture coordinates of some models and after lots of searches and tests and I have found and fixed the problem.  In some models the uvs were not being loaded correctly because of the export format, some modeling programs allow artists to flip the v coordinate and others allow them to negate the value. As I only want to support one texture coordinates format I ended normalizing all the texture formats into one generalized system and everything is working again.


### Design Talk

Since I finished the main engine structure I wanted to ask one of my lecturers for his opinion and advice about the ideas and features I am developing and to ask him about some changes and features that I could include inside of the engine. We talked for about one hour and a half about multi-threading, the graphical API design, and the component system. I was asking for feedback and he looked pleased with most of the features I am developing and the design. He liked the task manager for the multi-threading part and the component oriented design. He also liked the global uniform system because (paraphrasing): "it is a good abstraction of a rendering concept that makes the user experience better". However, he did not like other aspects, such as the limitation in the engine of the type of uniforms to upload. I only allow uploading integers, vec4, matrix3, matrix4 and textures because if you think about it there is not need for anything else. He said that my thoughts on this are part of a low-level programming orientation more than in a friendly user experience, If I see that It doesn't work I would perform some changes on that part. Finally, I took advantage of this opportunity to discuss the implementation of a lookAt method inside the transform component.


### LookAt

One of the principal topics we discussed in the talk was the implementation of a lookAt method for all the transforms components. I have the system working right now; however, I would like to clarify some aspects where I got stuck. As the engine is based on a multi-threaded system I can not directly modify some aspects inside of the components because "maybe at some point, a thread will come and modify what I did." Let me explain this with an example, working with the transform component I am not allowed to modify the model matrix itself because when the thread in charge of updating the scene graph comes it will override that matrix and I will lose all the changes I had performed. However, that thread only modifies the model matrix; therefore, I am allowed to modify the rest of elements of the component (position, rotation and scale) and taking advantage of this I started to implement the lookat feature. The way I planned was to compute the global transformation to orientate this node to the desired point of view using a generated view matrix. I am using [GLM](http://glm.g-truc.net/0.9.8/index.html) inside the engine for all the mathematics transformations. Although, the answer was simple but not that simple enough because of the design of the engine and some forgotten mathematical properties that I would not forget again. The task is to generate a matrix that orientates the local object to a specific point. The glm::lookAt requires the position of the node, the desired target point of view and a reference vertical axis to calculate a view matrix.

![transpose and inverse demo]({{ "/assets/images/2018-04-07-view_matrix.jpg"}})

The view matrix is a 4x4 matrix which contains a new coordinates system to transform points from a system of coordinates (in most cases world space) to the "view" space of coordinates. The idea was to compute a lookAt matrix glm::decompose that matrix into the TRS components (transform, rotation and scale) and use those generated components into the component. However, there are a few things to take into account. The view matrix is generated to rotate the scene in the opposite direction it should, this can be easily explained if you think of relative moment and cameras. Let's think about a scene with a camera at (0,0,5) and a cube at (0,0,0) where if you move the object in the positive X axis (using OpenGL coordinates system) the cube will disappear through the right edge of the screen; however, if you apply that movement to the camera, the camera would disappear through the right edge but the cube will be moving to the left (relative moment). But I do not want such movements in my lookAt method because in this case all the objects are located in world space. However, there is another problem with this movement. The lookAt transforms the objects to the view space but what I want is to transform the actual object to be pointing to a specific point; therefore, the actual object can not represent the origin in the coordinate system as he is supposed to be rotated in world to a specific point. The solution to the actual problem is resolved performing the transpose and the inverse to erase rotations and to switch the coordinate system. If you have some knowledge about mathematics and matrix operations you could be thinking: "But the transpose and the inverse are exactly the same for orthonormal matrix's so inverting a matrix twice will end in the original matrix". And that is true the inverse and the transpose operations are equitable in the **orthonormal matrix**. The view matrix contains the edge directions for the new coordinates system and also contains the position of the origin of the new system, this means, that view matrix can be or not an orthonormal matrix.

![matrices conversion]({{ "/assets/images/2018-04-07-math_matrixes.jpg"}})


In the cases where the view matrix is not orthonormal we can not assume that the inverse and the transpose are equitable, in these cases the inverse of the transpose its equal to the transpose of the inverse, therefore, these transformations will always end in the view matrix I wanted to calculate. The next step is to multiply the matrix by the inverse of the parent's matrix (to perform the transformation in local space) and decompose the matrix into its components to apply the desired rotation to the actual object.


### Graphics API

I have been thinking about some requirements that the PBR and IBL need and I do not have yet on the engine and there are some features that I have not planned but I am integrating now. The first one is the support to change the rendering state. I knew that I was going to have the need of this change at some point and I was waiting for the moment to include it. Modify the rendering state means to be able to enable or disable some test (Depth, Stencil, Blend, RGB write, etc.) and to be able to set values and options for those tests (Depth: Less, Equal, Never, etc., Blend: ONE, SRC_Alpha, etc.). This is really important because some aspects and implementations require the ability to set up the actual state, for example, the lightning phase of the deferred rendering technique requires a different state than the gbuffer phase. And the state for opaque objects, sprites and transparent objects is different as well. The way I have introduced this concept into the engine is with a list of defines with different binary values. This way the user is allowed to mask different values into the same flag to set in one step the state of the engine, for example:

```c
uint32 flags = OE_STATE_ENABLE_DEPTH | OE_STATE_DEPTH_LESS | OE_STATE_DISABLE_BLEND;
```

### Editor & Uniforms

Finally, I have extended the functionality of the editor. At this point the editor allows the user to load models, load textures, create materials, create uniforms, set uniform values and change the value of the parameters of a specific material. I have also implemented other features such as the possibility of debugging the rendering pipeline showing the gbuffer on screen and some configuration parameters. In this demo, I have a model already loaded into the editor with the default texture of the engine and I am changing its value for the correct albedo texture of the model.

<iframe width="560" height="315" src="https://www.youtube.com/embed/B93xJFnbRrA?rel=0&amp;showinfo=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


### Next Step

My idea for this week is to finish some of the aspects I have not ended yet (editor, rendering features) and this time, start the implementation of the PBR and IBL inside the engine. One rendering feature I want to introduce first is the support of rendering layers inside the engine to sort the objects and to be allowed to only render objects if they are in a specific layer. For example, if I want to deferred render the scene I could use the 4 first layers for this phase and the rest for other forward objects such as Skybox, transparent objects, etc. I have not had much time yet about how is the good way to include this into the engine but I will do it soon. Finally, I would like to create inside the engine the concept of dynamic mesh to improve the performance of the editor.

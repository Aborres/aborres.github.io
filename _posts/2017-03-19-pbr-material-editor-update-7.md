---
author: aborres
comments: true
date: 2017-03-19 21:03:54+00:00
layout: post
link: aborres.com/post/pbr-material-editor-update-7/
slug: pbr-material-editor-update-7
permalink: /post/pbr-material-editor-update-7/
title: 'PBR: Material Editor – Update 7'
thumbnail: "assets/images/2018-04-07-pbr_demo7.gif"
feature-img: "assets/images/header_mi.png"
wordpress_id: 452
categories:
- Post
- PBR-Material-Editor
tags:
- Graphics
- OpenGL
- post
- projects
- Rendering
- teesside
- uk
- university
---

![Object_Picking]({{ "/assets/images/2018-04-07-pbr_demo7.gif" | absolute_url }})

### Structure Modifications & New Features

The development of a game engine can be complex and enormous. New features require deep changes that can complicate the initial structure or even to change it for a new one (something that can be a really painful experience). Some things do not work as initially planned and some of them have to be extended to allow new features or processes. For example, this week I have been working on the cubemap implementation and the memory management. The initial cubemap approximation did not allow to change or upload different cubemaps at run time. It did not even allow to have mipmapping. The new one has these features but now the process to configure a cubemap is bigger than it used to be and the number of necessary parameters has increased deeply. In terms of memory, I was not really working with how to manage memory liberation or how to control when a GPU resource has to be freed. The memory structure until this point was based on a SOA workflow (Structures of arrays). SOA can improve the performance of the engine because of the memory alignment and I have found that SOA also works nicely with multi-threading; however, SOA can complex the structure and the design of the engine. One thing I did not take into account was how to integrate the memory liberation. Now, SOA allows to count the number of references for a specific resource to free the resource in the right moment (when no one is referencing it) and now also controls and sorts the object positions inside of the arrays. Objects used to be registered in a specific place and they were going to stay there for the duration of the execution; however, that is not compatible when you need to free objects. Now the arrays are shorter depending on different parameters and the liberation of these objects depends on multiple purposes.


### UE4 PBR


PBR models are complex to implement and to integrate and usually, the mathematics are complex and the developers integrate tricks (which they rarely comment) inside of their code to improve the visual appearance even if it is not physically correct. I have been revisiting the Unreal Engine 4 presentations and diverse info they have published to improve the integration of the model inside of the engine. I still have lots of work to do with this specific model, I have to integrate IBL ambient illumination into the model the full generation of the IBL textures. I am still considering if this should be done as an external tool or inside of the editor. The main idea is to finish this work this week in order to start working on other PBR models such as Unity's model (which is quite similar to UE4's model) or Disney model (probably the most complex and expensive to compute). I also would like to introduce IBL texture generation for traditional corrected models such as Blinn-Phong or Oren-Nayart which could also produce really good visual results.
![MRComparison]({{ "/assets/images/2018-04-07-UE42.jpg" | absolute_url }})

I still have work to do; however, I think I am near to finish working on this specific model.


### IBL


Image Based Lighting is an illumination technique which requires capturing the ambient illumination of a specific scene and compute the color input for all the possible directions in the scene and use this information to compute the final illumination. Most IBL implementations split the rendering equation into two terms, one for the diffuse term (irradiance) and one for the specular term (radiance). The introduction of these terms is made through a number of cubemaps, 1 for the irradiance and n for radiance, where n depends on the number of textures generated with the roughness/glossiness of the scene. Combining these textures into the shader it is possible to approach the scene illumination to achieve visually realistic results. In the next demo, the textures were generated using a specific tool [cmftStudio](https://github.com/dariomanesku/cmftStudio) I will use these textures as a reference for my sphere convolution implementation.


### Tools


As I have said before, new needs grow during the development and apart from changes in the engine/API, the necessity to automate some processes that are constantly done by hand also appears. For example, I have a different number of scripts to upload, generate and clean the solution and for the resources repository management. But two that have become really important and useful are some scripts that I made using Python for managing and working with textures. As a programmer, dealing with photoshop to resize textures or change their format can be a loss of time; therefore, I am working with two tools: The first tool has some configuration to resize, crop and export textures in different formats. The second tool is able to crop from cubemaps textures, all the subtextures to allow their individual loading inside of the engine. This tool accepts all the possible cubemap configurations (hcross, vcross, strips or nvidia strips) and has proven to be really useful during the unfinished IBL generation.

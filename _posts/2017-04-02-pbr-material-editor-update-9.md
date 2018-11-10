---
author: aborres
comments: true
date: 2017-04-02 21:18:00+00:00
layout: post
link: aborres.com/post/pbr-material-editor-update-9/
slug: pbr-material-editor-update-9
permalink: /post/pbr-material-editor-update-9/
title: 'PBR: Material Editor – Update 9'
thumbnail: "assets/images/2018-04-07-picking.gif"
wordpress_id: 478
categories:
- Post
- PBR-Material-Editor
tags: [Games, Graphics, OpenGL, Post, Projects, Render,
       Rendering, Teesside, UK, University]
---

![Object_Picking]({{ "/assets/images/2018-04-07-picking.gif" | absolute_url }})

There is only one month left to finish this project. The due date is May 8th and there is still some work to do. I would like to use this post to list the things that I would like to integrate, the things I have to integrate and the things I won't probably do. Even more from today to the end I am going to change the post structure updating the editor and the rendering news separately and at some point I will not continue producing more rendering features (as the main object of this project is to produce a tool to work with models).

### IBL

The last step to integrate into my IBL implementation was the radiance map generation. The radiance represents the amount of light that hits the surface from an specific direction and bounces away. As you can see this term is used inside of the specular term of the lighting equation. Unreal Engine 4 model's use 2 sets of enviroment maps plus one extra texture where they codify the BRDF model they are using. The system is easy to use; however, it is not that straightforward to implement. The diffuse contribution is baked inside a cubemap representing the amount of light that reaches one fragment from all directions, the radiance contribution comes from a 2D texture which codifies the BRDF function and a set of N textures where the radiance contribution is stored depending on the roughness value of the surface. What is usually done is to store each diferent cubemap inside of each different mip map level of the radiance cubemap. Rougher surfaces requires less precision to represent reflexes (they become more blurred because of the surface). For example, lets think of a perfect smooth metallic ball, a smooth ball will behave as a mirror does, reflecting the environment with clarity because of the uniform light bounces; however, what happens if we sand this ball? The surface becomes rougher and the light will reflect with more dispersion than before, producing blurred images. Because of this phenomenon we need less precision to produce blurred reflexes; therefore, what most IBL implementations do is to calculate these environments in smaller textures and store them inside smaller mip map levels.

| ![Radiance_Map]({{ "/assets/images/2018-04-07-radiance.jpg" | absolute_url }}){:width="256px"} | ![Irradiance_Map]({{
  "/assets/images/2018-04-07-irradiance.jpg" | absolute_url }}){:width="256px"} |
| *Example of cubemap face* | *Blurred version of the environment* |

The 2D texture only represents the BRDF depending on the roughness and the angle of the cosine of the normal with the view direction; therefore, this texture is environment independant and only changes depending on the BRDF that is being used. The whole process is not computionally complex; however, to approximate integrals can be tough and some times requires different tests and different compensation functions. Although, most models reach the same conclusion if your visuals are good even if your calculations are not, then your implementation is correct.

<p class="align_center" markdown="1">
![Lookup_Texture]({{ "/assets/images/2018-04-07-lookup_texture.jpg" | absolute_url }}){: width="256px"}
<br>
*Look Up Texure*
</p>

### Editor

This week I have been working on different editor features:

The first one is the rebuild and redising of the editor to allow resizing. As you can see in the top gif now it is possible to resize the editor window, now the editor fits its screen to fixed positions instead of allowing the user to place them whenever they want. I thought that to provide a tested and constant user interface would be friendlier with the user rather than to let them to deal with the screen even without knowing what they do.

Secondly, I decided that it would be a great idea to have mouse selection inside of the editor in order to work with models. I have developed and integrated inside of the engine a basic collison system (whithout physics) to allow mouse picking. There are different ways to implement mouse picking, this can be done using the depth of the scene, an easy approach working with deferred rendering, other techniques imply to render the scene providing each model a different color to codify an ID and later recover the selected pixel to decodify the desired ID. However, I decided to implement an OBB AABB system. OBB (Oriented Bounding Box) and AABB (Axis Aligned Bounding Box) are systems frequently used to test collisions in videogames. The main advantage of AABBs is that it is a cheap (computionaly) process. OBBs are more complex to test (or to integrate inside a physics engine); however, 2D OBB vs 2D point is a pretty straightforward test to implement. After checking for all the possible collisions with the mouse, I search for the closest object to the actual camera to find the selected object.

Now the editor allows the user to load HDR images and to transform them into classical cubemaps and save them. The editor also supports texture saving in different formats (.TGA, .PNG, .JPEG, .BMP).

At last, now the editor allows the user to instantiate basic geometric forms inside of the editor if they do not want to work with models or they only want to test basic implementations of features.

### Task List

| **TODO** |
| Integrate IBL generation inside of the editor |
| Integrate material selection inside of the editor |
| Integrate performance measurement |
| Scene save/load |

| **WOULD LIKE TO DO** |
| Integrate more IBL models |
| Integrate more PBR models |

| **PROBABLY NOT DOING** |
| Extensive testing and comparison of different models |
| Integration of MonteCarlo based convolution (for comparisons) |
| Spherical Harmonics |
| Spherical Map support |

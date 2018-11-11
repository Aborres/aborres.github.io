---
author: aborres
comments: true
date: 2017-03-26 18:14:40+00:00
layout: post
link: http://aborres.com/post/pbr-material-editor-update-8/
slug: pbr-material-editor-update-8
permalink: /post/pbr-material-editor-update-8/
feature-img: "assets/images/header_mi.png"
thumbnail: "assets/images/2018-04-07-irradiance_lobe.jpg"
title: 'PBR: Material Editor – Update 8'
wordpress_id: 465
categories:
- PBR-Material-Editor
- Graphics
- OpenGL
- Post
- Project
- Text
tags: [Graphics, OpenGL, post, projects, Render, Rendering, teesside, uk,
       university]
---

At this point of the development I have to focus on the final implementation and in the details, I have left to finish. Most of the work is at least done; however, I still have some details to finish and, more importantly, I still have some parts to implement inside of the editor. The main problem of a relatively big project is the number of possible unexpected changes and necessities that can appear. Sometimes things do not work as you expect or as you planned them and their changes can modify huge pieces of code (the code was making use of the old version). This can bring bugs and useless (for now) blocks of code into your implementation. I always try to only implement what it is really useful at this point and try to not flood my code with functionality that can be or can not be useful or maybe I will never use. This is because I spend time on extra functionality that I do not need right now maybe I will never require those functions and even more if I do not need them now, maybe I won't need them never, ending with lots of lost time.


### IBL


As I have already discussed in other posts Image Based Lightning is based on environment images to illuminate scenes or objects. This is a common technique in most modern video game engines. These environments are usually called lightmaps and they are a representation of the illumination state for every object. This technique can only be performed as static lighting for the objects. The light map generation is a really low task that can not be performed in real time and for the same reason it is only valid for static objects; however, it can be mixed with real time illumination for better results.  For example, UnrealEngine 4 has a specific tool to compile lighting that even allows to setup a server/client state to speed up the process. To compile and generate all the possible light maps inside a map in UE4 can take a long time; however, it improves the performance and the visual results. In comparison, in Unity 5.6 they have introduced new tools to work with lighting and now it is possible to "hot reload"  the lightmaps and see a preview of the final result in real time, facilitating the lighting workflow. IBL workflows usually split the maps into two texture sets (one for the diffuse term and **n **for the specular term, in the same way, that most models split in two the lighting equation). Therefore, in my implementation IBL is split in **irradiance** and **radiance maps**.


#### Irradiance


The first term, the irradiance, represents the intensity of incoming light from all directions of the possible directions inside the sphere. Note: I am calculating only half of the sphere because the other half is considered occluded by the surface itself (this is not completely accurate but good enough and performed in most cases.).

<p class="align_center" markdown="1">
![Irradiance lobe]({{ "/assets/images/2018-04-07-irradiance_lobe.jpg" | absolute_url }})
</p>

To calculate this map it is necessary to sample, to calculate and to average the incoming light from all directions of the sphere. This process is computationally really expensive and can not be performed in real time. What it is usually done is to compute the result of the convolution into a texture and in the lighting phase, only a texture sample is performed to know the actual light intensity for this fragment. This term represents the diffuse term and it is "usually easier to perform" than the radiance term. However, other more complex models where the physical properties have to be taken into account also require more complex irradiance integrations.


#### Radiance


The radiance represents the amount of light received and reflected by the surface per solid angle direction. The radiance affects the specular term, in most models here is where the physical properties of the calculations are taken into account; therefore, it is necessary to calculate one lightmap for every property that affects the equation.


<p class="align_center" markdown="1">
![Radiance lobe]({{ "/assets/images/2018-04-07-radiance_lobe.jpg" | absolute_url }})
</p>


Most models use metallicity and roughness to define their materials; however, metallicity does not affect the equation directly; therefore, we only have to calculate one lightmap for roughness value. Once the material becomes more rugged the quality and the clarity of the shines is reduced. One common technique is to store all the maps in the mipmaps of the texture and this way earn some space and improve the performance and the texture count.


### DOING


Right now I am working on two different implementations of IBL. The first one is based on an uniform sampling using a fixed step to convolute the sphere. The second approach is based on importance sampling. This technique takes into account a fixed number of samples weighing them depending on the importance of the direction. I am also working on some editor features, integrating new options to work with uniforms and to load textures inside of the editor, select your model and convolute the sphere based on that model. At this point, the editor allows to load an HDRi file and transform the high dynamic range image to a 6 textures cubemap.


### TODO


What is really left is to integrate more PBR models inside of the editor, integrate their convolution and test them in real time. In terms of the editor, I still have to work on more editor features such as the irradiance and radiance generation and more options to work with models. I also have to integrate performance measurement for the shading, I have already metrics for the engine main loops and I should extent that implementation for the rendering pipeline.

![Remotery]({{ "/assets/images/2018-04-07-remotery.jpg" | absolute_url }})



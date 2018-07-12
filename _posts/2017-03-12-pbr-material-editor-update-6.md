---
author: aborres
comments: true
date: 2017-03-12 22:40:15+00:00
layout: post
link: aborres.com/post/pbr-material-editor-update-6/
slug: pbr-material-editor-update-6
permalink: /post/pbr-material-editor-update-6/
title: 'PBR: Material Editor – Update 6'
wordpress_id: 432
categories:
- Post
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

### Unreal Engine 4: Physically Based Rendering


Nowadays PBR is one of the most common topics in modern games, rendering engines and artists workflows. PBR is another shading technique based on energy conservation and physical properties. Classical shading techniques use the direction of the light and the position of the eye to the camera to compute the color for the actual pixel. However, surfaces are more complex.

<p class="align_center" markdown="1">
![Plastic Surface Microfacets](http://newsimg.bbc.co.uk/media/images/41372000/jpg/_41372903_id_surface203.jpg){: width="256px"}
<br>
*Plastic Surface Microfacets*
</p>

Even the most smooth and flat surface is compound of microfacets that can reflect and refract the light in a highly complex way (obviously not achieved by traditional shading), in 1967 Torrance-Sparrow introduced a new more complex model in order to achieve better-looking results. New PBR models try to simulate these conditions inside of their calculations, elements like self-occlusion, light dispersion, and microfacets distribution are included in calculating the final color. None of the actual PBR models are physically realistic. Even with the inclusion of microfacets into the equation this is just an approximation and real surfaces are far more complex than these approximations, furthermore, other elements have to be integrated into more complex materials. For example, light scattering through surfaces can be simulated with material layering; however, this is also an imperfect integration. Therefore, modern PBR models are based on the premises of achieving energy conservation and take into account physical properties of the material to compute the final color. For this first week, I have been working with the Unreal Engine 4 PBR model. The UE4 model is a simplification of the Burley model presented by Disney with some modifications and "possible optimizations" for real-time rendering.


### Diffuse Term


Unreal engine's model is using Lambert to calculate the diffuse term. This is one of the biggest differences between the Disney's model and UE4's model. Disney's model uses a more complex equation to compute the diffuse contribution; even more, this equation takes into account physical properties of the material to compute the final diffuse color (which really makes sense). However, lambert is fast and good enough for being used inside of their model.


### Microfacets Distribution


As said the microfacets distribution simulates the distribution of microsurfaces over the surface of the material. Unreal Engine model is using GGX to integrate this term into the equation. As Disney's model does, the GGX/Trowbridge-Reitz is integrated into the equation because of its little overweight in comparison with the traditional Blinn-Phong calculation.

<p class="align_center" markdown="1">
![Microfacets Distribution (GGX)]({{ "/assets/images/2018-04-07-D.gif" | absolute_url }}){: width="256px"}
<br>
*Microfacets distribution based on roughness*
</p>

### Geometric Attenuation


For the G term, the UE4's model is using Schlick attenuation; however with some reparametrization to fit better the GGX model.

<p class="align_center" markdown="1">
![Geometric Attenuation (GGX)]({{ "/assets/images/2018-04-07-G.gif" | absolute_url }}){: width="256px"}
<br>
*Geometric attenuation based on roughness*
</p>

### Fresnel Factor


The Fresnel Factor, sometimes called rim effect, is a parameter to simulate how the light is reflected or refracted depending on the incident angle and the properties of the surface. For the Fresnel factor, they are making use of the typical Schlick's approximation.

<p class="align_center" markdown="1">
![Fresnel Factor (Schilck)]({{ "/assets/images/2018-04-07-F01.jpg" | absolute_url }}){: width="256px"}
<br>
*Fresnel effect*
</p>

Although the Fresnel factor is not really expensive to calculate UE4's model is using an approximation to compute this factor. This approximation was introduced by [Sébastien Lagarde](https://seblagarde.wordpress.com/2012/06/03/spherical-gaussien-approximation-for-blinn-phong-phong-and-fresnel/) in one of his posts. This approximation makes use of a Spherical Gaussian approximation to reduce the number of calculations performed by this factor and as you can see in the graph below both equations are really close to each other. This optimization would be useful if they were using the full Disney's model where the Fresnel factor is taken into the diffuse part as well.
<p class="align_center" markdown="1">
![Fresnel Schilck vs Gaussian Approximation]({{ "/assets/images/2018-04-07-Fresnel.png" | absolute_url }})
</p>


### Final Result


The physical properties used in this model are roughness and metalness; however, other parameters such as the coefficient index at perpendicular angles or the ambient occlusion should be parametrized as well. These parameters differ from Disney's model which takes into account other properties such as specular color, glossiness or clothness. In this demo, you can see some examples of the material with different parametrizations in a black environment. However, the result will change deeply once I continue working with IBL (Image Based Lighting) and I introduce more complex calculations and post-processes.

![UE4Monkeys]({{ "/assets/images/2018-04-07-UE4.gif" | absolute_url }})

As soon I finish working with IBL and UE4's model I will introduce into the engine other models such as the Frosbite, the Unity or the Disney models.  Once this is done, most of the work will be completed.


### Extra features


Apart from the rendering advanced I have been working on some editor features and tweaks. One thing I have done this week is the inclusion of the serialization system inside of the editor. In this demo you can see how I am loading a pre-serialized model inside of the engine, reducing loading times to less than a 6th part.

![Texture Select in editor gif]({{ "/assets/images/2018-04-07-model_loading.gif" | absolute_url }})

Finally, I have been working on the editor rendering performance. One of the things I did poorly was the rendering loop of the editor. Since I began working with graphics I have been using the same structure to upload vertex information inside of the GPU buffers. I know that some systems prefer some formats more than others. For example, Apple recommends to upload all the vertex info consecutive using strides between each element. However, I am used to using each information separately. I am aware that probably uploading the vertex at once could be better because of the memory alignment (as said, in some systems) that's why I did not introduce support for this kind of buffers inside of the rendering API. Instead of that, I decided to convert the expected vertex format to my internal format ending with a really expensive process that could even freeze the entire editor. That's why this week I have spent some time on this change and now the performance of the engine is stable and fast again.

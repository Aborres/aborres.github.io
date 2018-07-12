---
author: aborres
comments: true
date: 2017-01-15 02:10:57+00:00
layout: post
link: aborres.com/post/mind-fighters-update-6-post-process/
slug: mind-fighters-update-6-post-process
permalink: /post/mind-fighters-update-6-post-process/
title: 'Mind Fighters: Update 6 – Post Process'
wordpress_id: 160
categories:
- Fighter
- Games
- Post
- Project
- Text
- Mind-Fighters
---

Post processes are commonly used in video games, video editing and image processing. Post processes can create different effects on games, colour filters, homogenise colour effects, or even more complex effects like screen space ambient occlusion or anti-aliasing. Post processing is one of the last steps a graphical engine performs before showing next frame on screen. Post processes are code inside of the GPU that can process the final image to create some effects. For example, it is possible to create cell shading toning in screen space using light information and textures from the engine with objects position's and objects normal's or it is even possible to create dynamic effects like motion blur or volumetric fog.

In our game we decided to introduce multiple post process:
 	
  * Antialiasing: This is a method to reduce aliasing from images. In other words, this technique is performed to reduce the saw edges of images.

![AA: image comparison](http://international.download.nvidia.com/geforce-com/international/images/dark-souls-ii/dark-souls-2-sgssaa.gif)

source: http://www.geforce.co.uk/whats-new/articles/dark-souls-ii-tweak-guide
 	
  * Depth of field: DOF includes effects of real cameras in a 3D camera render, specifically, recreates the focal distortion produced by the lens of a camera by distance.

![DOF](https://cdn.fstoppers.com/media/2014/08/dof_visual_0.gif)

  * SSAO (Screen Space Ambien Occlusion): When a ray of light hits one point of a surface the light separates into refracted light and reflexed light. The amount of refracted light depends on the surface properties. The refracted ray will be of the color of the hit surface and if that ray hits another surface it will contribute that surface with its color. That interaction is what SSAO simulates.

![SSAO gif](http://international.download.nvidia.com/geforce-com/international/images/dark-souls-ii/dark-souls-2-ambient-occlusion.gif) http://www.geforce.co.uk/whats-new/articles/dark-souls-ii-tweak-guide[/caption]

  * Blur: Blur effect only slightly distorts the image.
 	
  * Edge detection: This filter searches for edges and highlights them.

![https://www.shadertoy.com/view/MdGGWh](http://aborres.com/wp-content/uploads/2017/01/e63ca4d44cce02cc1e4a0d1185da1fb6.gif) https://www.shadertoy.com/view/MdGGWh[/caption]
 	
  * Bloom: This effect is produced in real life. If the naked eye looks to a light source or a bright object on a much darker background, the brighter object will cause flares or streaks over the darker objects.


![Bloom post process](https://media.giphy.com/media/3oxRmDSEUIrCxhHz7G/giphy.gif) http://polycount.com/discussion/165039/new-introducing-amplify-bloom-industry-level-post-process-bloom-for-unity[/caption]

  * Tilt Shift: This effect is similar to Depth of field, it generates a miniature effect on the scene (blurred edges and smallness sensation).


![Tilt Shift post process](https://s-media-cache-ak0.pinimg.com/originals/44/07/ad/4407ad66b375cd9e5a03b842e5e8a861.jpg) https://www.pinterest.com/garymcelveen/photography-tilt-shift/[/caption]

![Example of post process in unity](http://aborres.com/wp-content/uploads/2017/01/7ed37dc01c5fe5eea8655bc2e8ad10ec.png)

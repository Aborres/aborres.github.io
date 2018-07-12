---
author: aborres
comments: true
date: 2017-01-16 18:46:49+00:00
layout: post
link: aborres.com/post/mind-fighters-update-10-lights/
slug: mind-fighters-update-10-lights
permalink: /post/mind-fighters-update-10-lights/
title: 'Mind Fighters: Update 10 – Lights'
wordpress_id: 221
categories:
- Fighter
- Games
- Post
- Project
- Mind-Fighters
---

Light baking is one of the end processes that has to be done in every map after their completion. Light baking is important because real-time illumination does not generate any kind of light bounce; therefore, elements like ambient occlusion or the light ray's interaction are not considered so realistic global illumination can not be achieved. However, Unity provides a tool to generate light maps of every light source. The process has several steps:



 	
  1. First, I defined all the static objects of the scene. This is important because baked light only affects static objects in the scene. Thankfully, in our game most of the objects on the scene were static, backgrounds and the environment are not supposed to move in our fighting game.

 	
  2. Secondly, I differentiated on real time lights, mixed lights and baked lights. Most of our point lights in the scene were supposed to only affect environment so, I configured most of them as baked lights. Our artists placed spotlights to illuminate certain spots of the game and the way they placed them allowed me to configure them as mixed lights. Mixed lights have their own lightmap for static objects but they are also computed in real time. I found this important because they were placed on spots where the player is supposed to pass through so their characters should be illuminated. I configured our directional lights as real time because our artists were only using them as "fake ambient lighting" but they were generating all the shadows in the game so we needed them as real time lights.

 	
  3. Finally, I configured the baking process in the unity tool and it took between 2 to 3 hours per scene to compute lighting. When this process is done Unity uses the light maps in the static objects to compute the incoming illumination of those objects instead of calculating it in real time.


As you can see, I decided to use simultaneously baked global illumination and real-time illumination. Baked global illumination is expensive in terms of video memory, the engine also has to make texture access for each frame for every pixel and light map, something that can be really expensive as well. However, modern GPUs can handle these problems and I thought this was the best configuration for our project.

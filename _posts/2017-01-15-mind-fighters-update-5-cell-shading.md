---
author: aborres
comments: true
date: 2017-01-15 01:48:00+00:00
layout: post
link: aborres.com/post/mind-fighters-update-5-cell-shading/
slug: mind-fighters-update-5-cell-shading
permalink: /post/mind-fighters-update-5-cell-shading/
title: 'Mind Fighters: Update 5 – Cell Shading'
wordpress_id: 148
categories:
- Fighter
- Games
- Post
- Project
- Mind-Fighters
---

Once most of the programming work and the artwork is almost complete it is time to start including visual effects in our game. Particles, post-process, extra animations and scripts can drastically improve the way our game is perceived by the player.  For this game, I have written different shaders to improve our visual appearance and lots of different particle effects.

First, my artists asked me for a cartoonish style, they wanted to achieve a style similar to Zelda: Wind Waker but with more shadow detail so I decided to modify the main deferred PBR shader of Unity to include this effect in all our assets (those with PBR materials).

![Zelda: Wind Waker picture](http://www.zeldadungeon.net/Zelda09/Walkthrough/01/035.png)

In its main page, Unity provides the source code for the default materials, it is possible to tweak those files to include custom shading inside the "defaults" of the engine. Cell shading is a common technique in cartoon games. This technique consists of creating different ranges where the amount of light reflected will remain constant. Surprisingly, Unity's default shader uses Lambert illumination (dot(N, L)) for the diffuse term, which is quite simple for modern PBR models; however, it is commonly used in lots of models because of its basic calculus and performance. Lambert is probably the most basic illumination model, because of its simplicity it was easy to integrate cell shading into our game. I created 5 ranges of illumination based on my artists requests to separate light.

![Cell Shading Code](http://aborres.com/wp-content/uploads/2017/01/d46a4e58444c893fa38c2793ce61b7e4.png)

![MPD](http://aborres.com/wp-content/uploads/2017/01/41d6b9c7edc7227242a99b17ece1a296.png)



Zelda's cell shading appears to be simpler, its technique only differentiates two ranges of color, one darker for the shadows and the other one the light reflection; however, our artists preferred to have more color differentiation to take advantage of their textures. We considered to implement cell shading as a post process into the game, but the artistic team didn't want to have the cell shading effect as an effect for the global scene. In Unity it is possible to filter and order objects by layers and to apply different effects depending on the layer; however, the material option seemed to be easier and more affordable for the artistic team.



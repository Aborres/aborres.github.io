---
author: aborres
comments: true
date: 2017-01-16 18:44:55+00:00
layout: post
link: aborres.com/post/mind-fighters-update-8-sound/
slug: mind-fighters-update-8-sound
permalink: /post/mind-fighters-update-8-sound/
title: 'Mind Fighters: Update 8 – Sound'
wordpress_id: 217
categories:
- Fighter
- Games
- Post
- Project
- Mind-Fighters
---

Sound effects are really important to increase feedback in games. Particles, post process, materials and sound effects can deeply change how a game is perceived by the player. In our game, we didn't introduce much sound. Our designers searched for open source clips and I integrated them into Unity.

Unity splits sounds, like most of the engines, in two separated elements, an emitter and a receptor. The receptor is usually placed on the camera or in the player (in our case the receptor is on the camera) and the emitters are placed around the map. This configuration allows the engine to perform calculus in terms of distance or the relative velocity between the two elements. It is simple to use; however, it ends with good results. I integrated the sounds for the hits, the deaths and the background music.

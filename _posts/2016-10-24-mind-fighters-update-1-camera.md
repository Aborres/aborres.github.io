---
author: aborres
comments: true
date: 2016-10-24 00:01:53+00:00
layout: post
link: aborres.com/post/mind-fighters-update-1-camera/
slug: mind-fighters-update-1-camera
permalink: /post/mind-fighters-update-1-camera/
title: 'Mind Fighters: Update 1 - Camera'
wordpress_id: 52
categories:
- Fighter
- Games
- Post
- Project
- Mind-Fighters
tags:
- bross
- figther
- games
- post
- projects
- smash
- teesside
- uk
- university
---

As our project is a smash bros like game, the camera results to be a really important element in our game. If we were making a street fighter like game the camera would have been a pretty straightforward task to do. But in our game, the camera has to resolve so many problems that it could doom our game. With this in mind, we make a list of requirements about what the system has to resolve:



 	
  * The camera has to be able to show all the players at the same time on screen (up to 8 players).

 	
  * The camera has to be able to rotate and reposition itself in order to show enough space for the player to foresee other players actions.

 	
  * The camera has to have a set of tools to allow our designers to create planes, effects (shakes and turnings), etc.

 	
  * The camera has to have enough configuration to allow the designers to create whatever they want to.




With this in mind, I started thinking about how to resolve the movement and I tried many different approximations to this problem. At first, I developed a system where the camera generated planes and each plane contained a point with the position of the player as the middle point, with a percentage of rotation respect to the origin. With all of these planes, I managed to intersect them and average them to obtain a valid position and a valid rotation. The plane system seemed to work but not good enough and I was not really able to resolve the vertical align problem.

My second solution was to approximate a circle which contains the players as points of itself. But this system had pros and cons. The radius of the circle was fixed otherwise it would have been another unknown quantity in my equation system, in contrast, it was easy to determine the final position of the camera to contain the players on screen but this solution had the vertical problem as well and it was hard to configure in order to obtain good results.

![camera](http://aborres.com/wp-content/uploads/2016/10/camera.png)

Thanks to these tests I ended with the final solution that I chose to use in our game. The idea is pretty easy, fast to implement and gives us good performance. The algorithm is simple, I calculate which are the furthest players in both axis. With these points, and knowing the camera FOV, I create two triangles, one in the (X, Z) system and another in the (X, Y, Z) using the data of the previous triangle. This system has the benefits of both previous systems. With the triangles, I can calculate the final camera position to fit the furthest players on screen and to obtain the correct camera rotation to move between different configurations of "scenes".

![camera-unity](http://aborres.com/wp-content/uploads/2016/10/8ec523725c04ce6c01940dcb9d77d416.gif)

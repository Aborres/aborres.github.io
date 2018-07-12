---
author: aborres
comments: true
date: 2016-11-02 15:45:52+00:00
layout: post
link: aborres.com/post/mind-fighters-update-2-input-reading/
slug: mind-fighters-update-2-input-reading
permalink: /post/mind-fighters-update-2-input-reading/
title: 'Mind Fighters: Update 2 - Input Reading'
wordpress_id: 102
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
- teesside
- uk
- university
---

Reject input is a common mistake in most modern video games. Most developers do not care about how to properly process input in their engines and the world's biggest game engines are proof of that reality. Unity engine (almost the current version 5.4.1f1) has a really bad input manager and, Unreal Engine 4 (4.12.5), its direct competitor, does not do it any better. In Unreal Engine, talking about local multiplayer games, the only one who receives the actual input is the first player and others have to ask him for their input...

I have been using Unity since version 3 and the input manager continues being the same bad code system. For example, if you want to make a local multiplayer game in unity you have to map every key for as many controllers you are going to support in your game. If your game is a one vs one local multiplayer maybe it is good enough but what if you are orientating your game to 8 players in local cooperative mode? It is tedious to map every single key for every single gamepad.

Although, the input task does not end there. Once you have your input mapped in the input manager you have to ask for every different controller and for every possible key of the gamepad and, in multiplayer fighter games that can cause lag while processing your input.

As this project is only a final module work, I decided that I could lose some of my time mapping every key (I also reduced my game to 4 players in local multiplayer) and I could generate a input wrapper over the original input manager to only ask for the desired input depending on the character and, what I encountered is that Unity does not ensure that the controller with the first light on (almost speaking about Xbox 360 controller, which is probably the most common controller on PC) is going to be the first player. I mean, if you define that your "P1_Horizontal" is coming from the first Joystick, Unity does not ensure that the first joystick is from the first gamepad. My trick here is that when players enter the game I ask them to press "Start button" to join the game, and when they do it I associate them with the correct player in the game.

Furthermore, I asked some partners about their solutions over Unity's input system and they had the same response: "use an external library". I will keep my solution in this project but probably in future projects, I will choose to use the steam's input library which looks to be a better-designed manager.



---
author: aborres
comments: true
date: 2017-01-16 18:47:20+00:00
layout: post
link: aborres.com/post/mind-fighters-update-11-performance/
slug: mind-fighters-update-11-performance
permalink: /post/mind-fighters-update-11-performance/
title: 'Mind Fighters: Update 11 – Performance'
wordpress_id: 223
categories:
- Fighter
- Games
- Post
- Project
- Mind-Fighters
---

Because of the design of our game, gameplay has not been a real problem in terms of performance. In a fighting game, gameplay logic can be simple or at least cheap to compute. Even so, when we started developing our game I thought which could be our most expensive (in terms of computation) task and I found that input processing and action executing was going to be our hot spot in the logic. I designed a system similar to a compiler parser. The idea was to create an easy to extend and fast system. The system consists of series of logical functions queued by series of logical operators. The last action processed by the input manager will enter the system and pass from one logical function to the next one until one of them consumes the input if that happens the processing process ends this frame.

Although gameplay should not generate a big impact on performance, graphics, particles and post process are a huge consuming task. We decided to spend our spare performance on graphics and visual effects. We introduced too many particle emitters in our maps and they were causing lag in our game. I cleaned the number of particle emitters in our maps and I also reduced and optimized some scripted effects which were taking long computation times (firefly emitter or fireworks).

Finally, light computation is also a big task (Update 10 - light post) and as I explained I introduced baked lighting in our game to improve performance. However, some of the post process we were using were taking long computation times, some of them used multiple texture access, random sampling and long loops inside of them, but as they were necessary I optimized them as much as possible, also we were using PBR materials in our games which can take more time to compute than traditional shading. I modified the default PBR shader in Unity; however, my modification introduced more latency to generate cell shading effect.

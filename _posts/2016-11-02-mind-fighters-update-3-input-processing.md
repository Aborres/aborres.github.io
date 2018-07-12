---
author: aborres
comments: true
date: 2016-11-02 16:38:46+00:00
layout: post
link: aborres.com/post/mind-fighters-update-3-input-processing/
slug: mind-fighters-update-3-input-processing
permalink: /post/mind-fighters-update-3-input-processing/
title: 'Mind Fighters: Update 3 - Input Processing'
wordpress_id: 115
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

Processing input for a fighter game is not an easy task. Multiple combos and the combination of actions with all the other buttons and all the directions can create a really painful experience when generating the skill tree for a character. In our game, we considered 4 action buttons with 4 possible directions, that makes around 400 possible interactions to generate movements without considering double-tapping the same button to create a combo... We do not have enough time or a big enough animation team to support all these variations (we are not that crazy though) so we have decided to use 4 directions with 4 buttons (no button combination) instead which creates 16 possible interactions and, of course, any character is going to use the total range of buttons. Moreover, each action is associated with one animation and has to end in an attack which the combat system has to process and every attack can be stopped by an enemy hit. As you can see, after all our reductions the range of possibilities is still huge and hard to combine.

![chung li kicking](http://aborres.com/wp-content/uploads/2016/11/giphy.gif)

With all of this, the best approach that came into my mind was to develop a process relatively similar to a compiler parser. The system has 4 different parts:

  * Input reading
  * Input processing
  * Character processing
  * Character storing

The first phase is the input reading. The first thing I did was create a manager which depending on the actual player, parses all the buttons that the player can use (gamepad/keyboard), and stores all the actions of this frame into a buffer. This phase also reads each axis and stores the active value of the joysticks in the buffer.

The second phase aim is to clear some possible fake actions in the buffer, for example, inactive joysticks or inactive buttons for the actual player.

Once the buffer is clean and ready for processing, the character processing unit reads the buffer and starts to parse depending on the primordial preferences of the actual player. For example, a really basic workflow could start parsing if the player has to run any action from the last frame input or from the combination of the last input with the unconsumed buffer commands. If the actions parser does not consume the last input, the last input enters the next parser. We could have decided that jump is more important than movement then, the player will check if he has to jump, if not, the last command will enter into the next unit. The last unit could be the movement itself, if the last action was a movement then the player will move and no command will be stored in the buffer, otherwise, the actual action will be stored in the persistent buffer of the player for later possible actions.

The character storing is a simple unit that tries to pack in the best way possible the commands into the persistent buffer and tries to predict future movements that the player will realize. Working this way, I managed to create a stable system that was easy to extend with future actions or different fighters.

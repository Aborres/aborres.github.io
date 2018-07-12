---
author: aborres
comments: true
date: 2017-01-16 18:45:22+00:00
layout: post
link: aborres.com/post/mind-fighters-update-9-git-tools/
slug: mind-fighters-update-9-git-tools
permalink: /post/mind-fighters-update-9-git-tools/
title: 'Mind Fighters: Update 9 – Git & tools'
wordpress_id: 219
categories:
- Fighter
- Games
- Post
- Project
- Text
- Mind-Fighters
---

During the development of the project, we have been working with multiple different tools. One of the most important has been git. Git is a control version system developed by Lynus Torvalds in 2005. Git tracks all the files in the project and keeps the changes in each of them during the development. Having copies of all your work is very useful and can drastically speed up developing time. For our project, I created a Git repository in a private gitlab server. I separated our repository in 4 branches (plus some extra branches for crunch sessions or tests), one branch for me, one for the other programmer, one branch for the art team and the last branch for the designing team. Working this way we were able to work quietly and merges were only done under my supervision. I have worked on projects before and I am currently working in a company and I have found this is the best way to avoid conflicts between everybody's work.

![Vim logo](https://vim.sexy/img/Vimlogo.svg)

For coding, I have been using vim as the principal text editor. Vim is an old school text editor based on Unix and Amiga editors. Some of Vim features are code completion, regular expression evaluation inside of the editor commands, plugin support, spell checking, tabbed windows, syntax highlighting, and overall, command support. The big Vim advantage over other editors is that vim allows you to codify inside vim commands which highly improves your efficiency and reduces your text editing time. Vim is portable, multi-platform, super fast and extremely efficient; however, the learning process to start using Vim can be hard and most people would prefer to use other common text editors such as Sublime Text or Notepad++.



We have been using other tools for work management such as trello or slack. In trello all the tasks of a project are distributed in boards, each board contains lists with cards. Every card is supposed to represent a single task, and tasks can be to do, doing or done. Trello is designed to help teams to administrate their time and to organize their tasks. Slack is a communication tool to communicate members of the team, slack allows to send files, send code snippets and more much functionality. However, all the assets and the documentation files have been stored in google drive. At the beginning, I created a gmail account for the team and all our information is stored in that account.

![SD - Trello](http://aborres.com/wp-content/uploads/2017/01/312ad9e0bee20ea6bc836cd426f69fe0.png)

I created every board for every team and I distributed every member of the team on the boards; however, it was the responsibility of each team to use trello as they wanted to. They were responsible for distributing their tasks and completing them in the supposed time. Finally, I have been using Visual Studio for compiling and code debugging. Vim is my main text editor; however, Vim is not an IDE, vim does not integrate debugging tools; therefore, I decided to work with Visual Studio, the default Unity IDE on Windows. It would have been painful to configure Vim on every computer on which I have worked; therefore, I have been using VsVim in the university. VsVim is an open source plugin to emulate Vim capacities on Visual Studio.

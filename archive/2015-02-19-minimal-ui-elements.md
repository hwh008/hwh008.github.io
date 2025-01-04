---
layout: post
title: 'Minimal UI Elements for Game'
date: 2015-02-09
author: huang
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: gamedev
---

Elements for Game
-----------------

 1. Banner in first screen
 2. Button frame
 3. Window frame
 4. Next and prev button
 5. Normal icons: config, close, star, heart, chest, locker, gem, coin, pause
 6. Progress bar
 6. Choose level: normal and boss, lock and unlock
 7. List frame
 8. Grid frame
 9. In game background and tiles.
 10. Poster

First Screen
------------
It is composed by 2 things.<br>
**Background**

It can be the clean color or a drawing. You can translate the drawing repeatly.
![firstscreen](min-ui/firstscreen1.jpg)

**Title and Characters**

You can show the title with big font only. you can also add some game characters behind the title.
You can add some float animation to the characters.
Only 1 thing is dynamic in both background and title's elements.
![firstscreen](min-ui/firstscreen2.jpg)
![firstscreen](min-ui/firstscreen3.jpg)

Button Frame
------------
You can draw some delicate button, or just use a clean color box with shadow as your button, but remember it should be enlarged as a frame.

Window Frame
------------
Normal border and clean color inside the content area, or try more simple design without border.
![window](min-ui/window1.jpg)

Choose Level
------------

 - A background which is simple drawing or clean gradient color
 - A title as *'CHOOSE LEVEL'*
 - Level icons
 - Next and prev button to shift level page

A simple design is showing 5x5 grids for levels of each section, each section use different background.
![chooselevel](min-ui/chooselevel.jpg)

List Frame
----------

 - Title and description
 - Neighbor item must be distinct, you can make this by light/dark color or item border
 - Item left is icon
 - Item middle is title and description
 - Item right is an event button
 - If the count of items more than 1 page,  do not use vertical scroll, flip page by next and prev button

![listframe](min-ui/listframe.jpg)

Grid Frame
----------

 - Clean color as background
 - Title on top
 - Item icon and selected cursor
 - Show description on bottom when item is selected
 - Do not use vertical scroll, use flip page

![gridframe](min-ui/gridframe.jpg)

In Game Background and Tiles
----------------------------

 - Background -1 is a static shade, as paint by brush with clean color, it don't contain any detail. or you can use a sky drawing by gradient color
 - Background 1 is the near sight, it move follow by character
 - It must need 2 small tiles and 1 big tiles at least, and shade them with 2 colors, then you get 4+2 tiles

![scn1](min-ui/gamescn1.jpg)
![scn2](min-ui/gamescn2.jpg)

Poster
------

 - Title
 - Banner
 - Description
 - Event button on bottom with different color background

![poster](min-ui/poster.jpg)


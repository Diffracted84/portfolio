---
layout: posts
title: The State Machine
date: 2026-05-19
categories: [Programming, Python, Game Dev]
#tags: [pygame, scroller]
#project:  "SideScroller"
excerpt: "Building the backbone of the game."
permalink: /statemachine
#header:
#  image: /assets/images/androgogic-header.png
#  teaser: /assets/images/androgogic-teaser.png
words_per_minute: 100
---
## Overview
I have been playing with making a game using the pygame framework for a few months now. It has been a learning curve, but I am starting to get the hang of it.
A few days ago I realised that game is ultimately a state machine. There are several states within this machine that need to be considered. So I started exploring how I would refactor my infant project to make states a reality.

## Planning
To start with I needed to do some research on how to implement. I have had previous experience using a state machine in LabVIEW, but not in python. I found what I thought was a really good pair of tutorials on pygame/python implementation by watching [Pygame Game States Tutorial](https://www.youtube.com/watch?v=b_DkQrJxpck) by [CDcodes](https://www.youtube.com/@CDcodes) and [PyGame State Machine - Python Game Dev Tutorial](https://www.youtube.com/watch?v=PZTqfag3T7M&t=512s) by [It's That Ian Guy](https://www.youtube.com/@itsthatianguy). If this is something you are looking to do, these are worth a watch.
Secondly, I needed to plan what my states are going to be. A bit of brainstorming and I came up with the plan below:
<Insert figure here>

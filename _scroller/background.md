---
title: Parallaxing Backgrounds
layout: single
index: 2
excerpt: Layering the world behind the player.
permalink: /projects/scroller/background/
date: 2026-08-06
last_modified_at: 2026-08-06T12:46+10:00
show_date: true

read_time: true
words_per_minute: 200

categories: [Programming, Python, Game Dev]
tags: [pygame, scroller, parallax]

author_profile: true
author: Casual

toc: true
toc_label: Jump to Section
toc_icon: book
toc_sticky: true

comments: true
share: false
related: false

header:
  image: /assets/images/projects/sidescroller/background/header.png
  #caption: Image by [artist](https://pixabay.com/) from [Pixabay](https://pixabay.com)
  teaser: /assets/images/projects/sidescroller/background/teaser.png

gallery_layers:
  - image_path: /assets/images/projects/sidescroller/background/layer_mount.jpg
    alt: Mountain layer
  - image_path: /assets/images/projects/sidescroller/background/layer_hill.jpg
    alt: Hill layer

gallery_working:
  - image_path: /assets/images/projects/sidescroller/background/background_00.png
    alt: One frame of animated background.
  - image_path: /assets/images/projects/sidescroller/background/background_12.png
    alt: The same background 12 seconds later.
---
## Overview
A side-scroller needs somewhere to scroll through, and a single flat background makes that motion look like a little off. Real depth comes from layers moving at different speeds - the sky barely shifting, the mountains behind that a bit more, hills in front of those more again.

It's the same effect as looking out a car window: the fence right next to the road blurs past while the hills on the horizon barely seem to move at all. 

Each background layer in my game is its own sprite, and the only thing that separates them is a parallax multiplier applied to the same game scroll speed, for which i used the variable `runspeed`.

{% include gallery id="gallery_layers" caption="Examples of background layer sprites. Black represents transparency." %}

## Tiling the Layer
A background image is finite, but the scroll isn't, so each layer draws itself several times side by side and wraps the offset back to zero once it's scrolled a full tile width:

```python
self.x_pos = int(-1 * (-self.precise_xpos % self.rect.width))
```
**Info:**
You would think that a negative multipled by a negative number would make a position, so "why not just leave it positive?". I thought that too, but it turns out that in python the modulus operator does care. Not having the first negative number ensures that the background scrolls from right to left.
{: .notice--info}

Think of a scroller background like a conveyor belt loop rather than a single long banner - once a tile scrolls fully off one side, the modulo just hands the same tile back in from the end. Coupled with a seamless image and the join never shows.

## The Bug That Wasn't in My Code
Wrapping initially refused to work even though the numbers checked out. The reason turned out to be pygame itself.

I was lumping my background sprites into a sprite group. `pygame.sprite.Group.draw()` doesn't call each sprite's own `draw` method - it blits `sprite.image` at `sprite.rect` and nothing else. Even though I had built a custom `draw` method within the background class to handle the tiling loop, the group was quietly ignoring it and drawing one untiled image instead. The fix was to stop trusting the sprite group with drawing at all and call each sprite directly:

```python
for sprite in self.bg_sprites:
    sprite.draw(surface)
```

## Chasing a Speed That Kept Resetting
Parallax depends entirely on `runspeed` reaching each layer correctly, and mine kept arriving as zero. I was accidentally recalculating the value fresh every frame in a spot that also reset it under certain conditions, so any layer reading it after that point saw nothing.

On top of that, the value that governs scrolling was only meant to refresh once a second rather than every frame - without that, acceleration stutters instead of ramping smoothly. I removed the resetting from the game loop.

I also wanted to cap the speed at a maximum value, otherwise the game would eventually be to fast and impossible to play.

```python
def gamespeed(self) -> None:
    if (0 < self.runspeed <= self.runspeedmax):
        self.runspeed += self.runspeedinc
        self.temprunspeed = self.runspeed
    elif self.runspeed == 0:
        self.runspeed = self.temprunspeed
```

The above method (within the game loop) keeps a running speed that increments toward a cap, and only falls back to a cached value in the specific case where scrolling has been paused to zero (this is a planned feature upon hitting an obstacle).

{% include gallery id="gallery_working" caption="An example frame and another taken 12 seconds later" %}

Don't worry, the black bar at the bottom of the window is deliberate. That is where ground tiles will eventually live.
{: .notice--info}

## Conclusion
None of this was hard once I figured it out, but silent bugs were actually breaking the code without my seeing what was really happening. Parallax itself turned out to be the easy part; trusting that the numbers feeding it were what I thought they were was the real work.

## Attributions
I am using some great free sprites from the following artist:

[Bevouliin](https://dribbble.com/bevouliin)

Check out their work.
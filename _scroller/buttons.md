---
title: Buttons
layout: single
index: 1
excerpt: Building a class for user interaction.
permalink: /projects/scroller/button
date: 2026-07-10
last_modified_at: 2026-07-11T12:00+10:00
show_date: true

read_time: true
words_per_minute: 200

categories: [Programming, Python, Game Dev]
tags: [pygame, scroller]

author_profile: true
author: Casual

toc: true
toc_label: Jump to Section
toc_icon: book
toc_sticky: true

comments: true
share: false
related: true

header:
  image: /assets/images/projects/sidescroller/buttons/header.jpg
  caption: Image by [nafeti_art](https://pixabay.com/users/nafeti_art-5143689/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=8120993) from [Pixabay](https://pixabay.com)
  teaser: /assets/images/projects/sidescroller/buttons/teaser.jpg

single_layout_gallery:
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/noicon.png
    alt: "Procedural button with no icon."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/lefticon.png
    alt: "Procedural button with icon on left of label."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/righticon.png
    alt: "Procedural button with icon on right of label."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/iconmulti.png
    alt: "Procedural button with icon on left of label and wrapping around multiple spaces."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/iconsingle.png
    alt: "Procedural button with icon on left of label single word with no wrapping."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/lefticonhover.png
    alt: "Procedural button in hover state."

toc: true
toc_label: Jump to Section
toc_icon: book
toc_sticky: true
---
## Overview
The latest task I have given myself still isn't directly involved with making a game. It is making user interface buttons. I want the user to be able to navigate a menu screen with ease using a mouse and maybe later by keyboard. In my opinion, buttons are a great way to allow this.

## What Makes a Good Button
Before writing any code I wanted to be clear on what I was actually building, because "draw a rectangle with some text on it" isn't really a button, it's a label that happens to sit still.

Boiled down, I think a good button needs to do two things. First, it needs to look appealing and match the theme of the context it's in - in this case, a game, so it should feel like part of the game rather than a generic UI widget dropped on top of it. Second, and just as important, it needs to operate smoothly and predictably. However it's styled, hovering and clicking should behave the same way every time.

That second point is really what shaped the class structure below - the visuals can vary as much as I like, as long as the underlying behaviour doesn't.

## The Parent Class
Every button, however it ends up looking, needs to know the same three things: is the mouse over it, has it been clicked, and where does it draw itself. That's the job of the `Button` parent class - it doesn't know or care how its surfaces were made, only how to react once they exist.

```python
class Button():
    def __init__(self, centre: tuple[int, int]) -> None:
        #button spatial information
        self.centre: tuple[int, int] = centre
        self.hover: bool = False
        self.clicked: bool = False
        self.action: bool = False
```
Nothing fancy at init - just position and three state flags. The actual visuals get handed in afterwards by a child class, via a small dataclass:

```python
@dataclass
class ButtonSurfaces:
    background: pg.Surface
    hover_bg: pg.Surface
    foreground: pg.Surface
    hover_fg: pg.Surface
```
`ButtonSurfaces` is just a bundle of four pre-rendered surfaces - background, hover background, label, hover label. Whatever subclass I write later, its only job is to produce one of these and hand it to the parent:

```python
def make_the_button(self, surfaces: ButtonSurfaces) -> None:
    #button states
    self.surfaces: ButtonSurfaces = surfaces

    #background
    self.image: pg.Surface = self.surfaces.background
    self.rect: pg.Rect = self.surfaces.background.get_rect(center = self.centre)
    
    #button foreground
    self.label: pg.Surface = self.surfaces.foreground
    self.label_rect: pg.Rect = self.surfaces.foreground.get_rect(center = self.centre)

    #make the collision mask
    self._get_mask()
```

### Hit Detection
A `pg.Rect` is only ever a bounding box - useful, but a circular button still has a rectangular hitbox if that's all you use. Think of it like a doorknob: the door frame around it is rectangular, but you don't expect to open the door by pressing the corner of the frame. So I use pygame's mask module to build a pixel-accurate collision shape from the button's actual image:

```python
def _get_mask(self) -> None:
    #this should be the dame for all the ploymorphs - do it here.
    self.surface_mask: pg.Mask = pg.mask.from_surface(self.image)
```
Checking the mouse position then happens in two passes - broad, then fine:

```python
def check_mouse_position(self, screenratio: tuple[int, int], position: tuple) -> None:
    #was the mouse in the button when clicked?
    scaled_pos: tuple[int, int] = cnv.scale_mouse_pos(screenratio, position)

    #check if scaled mouse in rect first:
    rel_x: int = scaled_pos[0] - self.rect.left
    rel_y: int = scaled_pos[1] - self.rect.top

    #check if in rect and then apply a mask collision detect
    if (rel_x in range(0, self.rect.width) and 
        rel_y in range(0, self.rect.height)):
        #now apply mask and see if colliding
        if self.surface_mask.get_at((rel_x, rel_y)):
            self.hover = True
            #check clicks
            self.check_clicked()
        else:
            self.hover: bool = False
    else:
        self.hover: bool = False
```
First the mouse position is scaled to account for the game window not necessarily matching the native resolution. Then it's a cheap rectangle check - is the mouse even in the ballpark - before the more expensive per-pixel mask check runs. No point asking "which pixel is this?" if the mouse isn't near the button at all.

### Click Debounce
Reading `pg.mouse.get_pressed()` directly would fire the action every frame the button is held down, which isn't what a click means. I wanted one action per press, the same way a doorbell only rings once no matter how long you lean on it - you have to let go and press again.

```python
def check_clicked(self) -> None:
    self.action = False
    #check if the left mouse button has been clicked
    if pg.mouse.get_pressed()[0] == True and self.clicked == False:
        self.clicked = True
        self.action = True
    if pg.mouse.get_pressed()[0] == False:
        self.clicked = False
```
`self.clicked` tracks whether the button is currently being held, and `self.action` only goes true on the single frame the mouse transitions from up to down. `hoverstate` and `clickstate` then just expose these two flags as read-only properties for the rest of the game to query.

The remaining methods on the parent - `make_button_surfaces`, `change_colour`, `draw` - are empty on purpose. They're the contract each child class has to fulfil, since only the child knows how its own surfaces are built and coloured.

## Procedural Buttons
A procedural button draws its own background and label from scratch, rather than relying on artwork. That gives me full control over colour, hover state and layout without needing a designer or an image editor open at 11pm.

This one is mid-refactor, so it's really the story of one class, not two. The current `ProceduralButton` handles icon placement and label wrapping well, but leans on external crop/overlay mask images for anything beyond a plain rectangle - which meant reaching for image editing software in an otherwise fully procedural pipeline just to get a rounded corner. So I've started rebuilding it as `NewProcButton`, which draws its own shapes and shading instead of using masks. Once it also has the icon and label logic ported over, `NewProcButton` becomes the new `ProceduralButton`.

### What the Current Version Does Well
Icons can sit to the left or right of the label, and the label wraps to a second line if it doesn't fit in the remaining space:

```python
def _icon_corner(self, icon_rect: pg.Rect) -> tuple[int, int]:
    #return the left position of the icon surface for the button
    if self.icon_position == icopos.LEFT:
        icoleft: int = self.padding
    elif self.icon_position == icopos.RIGHT:
        icoleft: int = self.reference_rect.width - (icon_rect.width + (2 *self.padding))
    ...
```
The wrap logic itself is deliberately simple rather than clever - it splits the label on the space closest to the middle of the string, or just cuts it in half if there's no space at all. It's not something I'd reach for anywhere text length is unpredictable, but it's enough for short button labels.

{% include gallery id="single_layout_gallery" caption="Various layout configurations possible with ProceduralButton class." %}

### The Part Being Rebuilt
Shapes are now drawn directly with pygame's own drawing functions instead of external masks - `pg.draw.rect` with a `border_radius` for rounded and pill buttons, `pg.draw.circle` for circular ones:

```python
class btnShape(Enum):
    RECTANGLE = 0
    ROUND_RECT = 1
    PILL = 2
    CIRCLE = 3
```
Shading is the more interesting addition. I use numpy to build a brightness profile - a single vertical strip of values, brightest just above centre, fading towards both edges - and tile it sideways across the whole button:

```python
def _shading_round(self, surface: pg.Surface) -> pg.Surface:
    pixels = pg.surfarray.pixels3d(surface)

    #curved sine-wave profile over the button's height, peak slightly above centre
    y_indices = np.arange(self.size[1])
    normalised = y_indices / (self.size[1] - 1)
    curve = np.sin(normalised * np.pi + 0.3)
    profile = (110 + curve * 145).astype(np.uint8)

    return self._apply_shading(pixels, profile, surface)
```
An edge-highlight variant works the same way with a flatter profile, giving a harder, more plastic-looking highlight rather than a soft gradient.

`NewProcButton` doesn't yet have the icon placement or wrapping that the current `ProceduralButton` has, and a planned bevel shading option is still an empty `pass`. Once those are carried across, this becomes the one procedural button class rather than two.

## Image Based Buttons
Sometimes a button just needs to be a piece of artwork rather than something drawn from primitives - a hand-designed icon button, for instance, where procedural shading would never match. `ImageButton` covers that case by loading an image file straight in as the background:

```python
class ImageButton(Button):
    def __init__(self, font: pg.Font, image_path: str, text_primary_colour: int,
            text_hover_colour: int, position: tuple[int, int], size: tuple[int, int], 
            text:str, mask_colour: int) -> None:
        super(ImageButton, self).__init__(position)
```
Rather than drawing straight onto a blank surface, it first fills a reference surface with a colour key, then blits the loaded image centred on top:

```python
def _make_background_surf(self) -> pg.Surface:
    #make the injected image a surface and centre it on the reference surface
    image:pg.Surface = pg.image.load(self.path).convert_alpha()
    im_rect: pg.Rect = image.get_rect()

    #centre onto reference
    im_rect.center = self.ref_rect.center

    #blit onto reference surface
    self.reference.blit(image, im_rect)

    return self.reference
```
The colour key (`mask_colour`) means any part of the reference surface not covered by the image stays transparent, which matters when the source artwork doesn't fill the whole button area.

One deliberate simplification here: `ImageButton` doesn't have a separate hover background at all - `hover_bg` is just set to the same surface as `background`. The artwork already carries enough visual weight on its own, so only the label colour changes between states:

```python
def _make_button_surfaces(self) -> None:
    #make the background surfaces - normal = hover
    bg: pg.Surface = self._make_background_surf()
    hbg: pg.Surface = bg
```
Everything else - the label rendering, the transparency handling, `change_colour`, `draw` - follows the same pattern as the procedural buttons, since it's all inherited from the same `Button` contract.

## Conclusion
Three different ways to build a button's face - drawn procedurally, drawn and shaded procedurally, or lifted from artwork - but all three sit on the same `Button` skeleton underneath, which only cares about position, hover state, click state and drawing. That split has turned out to be the useful part: I can change how a button looks completely without touching how it behaves.

There's still tidying up to do. The icon and label logic needs to make its way from `ProceduralButton` into `NewProcButton` so it can properly take over, and keyboard navigation is still just an idea rather than code. But for mouse-driven menus, this is enough to start building screens on top of.

## Attributions
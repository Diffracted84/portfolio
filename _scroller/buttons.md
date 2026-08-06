---
title: Buttons
layout: single
index: 1
excerpt: Building a class for user interaction.
permalink: /projects/scroller/button/
date: 2026-07-23
last_modified_at: 2026-07-23T16:20+10:00
show_date: true

read_time: true
words_per_minute: 200

categories: [Programming, Python, Game Dev]
tags: [pygame, scroller, interface]

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
  image: /assets/images/projects/sidescroller/buttons/header.jpg
  caption: Image by [nafeti_art](https://pixabay.com/users/nafeti_art-5143689/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=8120993) from [Pixabay](https://pixabay.com)
  teaser: /assets/images/projects/sidescroller/buttons/teaser.png

gallery_proc_state:
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_rect_noshade_noico_norm.png
    alt: "Procedural button in normal state."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_rect_noshade_noico_hover.png
    alt: "Procedural button in hover state."

gallery_proc_shapes:
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_rect_noshade_noico_norm.png
    alt: "Procedural button with rectangular shape."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_round_noshade_noico_norm.png
    alt: "Procedural button with rounded corners."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_pill_noshade_noico_norm.png
    alt: "Procedural button with pill shape."

gallery_proc_shading:
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_round_round_lhico_norm.png
    alt: "Procedural button with rounded shading."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_round_edge_lhico_norm.png
    alt: "Procedural button with edge highlight shading."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_round_bevel_lhico_norm.png
    alt: "Procedural button with bevel shading."

gallery_proc_icon:
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_round_noshade_lhico_norm.png
    alt: "Round corner button with icon on the left."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_round_noshade_rhico_norm.png
    alt: "Round corner button with icon on the right."

gallery_proc_circ:
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_circ_noshade_noico_norm.png
    alt: "Procedural button in circle shape. No icon."
  - image_path: /assets/images/projects/sidescroller/buttons/procedural/procbtn_circ_noshade_centico_norm.png
    alt: "Procedural button in circle shape. With icon."

gallery_img:
  - image_path: /assets/images/projects/sidescroller/buttons/image/imgbtn_norm.png
    alt: "Image based button in normal state."
  - image_path: /assets/images/projects/sidescroller/buttons/image/imgbtn_hover.png
    alt: "Image based button in hover state."

toc: true
toc_label: Jump to Section
toc_icon: book
toc_sticky: true
---
## Overview
The latest task I've given myself isn't directly building the game itself - it's the buttons that let a player navigate a menu with a mouse, and eventually a keyboard. A rectangle with some text on it isn't really a button, it's a label that happens to sit still. A button needs to look like it belongs in the game rather than a generic UI widget dropped on top of it, and it needs to behave predictably - hovering and clicking the same way every time regardless of how it's styled.

## The Parent Class
Every button, however it ends up looking, needs to know the same three things: is the mouse over it, has it been clicked, and where does it draw itself. That's the whole job of the `Button` parent class - it doesn't know or care how its surfaces were made, only how to react once they exist.

```python
class Button():
    def __init__(self, centre: tuple[int, int]) -> None:
        self.centre: tuple[int, int] = centre
        self.hover: bool = False
        self.clicked: bool = False
        self.action: bool = False
```

The visuals arrive afterwards as a small dataclass - background, hover background, label, hover label - and the parent just wires them up to a position:

```python
@dataclass
class ButtonSurfaces:
    background: pg.Surface
    hover_bg: pg.Surface
    foreground: pg.Surface
    hover_fg: pg.Surface
```

Whatever child class builds the button, its only job is to produce one of these and hand it over. Alongside `centre`, the parent also exposes `hoverstate` and `clickstate` as two read-only properties for menu code to check.

{% include gallery id="gallery_proc_state" caption="***Yellow:*** Normal State. ***Olive:*** Hover State" %}

## Hit Detection
A `pg.Rect` is only ever a bounding box - useful, but a circular button still has a rectangular hitbox if that's all you use. It's the doorknob problem: the door frame around it is rectangular, but you don't expect to open the door by pressing the corner of the frame. So mouse collision uses pygame's mask module to build a pixel-accurate shape from the button's actual image, checked in two passes - broad, then fine:

```python
if (rel_x in range(0, self.rect.width) and 
    rel_y in range(0, self.rect.height)):
    if self.surface_mask.get_at((rel_x, rel_y)):
        self.hover = True
        self.check_clicked()
```

The cheap rectangle check runs first, and the more expensive per-pixel mask check only runs if the mouse is already in the ballpark.

## Click Debounce
One action per press is the goal, the same way a doorbell only rings once no matter how long you lean on it - you have to let go and press again. `self.clicked` tracks whether the button is currently held, and `self.action` only goes true on the single frame the mouse transitions from up to down:

```python
def check_clicked(self) -> None:
    self.action = False
    if pg.mouse.get_pressed()[0] == True and self.clicked == False:
        self.clicked = True
        self.action = True
    if pg.mouse.get_pressed()[0] == False:
        self.clicked = False
```

## Configuration by Dataclass
A button needs a lot of settings - colour, hover colour, icon path, padding, shape, shading, label - and passing all of that as separate constructor arguments gets unwieldy fast. So each group of related settings is its own small dataclass - `ButtonLayer` for a colour pair, `IconLayer` for icon placement, `Geometry` for shape and size - bundled into one `ProcButtonArgs` for the whole button:

```python
@dataclass
class ProcButtonArgs:
    bg_settings: ButtonLayer
    fg_settings: ButtonLayer
    icon_settings: IconLayer
    geometry_settings: Geometry
    label: str
```

It's the difference between handing someone a shopping list item by item versus handing them a packed bag - same contents, but only one of those is easy to carry. `ImageButton` takes the same approach with `ImageBtnArgs`. Both button classes just unpack a settings object at the top of `__init__` rather than reading through a long parameter list.

## Procedural Buttons
### Shapes
`ProceduralButton` draws its own background and label from scratch rather than relying on artwork, using pygame's own drawing functions - `pg.draw.rect` with a `border_radius` for rounded and pill shapes, `pg.draw.circle` for circular ones:

```python
class btnShape(Enum):
    RECTANGLE = 0
    ROUND_RECT = 1
    PILL = 2
    CIRCLE = 3
```

{% include gallery id="gallery_proc_shapes" caption="Examples of procedural button geometery." %}

### Shading
Shading builds on top of the shape. It works by constructing a brightness profile - a single vertical strip of values - and tiling it across the button. The round variant peaks just above centre for a soft highlight, the edge-highlight variant uses a flatter, harder-edged profile, and the bevel variant brightens the top few pixels and darkens the bottom few to fake a raised edge:

```python
def _shading_round(self, surface: pg.Surface) -> pg.Surface:
    pixels = pg.surfarray.pixels3d(surface)
    y_indices = np.arange(self.size[1])
    normalised = y_indices / (self.size[1] - 1)

    curve = np.sin(normalised * np.pi + 0.3)
    profile = (205 + curve * 50).astype(np.uint8)

    return self._apply_shading(pixels, profile, surface)
```

{% include gallery id="gallery_proc_shading" caption="Examples of faux shading." %}

None of these are physically accurate lighting - they're a cheap approximation that reads as "raised" or "glossy" at a glance, which is all a menu button needs.

Icons can sit to the left, right, or centre of the label, and the label wraps to a second line if it doesn't fit in the remaining space. The wrap logic is deliberately simple - it splits the label on the space closest to the middle of the string, or cuts it in half if there's no space at all. It's not something I'd reach for anywhere text length is unpredictable, but it's enough for short button labels.

### Icon Placement
An icon isn't bolted onto a finished button - it claims space from the label before anything gets drawn. `IconLayer` fixes a position (`LEFT`, `RIGHT`, `CENTRE`, or `NONE`) and a padding value, and the corner-placement logic only runs when that position isn't `NONE` (since a button with no icon has nothing to place).

{% include gallery id="gallery_proc_icon" caption="Examples of icon placement." %}

Left and right placement pushes the icon to its edge of the reference rect with padding as the gap, and the label's available width shrinks to match - which is what feeds the wrap logic above. A wide icon on a narrow button means the label wraps sooner, not that it gets clipped.

### Circles Are a Special Case
A circular button was never going to fit a full word, so it doesn't try. If it has an icon, the icon sits dead centre and the label is dropped entirely. If it doesn't, the label is cut down to its first letter, capitalised. Rather than wrapping text into an unreadable stack to force it into a shape that isn't built for text, the button just gives up on the label gracefully.

{% include gallery id="gallery_proc_circ" caption="Circle buttons with and without icons." %}

## Image Based Buttons
Sometimes a button needs to be a piece of artwork rather than something drawn from primitives - a hand-designed icon, for instance, where procedural shading would never match. `ImageButton` covers that case by loading an image file straight in as the background, centred onto a reference surface filled with a colour key so any part not covered by the artwork stays transparent.

Hover state gets its own artwork too, rather than reusing the normal-state image with just the label colour changing:

```python
bg: pg.Surface = self._make_background_surf(self.bg['primary'])
hbg: pg.Surface = self._make_background_surf(self.bg['hover'])
```

{% include gallery id="gallery_img" caption="***Blue:*** Normal state Image based button. ***Green:*** The same button in hover state." %}

`ImageBtnArgs` takes a separate `hover_path` for this. A caller that doesn't want a distinct hover look can still pass the same path twice, but that's a choice being made rather than a limitation baked into the class.

## Conclusion
Three ways to build a button's face - drawn procedurally, drawn and shaded procedurally, or lifted from artwork - all sitting on the same `Button` skeleton underneath, which only cares about position, hover state, click state, and drawing. That split is the useful part: how a button looks can change completely without touching how it behaves. Keyboard navigation is still just an idea rather than code, but for mouse-driven menus, this is enough to start building screens on top of.

## Attributions
I gained inspiration from these two sources:
1. [EASY Way to Make BUTTONS for Python/PyGame Projects](https://www.youtube.com/watch?v=al_V4OGSvFU&t=290s) by [baraltech](https://www.youtube.com/@baraltech)
2. [PyGame Beginner Tutorial in Python - Adding Buttons](https://www.youtube.com/watch?v=G8MYGDf_9ho&t=902s) by [Coding With Russ](https://www.youtube.com/@CodingWithRuss)
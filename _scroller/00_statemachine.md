---
title: The State Machine
excerpt: Building the backbone of the game.
permalink: /projects/sidescroller/statemachine
date: 2026-05-25
categories: [Programming, Python, Game Dev]
tags: [pygame, scroller]
share: false
related: false
#project:  "SideScroller"

author: Casual

header:
  image: /assets/images/projects/sidescroller/statemachine/header.png
  caption: Image by [Lucas Israel](https://pixabay.com/users/lucasjisrael-43158173/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=8860311) from [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=8860311)
  teaser: /assets/images/projects/sidescroller/statemachine/StateDiag.png

words_per_minute: 200
---
## Overview
I have been playing with making a game using the pygame framework for a few months now. It has been a learning curve, but I am starting to get the hang of it.
A few days ago I realised that game is ultimately a state machine. There are several states within this machine that need to be considered. So I started exploring how I would refactor my infant project to make states a reality.

## Planning
To start with I needed to do some research on how to implement. I have had previous experience using a state machine in LabVIEW, but not in python. I found what I thought was a really good pair of tutorials on pygame/python implementation. You can find these in the [Attributions](#attribution) section and the end of this post. If this is something you are looking to do, these are worth a watch.
Secondly, I needed to plan what my states are going to be. A bit of brainstorming and I came up with the plan below:
<figure>
  <img src="{{ '/assets/images/projects/sidescroller/statemachine/StateDiag.png' | relative_url }}" alt="State Diagram">
</figure>
I think that this is a fairly robust model for a simple game.

## Application
### Building the bones of the machine.
In practical Python terms, each state needs to be a class. There is a bit of abstraction as there is a parent class, let's call it BasicState, and then each state in the diagram is a child class.
Here is a look at the BaseState class. We'll start with initialisation of the parent class.
```python
class BasicState:
    def __init__(self, game) -> None:
        self.game = game
        self.prev_state = None
```
Pretty simple really. We have inject the game as a dependancy so that each state has access to the game and it's attributes. ```self.prev_state``` is a placeholder for the state stack. We'll take about that more a bit later on.

Next are a pair of methods to control state transitions. One is to enter a new state via the stack, the other is to leave a state and remove it from the stack.
```python
def enter_state(self) -> None:
    #get previous state and enter a state into the stack
    if len(self.game.state_stack) > 1:
        #there is more than 1 states in the stack
        self.prev_state = self.game.state_stack[-1]
    #append the stack with the new state
    self.game.state_stack.append(self)
```
and
```python
def exit_state(self) -> None:
    #remove the top state from the stack
    self.game.state_stack.pop()
```
There are three abstract methods to pass on to the children that are for the pygame framework. The are simply passed through in this instance. I will talk about these methods in greater detail in another post.
```python
def get_updates(self) -> None:
    pass

def update(self, dt: float, actions: dict) -> None:
    pass

def draw(self, surface: pg.Surface) -> None:
    pass
```
### The Stack
For those that don't know, a quick intro to a queue:

**NOTE**
A queue is a collection of ordered entities. Typcally an entity becomes an element of the queue by joining at the back of the queue. As elements are processed they drop out. A bit like a line of people waiting to enter a club.
{: .notice--info}
Now that is out of the way, a typical queue acts as a buffer for data that needs processing. It waits in line until its turn and then it leaves the queue.
<figure>
  <img src="{{ '/assets/images/projects/sidescroller/statemachine/DataQueue.png' | relative_url }}" alt="Data Queue">
</figure>
A stack is similar, but works on a *Last In - First Out* basis. A stack is more like a deck of cards. You pick the one on the top, but you also place a card in the topof the stack.
<figure>
  <img src="{{ '/assets/images/projects/sidescroller/statemachine/DataStack.png' | relative_url }}" alt="Data Queue">
</figure>
This is useful for states as you usually wish to go to the state defined last, but to have a queue of states to return to. This is particularly handy for things like in-game menus.

### An Example from my Game
We play the game from a GameLogic class. The user can enter a pause menu by pressing ```P``` key. The state transition in the GameLogic class would look a little something like below:
```python
def update(self, dt: float, actions: dict) -> None:
    #Has a pause action been registered?
    if actions['paused']:
        #enter the paused state
        new_state = Pause(self.game)
        new_state.enter_state()
```
**INFO:**
We haven't exited the GameLogic state, as we may wish to resume the game.
{: .notice--info}
When in the pause state, the game can be unpaused and the game resumed. The Pause state would look like below:
```python
def update(self, dt: float, actions: dict) -> None:
    #has the user made a menu selection?
    if actions['unpaused']:
        #continue the game
        self.exit_state()
    elif actions['reset']:
        ...
```

## Conclusion
The state machine is a powerful design pattern in game development. It can be used to compartmentalise the logic of the game in to its separate areas (states) for cleaner code and more predictable behaviour.
It does require forward planning, particularly when and how to move between states, but it robust and flexible. Modifiations can be made between versions of the application in that states can be added or removed, and the relationships between them can also change as required.
I am using a state machine in my games moving forward because of these advantages.

## Attribution
1. [Pygame Game States Tutorial](https://www.youtube.com/watch?v=b_DkQrJxpck) by [CDcodes](https://www.youtube.com/@CDcodes)
2. [PyGame State Machine - Python Game Dev Tutorial](https://www.youtube.com/watch?v=PZTqfag3T7M&t=512s) by [It's That Ian Guy](https://www.youtube.com/@itsthatianguy)
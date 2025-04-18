# Lab 4 - Moore Elevator Controller

```{contents}
:local:
:depth: 2
```

## Overview

This lab is all about **designing complex systems!**

To do this you will need to pull together building blocks from several previous labs and ICEs!

Per usual, you must submit your own copy of VHDL but the lab report is done with a partner.

The **prelab** is on Gradescope; it will require you to make a copy and clone
https://github.com/usafa-ece/ece281-lab4

```{important}
*Only* need to edit `top_basys3.vhd`.
All other VHDL files and your constraints file are ready-to-go; do not edit them.

This is to emphasize modularity (and competing internal requirements 😉)
```

## Single-Elevator Controller

The first iteration of this lab is simply implementing
[ICE5](https://usafa-ece.github.io/ece281-book/ICE/ICE5.html)
Basic Elevator Controller, as shown in {numref}`basic-elevator-controller`

```{figure} img/lab4_basic_controller.png
---
name: basic-elevator-controller
---
Single-Elevator Controller user interface
```

- The elevator’s current floor is shown on seven-segment display 0 (rightmost... doesn't match picture above)
- Disable unused displays
- The elevator takes 0.5 seconds to move between floors
- The display updates according to the output
- The elevator must not teleport... no skipping floors
- Three push buttons are to be used for resets
- The Master Reset button resets both the FSM **and** the clock.
- The reset floor is **floor 2**.
- LED 15 must be tied to the clock signal that drives the FSM.
- You *may* use the other LEDs for debugging. For example, outputting the current floor in binary. Otherwise they should be grounded.

```{tip}
You should checkout how the template implements the elevator FSM.
It uses a fancy VHDL thing that is handy when scaling to a large number of sequential states!
```

> Demo this to a classmate *prior* to starting on Multi-Elevator Controller.
>
> Git commit and push.

## Multi-Elevator Controller

Expand the basic functionality above.
**Before** you start editing the VHDL you **must update** your prelab **diagram!!!**
We highly recommend that you check this diagram with an instructor (or at least a classmate).

```{danger}
If you don't update your diagram **first**, you are probably going to waste *Alot of time*.

![Alot of Time](https://i0.kym-cdn.com/photos/images/newsfeed/000/177/953/alot_of_times.jpg?1316886476)
```

1. Add a second elevator FSM
2. Connect this second elevator to use `sw(15)` for Up/Down and `sw(14)` for Stop.
3. Connect it to the same reset signal as the first FSM.
4. Display the floor on display 2 (second from left).
5. Set unused displays (3 and 1) to `F` (for "floor")

### Simulation

Simulation is not required for this Lab, and you can jump directly to hardware implementation.

```{note}
#### A brief discussion on testing philosophy.

The type of testing you choose results in a tradeoff of **complexity** vs.
how much **confidence** your test gives you that your design works.

Each component within your top_level already has been simulated and tested.
This is known as **unit testing** because you make sure that each
unit (component) does what it is supposed to.
Unit tests give you confidence that a single thing works and they are
quick to write and run. But unit tests don't give you any information on how components
will or won't work together.

**Integration testing** tests if multiple units work together;
for example, does the clock_divider indeed provide a slow clock to the FSM?
Often this is the sweet spot of complexity vs. confidence, but not always.

Finally, **end-to-end** testing tests if everything works together.
Unfortunately, end-to-end tests are also very complex to design and maintain.

As engineers, part of our job is to determine what is the appropriate level of confidence
and justify designing tests to meet that level.
```

## Deliverables

- **Demo** multi-elevator functionality by uploading a video to
    Teams > ECE281 > General > Files > Demos > Lab 4 > section. Name your video `lastname1_lastname2_demo.mp4`
- Git commit and push
- **Submit code** to Gradescope
- Write lab report (template in teams)

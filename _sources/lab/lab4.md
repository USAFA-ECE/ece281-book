# Lab 4 - Moore Elevator Controller

```{contents}
:local:
:depth: 2
```

## Overview

This lab is all about **designing complex systems!**

To do this you will need to pull together building blocks from several previous labs and ICEs!

Per usual, you must submit your own copy of VHDL but the lab report is done with a partner.

The **prelab** is on Gradescope.

```{important}
There is no GitHub template for this lab. Instead, you must make a new GitHub repository.

We recommend you create a new repository on github.com (including "Add a README file") and clone that repo.

In Git Bash you can make a new directory with

~~~bash
mkdir src/
~~~

Then copy in necessary source files.
Also copy in a `.gitignore` from a previous lab or your repo will get super cluttered.

Finally, [create a new Vivado project](https://usafa-ece.github.io/ece281-book/appendix/vivado.html#create-a-new-vivado-project)
in that folder, add your source files, and select the Basys3 board!

It's important to get this part correct or lots of things won't work!
```

> Git commit and push.

## Basic Elevator Controller

The first iteration of this lab is simply implementing
[ICE5](https://usafa-ece.github.io/ece281-book/ICE/ICE5.html)
Basic Elevator Controller, as shown in {numref}`basic-elevator-controller`

```{figure} img/lab4_basic_controller.png
---
name: basic-elevator-controller
---
Basic Elevator Controller user interface
```

- The elevator’s current floor is shown on seven-segment display 2 (2nd from right)
- The elevator takes 0.5 seconds to move between floors
- The display updates according to the output
- The elevator must not teleport... no skipping floors
- Three push buttons are to be used for resets
- The Master Reset button resets both the FSM **and** the clock.
- The reset floor is **floor 2**.
- LED 15 must be tied to the clock signal that drives the FSM.
- You *may* use the other LEDs for debugging. For example, outputting the current floor in binary. Otherwise they should be grounded.

> Demo this to a classmate *prior* to starting on Multi-Elevator Controller.
>
> Git commit and push.

## Multi-Elevator Controller

Expand the basic functionality above.

1. Change `elevator_fsm` to move between floors 1 to 9
2. Change  this elevator to use `sw(14)` = `i_stop` and `sw(15)` = `i_up_down`
3. Add a second elevator that is shown on display 0 (rightmost)
    - The second elevator FSM is reset with `btnD` or the Master Reset
    - The second elevator `i_stop` is `sw(0)`
    - The second elevator `i_up_down` is `sw(1)`

```{tip}
You can use the [predefined attributes](https://portal.cs.umbc.edu/help/VHDL/attribute.html)
of enumerated types to make this easier.

- `T'VAL(X)`    is the value of discrete type T at integer position X.
- `T'SUCC(X)`   is the value of discrete type T that is the successor of X.

So, for example, your basic elevator controller could have been implemented with:

~~~vhdl
-- Use enumerated type predefined attributes
f_Q_next <= s_floor2 when i_reset = '1' else
            f_Q      when i_stop  = '1' else
            sm_floor'succ(f_Q) when ( i_up_down = '1' and f_Q /= s_floor4 ) else -- go up if haven't hit top
            sm_floor'pred(f_Q) when ( i_up_down = '0' and f_Q /= s_floor1 ) else -- go down if haven't hit bottom
            f_Q; -- Bottom out or top out means stay on that floor
~~~
```

### Simulation

This section will be required for your lab report.

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

Each component in this lab already has a test bench (unit test) written for it - although **elevator_fsm** has evolved.
You are welcome to copy these test benches into your project or to leave them out.

What you need to make is a simulation that gives you reasonable confidence in your system as a whole.

Create a file named `src/top_basys3_tb.vhd` and add it as a simulation source.
Instantiate a **top_basys3** component and test *some* of the system's behavior.
However, **do not** feel the need to write `assert` statements, unless you want to.
A manual analysis is acceptable. If you do write assert statements, make sure you use the master reset to line up your timing.

## Deliverables

*After* you have a wave form you are happy with, check your constraints file and generate a bit stream.

- **Demo** full functionality to instructor
- Git commit and push
- **Submit code** to Gradescope
- Write lab report (template in teams)

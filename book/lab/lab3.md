# Lab 2 - Seven Segment Display Decoder

Due: Lesson 17

```{contents}
:local:
:depth: 2
```

## Overview

In this lab, you will design, write, test, and implement in hardware
a finite state machine to simulate the taillights of a 1965 Ford Thunderbird.

### Authorized Resources

For this lab, you may work in **teams of two** (including the prelab).
You may seek help from any cadet or instructor,
not limited to this course, and reference any publication in its
completion. Online resources are acceptable. However, no resources
containing solutions to the course homework, labs, exams, quizzes, or in
class/out of class exercises are allowed.

Your work (and your code) must **always** be your own.
Normal documentation of all resources utilized is required.

## Background

The 1965 Ford Thunderbird has three lights on each side that operate in
sequence to indicate the direction of a turn. {numref}`tailights` shows the tail
lights and {numre}`tailight-flash` shows the flashing sequence for (a) left turns and
(b) right turns.

```{figure} img/lab3_tailights.png
---
name: tailights
---
Thunderbird taillights
```

```{figure} img/lab3_tailightsflash.png
---
name: tailights-flash
---
Flashing sequence for taillights. The lights shaded light
blue are illuminated.
```

The objective of this lab is to create a turn signal that will engage
the left and right light sets to mimic the Ford Thunderbird. This lab is
divided into four parts: design (Prelab), VHDL creation, simulation, and
implementation. If you follow the steps of FSM design carefully and
**ask questions** at the beginning if a part is confusing, you will save
yourself a great deal of time.

## Prelab - FSM Design

Due on Gradescope midnight **T23**.

### Project functionality description

- On RESET, the FSM should immediately enter a state with all lights off.
- When you press LEFT, you should see $LA$, then $LA$ and $LB$, then $LA$, $LB$, and
$LC$, then finally all lights off again.
- This pattern should occur even if you release LEFT during the sequence.
- If LEFT is still down when you return to the lights off state, the pattern should repeat.
- The logic for the right lights is similar.
- When both LEFT and RIGHT switches are on, your state machine should blink all lights on and off (implementing hazard lights).

### Prelab Tasks

You will use a **Moore FSM** to implement the above functionality.

Complete the following tasks to prepare yourself for the lab.

1. Create a state transition diagramusing the
`Lab3_StateTransitionDiagram_Template.pptx` found in Teams.
2. Complete the **binary** encoding, state transition, and output tables
using the `Lab3_Tables_Template.xlsx` found in Teams.
3. Complete the **one-hot** encoding, state transition, and output tables
using the second tab in the same template.
4. Select *one* of the two encodings and write the **next state equations** for your choice.
5. Write the **output equations** for the same choice.
6. Submit everything to Gradescope.

> Submit everything to Gradescope.

In the remainder of the lab, you will be creating your FSM in VHDL,
testing it using a testbench, and then implementing your design on the
FPGA. Since you are designing a synchronous sequential circuit,
**we will be making use of a clock signal** to drive the logic. Finally,
while not required for the Prelab, you should already be thinking about
what **test cases** you will use to verify that your design is correct.

```{note}
This lab is modeled after a lab provided with instructor notes for
*Digital Design and Computer Architecture*, David Money Harris &
Sarah L. Harris, 2nd Edition.
```

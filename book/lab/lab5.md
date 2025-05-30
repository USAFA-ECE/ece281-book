# Lab 5 - Basic CPU

```{contents}
:local:
:depth: 2
```

## Overview

You will develop a basic CPU on the Basys3 board!

Your CPU will use a three-bit OPCODE and two eight-bit operands entered with switches.
It will cycle by pressing the center button, and show results on the seven-segment display.

### Demo

Here is a quick demo showing how your basic CPU should function.

<iframe width="560" height="315" src="https://www.youtube.com/embed/iANrLxhIU1Q?si=e83gbCnH4-JZdm3P" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Template Repository

https://github.com/USAFA-ECE/ece281-lab5

**Only** edit the following files:

- `top_basys3.vhd`
- `ALU.vhd`
- `controller_fsm.vhd`

```{tip}
The template contains an **ALU_tb** that you can simulate your ALU against!
```

### Authorized Resources

Work in **teams of two** (including the prelab),
but you **must** work with a *different partner* for this lab than previous labs.

Your work (and your team's code) must **always** be your own.
Normal documentation of all resources utilized is required.

### Background

The Arithmetic Logic Unit (ALU) is a core component of microprocessors.
For this lab, the goal is to implement both:

- a **control path** (how the components supporting ALU operations are configured)
- a **data path** (providing operands to the ALU).

Most (if not all) CPUs perform operations in multiple cycles.
Most CPUs do “instruction fetch”, “data fetch”, and “execution” in separate cycles.
The CPU in this lab is similar in that it has four cycles to complete any operation.
This CPU only has two registers and the result of any operation is displayed
(not stored in memory); however, the CPU is nearly fully functioning.

## Implementation Details

- Implement the ALU shown in *Digital Design and Computer Architecture, RISC-V Ed.*, **Figure 5.17**.

| ALU Control | Description |
|---|---|
| 000 | Add |
| 001 | Subtract |
| 010 | And |
| 011 | Or |

- **btnU** should serve as the master reset for all components with a synchronous reset;
- **btnL** should be the asynchronous reset for the clock divider.
- `o_cycle` (the FSM output) is one-hot!
- `led(3:0)` - the state of the FSM (as one-hot)
- `led(15:12)` - $NZCV$ ALU flags

```{figure} img/lab5-top_basys3.excalidraw.svg
---
name: lab5-top-level
---
Partially complete Lab 5 top level.
*Made with [Excalidraw](https://excalidraw.com/); you can open this `.svg` file in Excalidraw to make edits, if you'd like.*

Blue boxes are implemented directly in **top_basys3**; Black boxes are components; Red items are TODO; Signals should be labeled.
```

### Usage

The CPU takes 8-bit two's complement operands on `sw(7:0)` and a 3-bit operand on `sw(2:0)`.
All values are displayed in decimal on the seven-segment display - including a minus sign for negative values.

**btnC** steps through the following **four** states:

1. Starts with a clear display (also the **reset** state)
2. Store the 1st operand in a register and display it.
3. Store the 2nd operand in a register and display it.
4. Display the result of the operation on the two operands.
5. Return to the initial state.

## Deliverables

Each deliverable is **one submission per team.**

1. **Prelab** on Gradescope.
2. **Demo** to a classmate and certify the demo in spreadsheet: **ECE281 Team Files > Demos > Lab 5 Demo Tracker**.
3. **Lab code** pushed to GitHub and submitted to Gradescope.

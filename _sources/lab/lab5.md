# Lab 5 - Basic CPU

```{contents}
:local:
:depth: 2
```

## Overview

You will develop a basic CPU on the Basys3 board!

Your CPU will use a three-bit OPCODE and two eight-bit operands entered with switches.
It will cycle by pressing the center button, and show results on the seven-segment display.

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
- **btnU** should serve as the master reset for all components with a synchronous reset;
- **btnL** should be the asynchronous reset for the clock divider.
- `led(3:0)` - the state of the FSM (as one hot)
- `led(15:12)` - $NZCV$ ALU flags

```{figure} img/lab5-top_basys3.excalidraw.svg
---
name: lab5-top-level
---
Partially complete Lab 5 top level. Made with [Excalidraw](https://excalidraw.com/#json=IVv1cUOKE_gaUh9OLeXCj,M21pQ4sn4Fk6NO4FFR_fWQ)

Blue boxes are implemented directly in **top_basys3**; Black boxes are components; Red items are TODO; Signals should be labeled.
```

### Usage

The CPU takes 8-bit two's complement operands on `sw(7:0)` and a 3-bit operand on `sw(2:0)`.
All values are displayed in decimal on the seven-segment display - including a minus sign for negative values.

**btnC** steps through the following states:

1. Store the 1st operand in a register and display it.
2. Store the 2nd operand in a register and display it.
3. Display the result of the operation on the two operands.
4. Clear the seven-segment display

## Deliverables

Each deliverable is **one submission per team.**

1. **Prelab** on Gradescope.
2. **Demo** to a classmate and certify the demo in spreadsheet: **ECE281 Team Files > Demos > Lab 5 Demo Tracker**.
3. **Lab code** pushed to GitHub and submitted to Gradescope.

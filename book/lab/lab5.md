# Lab 5 - Basic CPU

```{contents}
:local:
:depth: 2
```

## Overview

You will develop a basic CPU capable of Addition,
Subtraction, AND, OR, and Shift operations in 3 phases!

To do this, write, test, and implement a CPU on the Basys3 Development Board.
A three-bit OPCODE and two eight-bit operands will be input using the 16
switches on the BASYS3 board. Upon pressing the center button, a
seven-segment display will load the operands into the memory and then,
upon a second push, will display the operands and output of the
operation on the 4-digit seven segment display.

### Authorized Resources

For this lab, you are encouraged to work in **teams of two** (including the prelab).
If you worked in a team for previous labs,
you **must** work with a *different partner* for this lab.

You may seek help from any cadet or instructor,
not limited to this course, and reference any publication in its
completion. Online resources are acceptable. However, no resources
containing solutions to the course homework, labs, exams, quizzes, or in
class/out of class exercises are allowed.

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

You will implement an ALU with supporting components capable of Addition,
Subtraction, bit-wise OR, bit-wise AND, and shift operations on two 8-bit operands
and display the result (including negative values in parts B and C) in decimal on the display.

The instructions for this simple CPU will be executed in four cycles:

1. Load Operand A into memory
2. Load Operand B into memory
3. Execute provided instruction on the Operands in memory
4. Clear the display

### Lab Flow.

```{table} Lab 5 ALU Tasks
:name: alu-tasks

|   **Lab Task**  |     ALU Operation              |     Input               |     Output              |     Display    |     Cycles                             |
|-----------------|--------------------------------|-------------------------|-------------------------|----------------|----------------------------------------|
|    $A$          |     Addition                   |     8-bit   unsigned    |     8-bit   unsigned    |     Hex        |     fetch A, fetch B, execute, clear   |
|    $B$          |     Addition and Subtraction   |     8-bit   2-comp      |     8-bit   2-comp      |     Decimal    |     fetch A, fetch B, execute, clear   |
|    $C$          |     Task $B$, AND, OR, Shift   |     8-bit   2-comp      |     8-bit   2-comp      |     Decimal    |     fetch A, fetch B, execute, clear   |
```

This lab has three functionality tasks: $A$, $B$, and $C$, summarized in {numref}`alu-tasks`

For all tasks, **btnU** should serve as the master reset (asynchronous preferred) for the
FSM, clock divider, and anything else that should have a reset.

#### Task A

Implement an ALU that provides Addition only on two unsigned 8-bit operands and displays
the results as an unsigned 8-bit number.

Operands are entered in unsigned notation (i.e., you will be able to express values from $0_d$ to $255_d$).
Use the **center button** on the BASYS3 board to step through the following states:

1. Store the 1st operand in a register and display it on the seven-segment display (digits 1-0) in HEX
2. Store the 2nd operand in a register and display it on the seven-segment display (digits 1-0) in HEX
3. Display the result of addition of the two operands on the seven-segment display (digits 1-0) in HEX
4. Clear the seven-segment display

The following inputs are used to specify the operands and operation:

- Operands are entered using Switches 7-0
- Opcodes/operations are entered using switches 3-0

In addition to the display of the result of the operation, the following
LEDs are part of the output:

- `0:3` - the state of the FSM (as one hot)
- `13` - CPU Cout
- `14` - CPU zero

#### Task B

Augment the ALU to implement Addition and Subtraction on two 8-bit
(including negative values: -$128_d$ to $127_d$) operands and display the result
(including negative values) as an 8-bit number (in decimal) on the display.
You will need to create a means to display a "-" sign.

Use the **center button** on the BASYS3 board to step through the following states:

1. Store the 1st operand in a register and display it on the seven-segment display
    (including "-" sign for negative values) in decimal using digits 2-0
2. Store the 2nd operand in a register and display it on the seven-segment display
    (including "-" sign for negative values) in decimal using digits 2-0
3. Display the result of the operation on the two operands on the seven-segment display
    (including "-" sign for negative values) in decimal using digits 2-0
4. Clear the seven-segment display

In addition to the display of the result of the operation, the following
LEDs are part of the output:

- `0:3` - the state of the FSM (as one hot)
- `13` - CPU Cout
- `14` - CPU zero
- `15` - CPU sign

#### Task C

Augment the ALU to implement Addition, Subtraction, bit-wise OR, bit-wise AND,
left logical shift, and right logical shift operations
on 8-bit two's complement operands (values from $-128_d$ to $127_d$).
Display the result as a decimal number on the display.

```{note}
It is a **logical shift**, so there is no sign extension.

The value of the first operand is shifted.
The three least significant bits of the second operand specify how much to shift by.
Left vs. Right shift are two different opcodes.
```

Use the **center button** on the BASYS3 board to step through the sames states as Task $B$.
The LEDs should also match Task $B$.

## Prelab

Submit one submission per team on Gradescope.

### Schematics

You are responsible for two schematics for this Prelab.
They can both be done by hand, or digitally.

- Complete the top_basys3 schematic in {numref}`lab5-top-level` (**Teams > Lab5**).
    - Use a color other than black
    - All you need to do is name signals and unlabeled ports!
    - For devices you plan to implement directly in the top_level
        instead of its own VHDL file, annotate with an asterisk.
- Create an ALU schematic that is capable of accomplishing Task $C$.
    - You cannot use a MUX larger than 4:1 in your ALU.

```{figure} img/lab5_top_level.png
---
name: lab5-top-level
---
Partially complete Lab 5 top level - signals need to be labeled.
```

A common way to implement addition and subtraction in an ALU is shown in {numref}`add-sub-gates`.

```{figure} img/lab5_add_sub_gates.png
---
name: add-sub-gates
---
A single select bit allows for addition and subtraction.
```

```{tip}
It may be the case that you have an easy opportunity for some extra instructions
that require zero extra gates. Don't be afraid to include those!
```

### Instruction Set

Before a CPU can operate on assembly code, it must be translated into machine code.

Create a table that maps what you would call an operation - for example, addition could be "ADD" -
to the opcode that it would generate - for example, ADD could be `000`.

These opcodes should directly control your ALU!

### Controller FSM

A **controller_fsm** is shown in the top_basys3 schematic.
This controller is responsible for cycling through the four CPU stages
and ultimately controlling outputs.

Draw the FSM diagram. You can draw it by hand as long as it is neat!

## Lab

- Fork and clone [Lab 5](https://github.com/USAFA-ECE/ece281-lab5).

You may take one of two approaches:

1. Demo each of Task $A$, $B$, $C$ to your instructor.
2. Demo only task $C$ to your instructor.

However:

If you choose **Option 1** you **must** push 3 git tags (like in Lab 3)
to mark your progress on each task (i.e. `git tag -a taskA -m "ADD in hex"`).

If you choose **Option 2** you **must** write a testbench for your ALU.
The waveform from your ALU testbench must be submitted with your lab report.

> Build the CPU.

## Deliverables

Below are the deliverables and point distributions for the Lab 4:

| Deliverable           | Points |
|-----------------------|--------|
| Prelab                | 20     |
| Demo(s)               | 30     |
| Written Report        | 50     |

The **2 page max** report will be very similar to Lab 4 Report requirements, with the following changes:

- Explain why your CPU works
- Discussion of your multiple-demo vs. ALU testbench approach
- Discuss any challenges you had
- Discuss your epic victories
- Discuss how Assembly Code is ultimately executed on hardware
- Note how many hours you spent on the lab and any feedback for the instructors on the lab.

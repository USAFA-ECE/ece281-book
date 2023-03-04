# ICE 4: Stoplight

```{contents}
:local:
:depth: 2
```

## Overview

So far in this course you have implemented combinational logic on your FPGA.
The ability to implement **synchronous** logic enables us to make
*finite state machines* and greatly expands your ability to implement
useful designs.

In order to accomplish this, we will expand our use of processes.

### Objectives

- Design the Stoplight FSM in VHDL
- Test the Stoplight FSM in VHDL
- Control a real world miniature stoplight.

### End State

Provide a demo to your instructor.

You are not required to turn in ICE3; however,
**ensure the following files are pushed to your repository**:

1. VHDL files
2. Constraints (`.xdc`)
3. `.bit` file used to program board

## Background

We will design the stoplight to match the description given in **Homework 3**.

- Light turns green when car is present.
- Light stays green while cars are present.
- Cycles through yellow to red when a car is not present.

The system has three outputs (Red, Yellow, and Green) and must
be implement with a Moore FSM as seen is {numref}`stoplight_fsm`.

```{figure} img/ice4_image1.jpg
---
name: stoplight_fsm
---
Stoplight state transition diagram
```

## Setup Vivado

1. Clone the ice3 assignment from GitHub Classroom
2. Open the folder
3. Double click `stoplight.xpr` to open Vivado!

> Modify headers and make an initial commit.

## stoplight_fsm.vhd

### Entity

The stoplight entity is given in `stoplight_fsm.vhd` as:

```vhdl
entity stoplight_fsm is
    port(
         i_C     : in  std_logic;
         i_reset : in  std_logic;
         i_clk   : in  std_logic;
         o_R     : out  std_logic;
         o_Y     : out  std_logic;
         o_G     : out  std_logic
        );
    end stoplight_fsm;
```

- `i_C` is the control signal to indicate if a car is present
- `o_R`, `o_Y`, and `o_G` are outputs to control the colored lights
- `i_clk` provides a rising edge to drive the flip flops
- `i_reset` will reset the flip flops to their default state

> **STOP!** Take out a physical or virtual piece of paper and draw your stoplight entity

### Architecture

**Every finite state machine is going to have the same structure, as shown in {numref}`fsm-block-d`**

1. "Combinational" or "Next State" Logic
2. State Register (flip flops)
3. Output logic

```{figure} img/ice4_image3.png
---
name: fsm-block-d
---
Finite state machine structure
```

You need to modify the architecture of your stoplight file to reflect
the design shown in {numref}`stoplight-fsm-arch`.

```{note}
This design looks more complicated than it actually is!
```

```{figure} img/ice4_image4.png
---
name: stoplight-fsm-arch
---
Entity and architecture for implementing stoplight
```

#### State Register

First, we'll implement Section 2: The State Register.

The state register for our stoplight is comprised of two flip flops.
Each flip flop has a $Q$ (current state) and a $Q_{next}$ (next state).
In order to represent three states, we need two bits,
so both $Q$ and $_{next}$ must be represented with 2-bit vectors.

According to our VHDL naming conventions, we will prefix these signals with `f_`
to indicate the signal is used for sequential logic.

```{warning}
It is critical that you give `f_Q` and `f_Q_next` a default value.
Because the current and next states are dependent on each other,
an undefined value in either one could break your system
and leave it in an undefined state.
```

Even though the original problem didn't specify, we are going to make
the **yellow state our default (and our reset) state.** To accomplish this,
the vectors need to be initialized with `"10"`.

```{hint}
Default values can be set with the syntax `:="10";` following
your initial signal declaration.
```

> Create and initialize two 2-bit vectors: `f_Q` and `f_Q_next`.

##### A "process" in VHDL

Once the signals have been created, we can use them in our state
register. The basis for a register in VHDL is a process. Declaring a
process allows us to create a sub section of our hardware that is
**only triggered when certain signals change** and is the only place we can
implement sequential statements.

If you remember, we have actually been using processes with every ICE and lab...
in our test bench!

Here is how to implement our register:

```vhdl
--- state memory w/ asynchronous reset ---
register_proc : process (i_clk, i_reset)
begin
    if i_reset = '1' then
        f_Q <= "10";        -- reset state is yellow
    elsif (rising_edge(i_clk)) then
        f_Q <= f_Q_next;    -- next state becomes current state
    end if;
end process register_proc;
---
```

Setup

- The text preceding the key word "process" represents the name you are giving that process.
- Unlike the test process, we have signals inside the parenthesis following the key word "process."
- These signals indicate which signals are going to trigger the process to run.
- We have `i_reset` because we want to implement an *asynchronous* reset.
- We have `i_clk` because we want the process to run on a rising edge of the clock.

If reset

- We check for the reset *first* because we do not wait for a clock edge... this is what makes it asynchornous.
- If we waited for the clock then it would be a synchronous reset.
- When we reset we want to go back to our yellow state of "10".

Else If rising edge

- The IEEE `std_logic_1164.vhd` library contains [functions we can use](https://github.com/ghdl/ghdl/blob/bde1e82a42b1c6468726434f35f80f06c4f83704/libraries/ieee2008/std_logic_1164.vhdl).
- The [`rising_edge()`](https://www2.cs.sfu.ca/~ggbaker/reference/std_logic/1164/rising_edge.html) function "Detects the rising edge of a std_ulogic or std_logic signal. It will return true when the signal changes from a low value ('0' or 'L') to a high value ('1' or 'H')."
- It is only when we have a rising edge that a register (or flip flops)reads in a new value.
- Once the rising edge is triggered we want to assign our current state `f_Q` to become `f_Q_next`.
- Because we made both `f_Q` and `f_Q_next` vectors, we don't need to worry about individually assigning the bits; we can just set `f_Q <= f_Q_next`.
- If there is no rising edge then we simply exit the process.

#### Next State Logic

Now that we have our register in place we can complete Section 1: Next State Logic.

We simply need to implement the `Q(0)` and `Q(1)` equations we developed in HW 3.

$$
Q_{next}(0) = \overline{Q(1)} * C
$$

$$
Q_{next}(1) = \overline{Q(1)} * Q(0) * \overline{C}
$$

> Implement these equations as combinational logic in your architecture.

#### Output Logic

Our final step in creating our stoplight component is Section 3: Output Logic.

We simply take the equations from HW3 and implement them.

$$
G = \overline{Q(1)} * Q(0)
$$

$$
Y = Q(1) * \overline{Q(0)}
$$

$$
R = \overline{Q(1)} * \overline{Q(0)} + Q(1) * Q(0)
$$

> Implement these equations as combinational logic in your architecture.

Check syntax, then:

> Commit your changes with git

## stoplight_tb.vhd

At this point your testbench VHDL file (stoplight_tb.vhd) should already
be added as a simulation source. If not, go back, add it, and set it as top.

**A fully completed test bench has been provided for this exercise.**
The provided test bench has two new aspects.

The first is the declaration of a constant.

```vhdl
-- Clock period definitions
    constant k_clk_period : time := 10 ns;
```

As you can see from the name, we intend to use this constant to create
an artificial clock to drive our FSM. When implementing our design on
the board, we will use the clk we get from the board, but
*inside the test bench we must make a virtual clock.*

Once the constant has been created, we can make a process to implement our clock:

```vhdl
-- Clock process
clk_proc : process
begin
    i_clk <= '0';
        wait for k_clk_period/2;
    i_clk <= '1';
        wait for k_clk_period/2;
end process;
```

Since **a process runs in parallel with the rest of the circuit**, this will
run continually in the background. Essentially, this process sets the
clock value low for exactly half of the period and then sets it high for
half of the period creating an oscillating signal we can use as our clock.

> Look through the `sim_proc` to see what the test bench is going to do.

### Simulation

{numref}`stoplight-waveform` shows what your simulation should look like.
Note the additional signals we have not seen in past simulations. Not only
can we check that the outputs are happening as expected, but we can
verify that the state $Q$ and next state $Q_{next}$ are behaving as they should.

```{hint}
By default the waveform will only show the signals in your test bench itself.
To add a signal from any component click "Window" and then "Scope."
Then select a subcomponent to view its internal signals. From there,
select a signal by highlighting where you declare it (double click will do this).
Right click on the highlighted signal and select "Add to Wave Window."

You will then need to run the simulation again. When you do this you will be prompted to save
the configuration. Click YES and add it to your project.
```

The ability to add signals in sub components will be a useful
tool when you are debugging your FSM designs. It can help you figure out
where your error is. For this design, if the state is correct but the
output is not then the issues is in your output equations. If `f_Q_next`
is correct but `f_Q` is not then maybe you are resetting on accident or
your process is incorrect.

```{figure} img/ice4_image16.jpg
---
name: stoplight-waveform
---
Stoplight FSM test bench waveform
```

> Take a screenshot of your waveform and commit it to your repo.

## top_basys3.vhd

Now that we have the stoplight FSM up and running, we need to create the
top_basys3 design to connect it to the board's hardware. We will do this
with the provided top_basys3.vhd file as well is the schematic seen in {numref}`stoplight-top-basys`.

```{figure} img/ice4_image17.png
---
name: stoplight-top-basys
---
top_basys3 design for stoplight ICE
```

In order to fully implement this design, we have to introduce a new
component that has been made for you.

The **clock divider**'s job is to take in the clock from the board, *which operates at 100 MHz*,
and slow it down to a speed we can observe. For this ICE we will slow the clk down to 1 Hz.

```vhdl
component clock_divider is
    generic ( constant k_DIV : natural := 2 );
    port (  i_clk    : in std_logic;    -- basys3 clk
            i_reset  : in std_logic;    -- asynchronous
            o_clk    : out std_logic    -- divided (slow) clock
    );
end component clock_divider;
```

The module uses a generic natural constant called `k_DIV` to allow you
to dynamically set how much you want to slow the clock down. This is
done at instantiation using a generic map statement similar to port
mapping and has been done for you in the top_basys3.vhd file.

The `i_reset` on the clock will effectively pause the clock when a high signal
is received.

> Explore the clock_divider to see if you can understand how it works.

A test bench has also been provided for the clock_divider. If you decide to run it,
set the clock_divider_tb file as top and simulate.

### Complete the file

Much of the top_basys3.vhd file has been done for you, however, you need
to declare the stoplight component and instantiated it. You also need to
fill out the port map for the clk divider already declared and instantiated.

> Complete top_basys3.vhd

## Hardware test

### Connect stoplight

Before we can test whether or not the stoplight works, we need to first
connect the stoplight to the FPGA.

We will be using the **Pmod** connection on the side of the Basys3.
{numref}`pmod-stoplight` shows how to connect the wires to Pmod *JA*.
Looking head on, the pins starting from the right and going to the left are
red, yellow, green, empty, ground.

```{figure} img/ice4_image18.png
---
name: pmod-stoplight
---
Pmod from above and from the side with connection labels
```

Luckily, the wires of the stoplight are color-coded! As you can see in
Figure 10, black will go to black, red to red, yellow to yellow, and
green to green.

```{figure} img/ice4_image6.jpg
Picture of stoplight model we will be using
```

### Test

> Edit the constraints file to ensure that all signals from the top_Basys3 entity are uncommented.

Then:

> Implement and generate bitstream. Push to your board.

Show an instructor that your design works! If you flip the
car input on, the system should cycle from red to green. If you flip the
car input off, the system should cycle to yellow, and then red. Pressing
the center button should immediately turn on the yellow light and
pressing the left button should pause the whole system.

## Deliverables

> Double check that you have everything **committed** to your git repo.
> Push the files.

| Deliverable           | Points |
|-----------------------|--------|
| Hardware Demo         | 50     |
| Passing GitHub Action | 50

Congratulations, you have completed In Class Exercise 4!

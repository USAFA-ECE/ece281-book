# ICE 3: Top Level Design

```{contents}
:local:
:depth: 2
```

## Overview

So far in this course you have implemented individual components on your
FPGA. However, one of the benefits of VHDL is that we can pull multiple
components into a single design.

In order to organize and reuse individual components, we are introducing the concept of a **Top Level Design**.

```{important}
The Top Level is where you pull in and connect all of the different components
before finally connecting them to the IO of the board.

This is how you will manage the complexity of combining numerous components.
```

You will need to use the concept of a Top Level Design in Lab 2,
so this is directly applicable.

### Objectives

The objectives of this in-class exercise are for you to:

- Implement and test a full adder with half-adders using VDHL
- Employ a `top_level.vhd` file
- Gain more experience using tools (Git, Markdown, Xilinx Vivado)

### End State

Provide a live demo to your instructor.

You are not required to turn in ICE3; however,
**ensure the following files are pushed to your repository**:

1. VHDL files used (top_basys3, half-adder and testbench)
2. Constraints (`.xdc`)
3. `.bit` file used to program board

## Background

As discussed in [ICE2](ICE2.md), a half-adder takes two single-bit inputs and outputs
their sum and a carry out; see {numref}`half-adder-summary` below.

A full-adder extends the half-adder to include a Carry In bit, $C_{in}$.

A truth table, schematic symbol, and logic equations for a full-adder are shown below.

```{figure} img/ice3_image1.jpg
Full-adder schematic
```

$$
S = A \oplus B \oplus C_{in}
$$

$$
C_{out} = AB + AC_{in} + BC_{in}
$$

```{table} Full-adder truth table
:name: full-adder-truth
| $C_{in}$ | $A$ | $B$ |   | $C_{out}$ | $S$ |
|----------|-----|-----|---|-----------|-----|
| 0        | 0   | 0   |   | 0         | 0   |
| 0        | 0   | 1   |   | 0         | 1   |
| 0        | 1   | 0   |   | 0         | 1   |
| 0        | 1   | 1   |   | 1         | 0   |
| 1        | 0   | 0   |   | 0         | 1   |
| 1        | 0   | 1   |   | 1         | 0   |
| 1        | 1   | 0   |   | 1         | 0   |
| 1        | 1   | 1   |   | 1         | 1   |
```

### Full-adder from multiple half-adders

It turns out that we can create a full-adder by combining two half-adders.

Recall from {ref}`half-adder-traits` a half-adder is:

```{figure} img/ice3_halfaddersummary.png
---
name: half-adder-summary
---
Half-adder schematic, truth table, and equations.
```

{numref}`full-adder-from-two` shows how two half-adders can be combined to function as a full-adder.

```{figure} img/ice3_fullfromtwo.png
---
name: full-adder-from-two
---
A full-adder from two half-adders
```

Our goal will be to implement this in VHDL!

## The Project

### Setup Vivado

Just like in the previous ICE, we will get our starter code from GitHub Classroom.

- Join the assignment to create your private copy of the repository
- Clone the code to your local workstation
- Build the project with `build.bat`

> Build your project, open it in Vivado, and modify headers.

### Import half adder

You created a half adder during [ICE2](ICE2.md). We will reuse that code here.

First, copy and past your completed the `halfAdder.vhd` file into `src/hdl/`

Then, follow the instructions in {ref}`manual-add-to-vivado-project`
to add `halfAdder.vhd` to the project as a source.
Make sure you see hallfAdder in the sources hierarchy.

The half-adder test bench has already been added for you.
We recommend you run the simulation just to verify that things
are working properly.

```{note}
Adding a file to the project will allow Vivado to see it and use it
but the file will **NOT** be automatically added if you migrate
the project to a new computer (such as with Git).

In order to do this you must {ref}`write-tcl-file`
and commit those changes as well.
You are not required to do this for the lab.
```

> Commit the addition of `halfAdder.vhd` with Git

## Top Level Design

Our top level design is the boundary of our system.

It allows us to connect internal components and our I/O.

### top_basys3 entity

Unlike the half-adder, the full-adder has three inputs, though it shares
the same two outputs. This has been given to you.

```{hint}
A std_logic_vector combines multiple std_logic bits into one variable.
The different signals within the vector can be referenced by index.
```

We will use switches for inputs and LEDs for outputs.

```vhdl

entity top_basys3 is
    port(
        -- Switches
        sw  :   in  std_logic_vector(2 downto 0);

        -- LEDs
        led :   out std_logic_vector(1 downto 0)
    );
end top_basys3;
```

### top_basys3 architecture

Always with layers of abstraction in mind, let's redraw the schematic
with the additional details of internal signals and board inputs and outputs.
This is shown in {numref}`full-adder-entity`.

```{figure} img/ice3_image5.jpg
---
name: full-adder-entity
---
Entity architecture for implementing full adder with two half-adders
```

You need to modify the architecture of your top_basys3 file to reflect
the schematic in {numref}`full-adder-from-two`.

#### Declare half-adder component

The half-adder component must match
the halfAdder entity declaration in `halfAdder.vhd`
These declarations go in between the architecture and begin statements.
Reference [ICE2 testbench](ICE2.md) if you don't remember the syntax.

```vhdl
architecture top_basys3_arch of top_basys3 is

    -- declare the component of your top-level design
    component halfAdder is
        port (
            i_A : in std_logic;
            i_B : in std_logic;
            o_S : out std_logic;
            o_Cout : out std_logic
            );
        end component halfAdder;

  -- declare any signals you will need

```

Just like the test bench pulls in your component to simulate inputs and outputs for it,
a top level design will bring in components and wire them together.
If you aren't sure what signals you may need to create, the general rule is to create
a signal for any wire not connected directly to an input or an output.

#### Instantiate half-adder components

Now that we have made the halfAdder component available to the top_basys3 entity,
we need to instantiate occurrences of the components and declare the logic to connect them
to each other, and to inputs/outputs.

Remember, we are trying to build what is in {numref}`full-adder-entity`.

Notice that we have TWO instantiations of halfAdder and we have given them unique names
(`halfadder1_inst` and `halfAdder2_inst`).
The `or` gate can be directly declared in "CONCURRENT STATEMENTS"

```vhdl
-- PORT MAPS --------------------
    halfAdder1_inst: halfAdder
    port map(
        i_A     => sw(0),
        i_B     => sw(1),
        o_S     => w_S1,
        o_Cout  => w_Cout1
    );

    halfAdder2_inst: halfAdder
    port map(
        i_A     => -- TODO
        i_B     =>
        o_S     =>
        o_Cout  =>
    );
    ---------------------------------

    -- CONCURRENT STATEMENTS --------
    led(1) <= -- TODO
    ---------------------------------
```

## Test design

We already have a completed `halfAdder_tb.vhd` file, which is awesome, because it means
we can test our system at multiple layers!

Your job is to complete the `top_basys3_tb.vhd` file and test the system as a whole.

### Edit testbench

Open `top_basys3_tb.vhd`.
Note the header has `top_basys3.vhd` as a REQUIRED FILE.
Revisit [ICE2](ICE2.md) for more detailed instructions if you need them.

Declare your top_basys3 "component" in the test bench, create signals to simulate
the inputs and outputs, and connect them in a port map.

Then create your test cases within the test process. Here is an example of the first line:

```vhdl
    begin

        w_sw <= x"0"; wait for 10 ns;
            assert w_led = "00" report "bad x0" severity failure;
        w_sw <= x"1"; wait for 10 ns;
            assert w_led = "01" report "bad x1" severity failure;
        --You must fill in the remaining test cases.
```

Just like in [Lab 1](../lab/lab1.md) we are using hex to set our 3-bit input.
We could use hex for the output too, but for two bits binary is easier.

> How many test cases will you need? Create them all 😄

### Simulate project

Your half-adder *should* be working already, so focus on the top_basys3_tb.
Use your waveform to debug if any of the assert statements fail.

## Implement Design

### Constraints File

Unlike our pervious exercise, you will not need to replace the highlighted portions.
We made our entity inputs and outputs reflect the default constraint file naming convention.

```xdc
## Switches
#set_property PACKAGE_PIN V17 [get_ports {sw[0]}]
    #set_property IOSTANDARD LVCMOS33 [get_ports {sw[0]}]
```

> Uncomment all lines needed for this design (`sw{0}`, `sw{1}`, `sw{2}`, `led{0}`, and `led{1}`.

### Test bitstream on FPGA

Synthesize, implement, generate.

Squirt the bitstream to the board, and test!

## Wrapping up

See [GitHub real fast](../appendix/github.md) if you need some tips.

### README

Take a screenshot of your waveform, save it to an the top level
of your directory (or you can use an `img/` folder).

Make the screenshot appear in your README with the following Markdown syntax:

```markdown
![description of my waveform](imagename.png)
```

Add a `## Documentation` section to the README. This is the only
one you need for the entire ICE; you don't need one in the file headers.

```markdown
## Documentation

My statement here.
```

Add the image and README.md to git and commit.

### Other files

- Run `git status`
- Double check that you have all `.vhd` and `.bit` files **committed** to your git repo.
- Then [push](https://www.atlassian.com/git/tutorials/syncing/git-push) them to your repo.
- Make sure all your work appears in GitHub as you expect.
- Make sure the GitHub action passed.
- Demo your working board to an instructor.

Congratulations, you have completed In Class Exercise 3!

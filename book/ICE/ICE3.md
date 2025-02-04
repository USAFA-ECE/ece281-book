# ICE 3: Ripple-Carry Adder

```{contents}
:local:
:depth: 2
```

## Overview

In this ICE we will make a 4-bit ripple-carry adder.

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

- Implement and test a ripple-carry adder with full adders using VDHL
- Employ a `top_level.vhd` file
- Gain more experience using tools (Git, Markdown, Xilinx Vivado)

## Background

### Full Adder

A full adder extends the half-adder to include a Carry In bit, $C_{in}$.

```{tip}
Adders are discussed on page **238** of *Digital Design and Computer Architecture, RISC-V Ed.*
```

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

### Ripple-Carry Adder

> The simplest way to build an $N$-bit carry propagate adder is to chain together $N$ full adders.

```{figure} img/ice3_ripplecarryadder.png
---
name: ripple-carry-adder
---
**DDCA* Figure 5.5 shows a 32-bit ripple carry adder.
```

Our goal will be to implement this in VHDL!

## The Project

### Setup Vivado

Just like in the previous ICE...

- Navigate to https://github.com/USAFA-ECE/ece281-ice3
- **Use this template**
- Clone *your copy* of the repository
- Open the project in Vivado
- Modify the headers

Notice that we have include a Full Adder for you. It's very similar to the Half Adder you made for ICE 2!

%% This Section should be changed to "Full Adder" next year, after modifying ICE 2
<!-- ### Import half adder

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
-->

## top_basys3 Entity

```{important}
From now on, we will use a top level design file as the boundary of our system.

It allows us to connect internal components and our I/O in a reliable manner.
```

We will name the top-level file `top_basys3.vhd`... because it's the top file for our Basys 3 board!

Open that file now.

Previously, you edited `Basys3_Master.xdc` to match your entity.
Going forward, we will leave the `xdc` defaults and *only uncomment* the ports we need.
You'll find that this will reduce errors and increase reusability.

Our `Basys3_Master.xdc` has the following default for switches:

```tcl
set_property PACKAGE_PIN V17 [get_ports {sw[0]}]
    set_property IOSTANDARD LVCMOS33 [get_ports {sw[0]}]
```

But instead of doing each bit, we will use a `std_logic_vector`:

```{hint}
A `std_logic_vector` combines multiple `std_logic` bits into something like an array.
The different signals within the vector can be referenced by index.
```

Our **user** will input the following:

- $C_{in}$ on switch 0
- $A$ on switches 4-1
- $B$ on switches 8-5

Our **user** will expect the following outputs:

- $Sum$ on LED 3-0
- $C_{out} on LED 15

**Sketch this *entity* description (none of the internal workings), then**
ensure you see the following in your `top_basys3.vhd`:

```vhdl

entity top_basys3 is
    port(
        -- Switches
        sw  :   in  std_logic_vector(8 downto 0);

        -- LEDs
        led :   out std_logic_vector(15 downto 0)
    );
end top_basys3;
```

## top_basys3 Architecture Declarations

You previously made an architecture schematic in Digital for the HW.
Open that file now for your reference.

You need to modify the architecture of your top_basys3 file to reflect this schematic.

### Full Adder Component

The full adder component must match the entity declaration in `full_adder.vhd`.

Declare a full_adder component between the architecture and begin statements.

```vhdl
architecture top_basys3_arch of top_basys3 is

    -- declare the component of your top-level design
    component full_adder is
        port (
            i_A     : in std_logic;
            i_B     : in std_logic;
            i_Cin   : in std_logic;
            o_S     : out std_logic;
            o_Cout  : out std_logic
            );
        end component full_adder;
```

Just like the test bench pulls in your component to simulate inputs and outputs for it,
a top level design will bring in components and wire them together.

### Signal Declaration

Remember, signals are like wires between inputs and components.

We will need the following signals:

- `w_A` and `w_B` to carry the switch `sw` input to the components.
- `w_Sum` to carry the Sum outputs to the LEDs.
- A carry signal to carry the ripple *between* adders.

Replace the `?` with the appropriate number and copy this signal declaration into `top_basys3.vhd`

```vhdl
    -- declare any signals you will need
    signal w_A, w_B, w_Sum : STD_LOGIC_VECTOR(3 downto 0); -- for sw inputs to operands
    signal w_carry  : STD_LOGIC_VECTOR(? downto 0); -- for ripple between adders
```

## top_basys3 Architecture Instantiations

Now that we have made the full_adder component available to the top_basys3 entity
we need to instantiate multiple occurrences of the component and declare the logic to connect them
to each other, and to inputs/outputs.

Remember, we are trying to build what's in your schematic, which is the 4-bit version of {numref}`ripple-carry-adder`.

Here are the first two instantiations of full_adder.

Notice that each has a unique name (because of the number at the end) and is connected slightly differently.

Copy this code into `top_basys3.vhd` below the `begin` statement.

```vhdl
-- PORT MAPS --------------------
    full_adder_0: full_adder
    port map(
        i_A     => w_A(0),
        i_B     => w_B(0),
        i_Cin   => sw(0),   -- Directly to input here
        o_S     => w_Sum(0),
        o_Cout  => w_carry(0)
    );

    full_adder_1: full_adder
    port map(
        i_A     => w_A(1),
        i_B     => w_B(1),
        i_Cin   => w_carry(0),
        o_S     => w_Sum(1),
        o_Cout  => w_carry(1)
    );
```

Then add the other two full adder instantiations.

### Signal connections

Lastly, we need to connect our signals. Vectors make this easy!

Do this below your port maps.

```vhdl
    w_A <= sw(4 downto 1);
    -- TODO: w_B
    led(3 downto 0) <= w_Sum;
```

## Test design

We already have a completed `full_adder_tb.vhd` file, which is awesome, because it means
we can test our system at multiple layers!

Your job is to complete the `top_basys3_tb.vhd` file and test the system as a whole.

### Edit testbench

Open `top_basys3_tb.vhd`.

Do some quick math... how many possible inputs are there to our 4-bit adder?

More than we want to manually input! 😄

Fill out the remainder of the test bench with a few cases. Strive for decent coverage by:

1. The min and max cases (input all zeros and all ones)
2. The easy-to-forget cases (such $0 + 0 + C_{in} = 1$)
3. A few random ones from the middle.

Revisit [ICE2](ICE2.md) for more detailed instructions if you need them.

Declare your top_basys3 "component" in the test bench, create signals to simulate
the inputs and outputs, and connect them in a port map.

### Simulate project

Your full adder *should* be working already, so focus on the top_basys3_tb.

Use your waveform to debug if any of the assert statements fail.

## Implement Design

### Constraints File

Open `Basys3_Master.xdc`.

Because we used the appropriate naming conventions in `top_bays3.vhd` you **should not change the names!**

Simply uncomment `sw` $0$ to $8$ and all sixteen `led` lines.

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

Now do the same with your sketch of the **top_basys3 entity** that you made earlier (it can be a quick and dirty picture).

Add a `## Documentation` section to the README. This is the only
one you need for the entire ICE; you don't need one in the file headers.

```markdown
## Documentation

My statement here.
```

Add the image and README.md to git and commit.

### Other files

- Run `git status`
- Double check that you have the image, your README.md and all `.vhd` and `.bit` files **committed** to your git repo.
- Then [push](https://www.atlassian.com/git/tutorials/syncing/git-push) them to your repo.
- Make sure all your work appears in GitHub as you expect.
- Submit the assignment on **Gradescope**.
- Make sure the autograder in Gradescope passes!

Congratulations, you have completed In Class Exercise 3!

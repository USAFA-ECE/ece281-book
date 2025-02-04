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

### End State

Push the following files to your GitHub repository and submit to Gradescope:

1. VHD and XDC files in `src/`
2. `.bit` file used to program board

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

The full adder component must match the entity declaration in `fullAdder.vhd`.

Declare a fullAdder component between the architecture and begin statements.

```vhdl
architecture top_basys3_arch of top_basys3 is

    -- declare the component of your top-level design
    component fullAdder is
        port (
            i_A     : in std_logic;
            i_B     : in std_logic;
            i_Cin   : in std_logic;
            o_S     : out std_logic;
            o_Cout  : out std_logic
            );
        end component fullAdder;
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
    signal w_A, w_B : STD_LOGIC_VECTOR(3 downto 0); -- for sw inputs to operands
    signal w_carry  : STD_LOGIC_VECTOR(? downto 0); -- for ripple between adders
```

## top_basys3 Architecture Instantiations

Now that we have made the fullAdder component available to the top_basys3 entity
we need to instantiate multiple occurrences of the component and declare the logic to connect them
to each other, and to inputs/outputs.

Remember, we are trying to build what's in your schematic, which is the 4-bit version of {numref}`ripple-carry-adder`.

Here are the first two instantiations of fullAdder.

Notice that each has a unique name (because of the number at the end) and is connected slightly differently.

Copy this code into `top_basys3.vhd` below the `begin` statement.

```vhdl
-- PORT MAPS --------------------
    fullAdder_0: fullAdder
    port map(
        i_A     => w_A(0),
        i_B     => w_B(0),
        i_Cin   => sw(0),
        o_S     => w_Sum(0),
        o_Cout  => w_carry(0)
    );

    fullAdder_1: fullAdder
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

        w_sw <= o"0"; wait for 10 ns;
            assert w_led = "00" report "bad 000" severity failure;
        w_sw <= o"1"; wait for 10 ns;
            assert w_led = "01" report "bad 001" severity failure;
        --You must fill in the remaining test cases.
```

Similar to [Lab 1](../lab/lab1.md) we can set the enitre vector with a single value, but because there are only three bits,
we need to use octal instead of hex. The leading `o` is how you express an octal number in VHDL.

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
set_property PACKAGE_PIN V17 [get_ports {sw[0]}]
set_property IOSTANDARD LVCMOS33 [get_ports {sw[0]}]
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
- Double check that you have the image, your README.md and all `.vhd` and `.bit` files **committed** to your git repo.
- Then [push](https://www.atlassian.com/git/tutorials/syncing/git-push) them to your repo.
- Make sure all your work appears in GitHub as you expect.
- Submit the assignment on **Gradescope**.
- Demo your working board to an instructor.

Congratulations, you have completed In Class Exercise 3!

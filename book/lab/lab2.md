# Lab 2 - Seven Segment Display Decoder

Due: Lesson 17

```{contents}
:local:
:depth: 2
```

[Lab 2 GitHub Repo](https://github.com/USAFA-ECE/ece281-lab2)

## Overview

In this lab you will implement the
[seven-segment display](https://en.wikipedia.org/wiki/Seven-segment_display) decoder
in VHDL to display a hexadecimal digit. The hex digit will come from a
hardware-based binary to hex converter, which you will develop.

A four-bit value will be input using switches.
Upon pressing the center button, a seven-segment display will
output the correct hex digit, {numref}`hex-of-switch`

```{figure} img/lab2_image1.jpg
---
name: hex-of-switch
---
Binary-to-hex displayed on seven-segment display
```

### Objectives

1. Develop a hardware-based binary to hex converter
2. Display switch value on seven-segment display

### Supplies

- Basys3 board

### Collaboration

Your instructor will inform you if you can work in pairs or not. For all assignments in this course, unless
otherwise noted on the assignment, you may work with anyone. We expect
all graded work, to include code, lab notebooks, and written reports, to
be in your own work. Copying another person's work, with or without
documentation, will result in NO academic credit. Furthermore, copying
without attribution is dishonorable and will be dealt with as an honor
code violation.

## Background

When creating simple embedded digital designs, a seven-segment display
is a common way to display numbers or simple letters to the end-user. In
this lab, you will design a seven-segment display decoder. This decoder
takes a 4-bit binary input, and produces 7 bits that indicate whether
each "segment" of the display is **on** (`0`) or **off** (`1`).

There are seven "segments" (labeled a -- g, see {numref}`7seg-label`) and one decimal point
(we will ignore the d.p. in this lab) in a seven-segment display.

```{figure} img/lab2_image2.png
---
name: 7seg-label
---
Labeled segments in display
```

On our Basys3 board an anode determines which of the displays are on,
while cathodes allow you to turn on each individual segment,
{numref}`7SD-anodes`

```{figure} img/lab2_image18.jpg
---
name: 7SD-anodes
---
Basys3 7SD cathodes and anodes.
```

Said differently, you disable an entire display by connecting it to power, aka `1`.

Placing a `0` on a segment will cause it to light up, while a `1` will keep the segment dark.
This is called "active low."
For example, to display the number "0", a logic `0` must be placed on segments `a`-`f`,
while segment `g` will be a logic `1`.

In this way, you can display every hexadecimal digit:
"0" - "F" as shown in {numref}`7SD-digits`

```{figure} img/lab2_image3.png
---
name: 7SD-digits
---
Hexadecimal digits on a seven-segment display
```

## Pre-lab

1. **Thoroughly** read *Digital Design and Computer Architecture* **Example 2.10**, page 77-81.
2. Read Basys3 Reference [Basic I/O](https://digilent.com/reference/basys3/refmanual#basic_io) and [Seven Segment Display](https://digilent.com/reference/basys3/refmanual#seven_segment_display).
3. Complete the Gradescope Assignment (add rows to truth table and answer hardware questions).

## Lab

It is time to implement them in hardware with VHDL!

### Setup Vivado Project

We will be creating our own Vivado project, instead of giving you a pre-made one.
See [**Lab2 Vivado Buttonology.pptx**](https://usafa0.sharepoint.com/:p:/s/ECE281/EZ2NKQAyMX1BqbxHMI5yHiUBekrNlr77IPglt3RUFfbe2w?e=0QYZ4Q) in Class Materials for screenshots!

1. Go to the [Lab 2 GitHub Repo](https://github.com/USAFA-ECE/ece281-lab2)
2. Click the green "Use this template" button -> create a new repository.
3. Once your new repo is created, clone it into where you want it.
4. Open Vivado
5. Follow instructions in {ref}`create-new-vivado-project`. Name your project **binaryHexDisp**
6. Ensure your source hierarchy looks like you expect.

> Edit the file headers as necessary. Then commit your changes with git.

### Seven Segment Display Decoder

For better modularity, we will implement our seven segment display decoder as its own component.

#### File Creation

We have not given you a file, so you need to create one in Vivado. See [**Lab2 Vivado Buttonology.pptx**](https://usafa0.sharepoint.com/:p:/s/ECE281/EZ2NKQAyMX1BqbxHMI5yHiUBekrNlr77IPglt3RUFfbe2w?e=0QYZ4Q) in Class Materials for screenshots!

1. In the **Sources** tab click the blue **+** symbol.
2. Add or create design sources
3. **+** and Create file
4. Create file
    - File type: VHDL
    - File Name: sevenseg_decoder
    - File location: your project src/ folder (NOT default "local to project")
5. You should then see the file, click Finish.
6. Define a module:
    - Entity name: `sevenseg_decoder`
    - `i_Hex`, in, Bus, MSB: 3, LSB: 0
    - `o_Seg`, out, Bus, MSB: 6, LSB: 0
7. You should then see **sevenseg_decoder** added to Design Sources

> Commit this newly created file to git!

#### Architecture

Our truth table had $S_a$ to $S_g$. How does this map to hardware? Fundamentally, **we want to adhere to the constraints file.**
This is because we will ultimately want to do this:

```vhdl
-- Excerpt from expected sevenseg_decoder port map inside top_basys3
o_Seg => seg,
```

Returning to [Basys3 I/O](https://digilent.com/reference/basys3/refmanual#basic_io)
and comparing it to **Basys3_Master.xdc** we can see that:

![Basys3 Basic I/O](https://digilent.com/reference/_media/basys3-_basic_io_block_diagram.png)

- The pin `W7` is connected to cathode `CA`, which we have called `sA`.
- `Basys3_Master.xdc` shows `PACKAGE_PIN W7 [get_ports {seg[0]}]`, so **`sA` is the LSB**

Meanwhile,

- The pin `U7` is connected to cathode `CG`, which we have called `sG`.
- `Basys3_Master.xdc` shows `PACKAGE_PIN U7 [get_ports {seg[6]}]`, so **`sG` is the MSB**

Unfortunately, **this is the opposite of our prelab truth table 😭**

It's up to you to decide how to fix this *inside* sevenseg_decoder!

```{tip}
The answer to this needs to go in your lab report.
```

##### Behavioral vs. Structural modeling

There are two basic philosophies for modeling digital architectures:

- **structural** describes *how* the module would be composed as a hierarchy of simpler modules.
- **behavioral** describes *what* the logic does in terms of inputs and outputs.

We used *structural* for the adders earlier in this course.
We could use it here as well, giving each segment a Boolean equation:

$$
Sa = (D3')(D2')(D1')(D0) + (D3)(D2')(D1)(D0) + (D2)(D1')(D0') + (D3)(D2)(D1')
$$

But you'd then have to do this for each of your segments!

For this component, you are probably better off with a *behavioral* approach.
As a reminder, here is the behavioral code for a one-hot decoder.

```vhdl
-- Behavioral model for one-hot decoder
with i_sel select
o_D <=  "0001" when "00",
        "0010" when "01",
        "0100" when "10",
        "1000" when "11",
        "0000" when others;
```

How might you adapt this for your seven-segment decoder?

> Once you do this, commit your file with git.

### Test sevenseg_decoder

Follow steps similar to above to create a new file, except select **Add or create simulation sources**.
Name your test bench file `sevenseg_decoder_tb.vhd` with the entity **sevenseg_decoder_tb**.

Refer to previous assignments for how to make a test bench. Remember to bring in your sevenseg_decoder component!

Create a test plan to verify your decoder behaves as expected. Don't forget that you can use hex to assign values to signals.

> Once you are happy with your test, add a screenshot of your waveform to your folder and commit it and your other files with git.

### Top-level file

The functionality we are looking for is this:

```{mermaid}
flowchart LR
    binary([4-bit input]) --> hex[Convert to hex]
    hex --> button{Display active?}
    button --> out[Display hex output]
```

The **user** will:

- Input a 4-bit number on switches 3-0.
- Expect that value to be output *only* on display 0 when button C is pressed; otherwise all displays are off.

### Design

**Before writing code** draw the entity diagram for **top_basys3**.

- Ensure it has all the inputs and outputs you need.
- `sw` should just be four bits (and then only uncomment 0-3 in the constraints file).
- Include your **sevenseg_decoder** entity and all connections to internal signals and gates.
- DO NOT draw the inner architecture of sevenseg_decoder.
- You will need this for the lab report.

```{tip}
You entity I/O names should exactly match the port names already in the `Basys3_Master.xdc` constraints file.
```

### Implementation

Now connect everything in VHDL like you show in your diagram.

#### Using btnC

The output `an` is four bits but `btnC` is only one bit.
The easiest way to connect these is with a signal.
We will append `_n` to the signal name to remind ourselves it is active LOW.

```vhdl
w_7SD_EN_n  <= not btnC;

an(0)   <= w_7SD_EN_n;
an(1)   <= '1';
an(2)   <= '1';
an(3)   <= '1';
```

Alternatively, you could do this with some VHDL magic and the `()` aggregate operator:

```vhdl
an  <= (0 => w_7SD_EN_n, others => '1');
```

### Implement in hardware

Uncomment the relative lines in your constraints file.

Synthesize and implement your design. Then generate a bitstream and squirt it to your board.

> Commit the bitstream to your repo

## Deliverables

### Prelab

See instructions at the top of this lab.
Submit on Gradescope.

### Hardware demo

- You have to hold the center push button down for a 7SD to show your output. (10 pts)
- The correct hex digit is shown based on the switch positions for all input possibilities. (20 pts)

Demo can be performed live with an instructor (preferred) OR submitted via Teams video.

### Written Report

Seek to answer the fundamental question
**"How do you understand the cathode and anode structure, use a decoder, and map your design  to hardware?"**

Include the following sections:

- Abstract
- Introduction (background of how display works)
- Design methodology
- Discussion (lessons learned, how many hours spent, documentation statement for entire lab)

Rubric and template on Teams. Submit on Gradescope.

### Code on Gradescope via GitHub

- `sevenseg_decoder.vhd` and `sevenseg_decoder_tb.vhd` included in `src/`
- Extraneous comments are removed and code is formatted in a sane manner
- Screenshot of testbench waveform shows all 16 input combinations
- `Basys3_Master.xdc` appropriately uncommented
- Bitstream (`.bit`) file used for hardware demo in repo (in default location)
- Commit messages are useful in tracing the development of the project

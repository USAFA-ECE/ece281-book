# Lab 4 - Moore Elevator Controller

```{contents}
:local:
:depth: 2
```

## Overview

This lab is all about **designing complex systems!**
You will do this by integrating sequential and combinatorial building blocks
to design, test, and debug the **Basic Elevator Controller** from ICE5
before expanding functionality to an Advanced Elevator Controller.

In order to accomplish this, you will need to integrate modules from previous
in class exercises and labs. **Start now and make steady progress!**

### Authorized Resources

For this lab, you may work in **teams of two** (including the prelab).
If you worked in a team for Lab 3,
you **must** work with a *different partner* for this lab.

You may seek help from any cadet or instructor,
not limited to this course, and reference any publication in its
completion. Online resources are acceptable. However, no resources
containing solutions to the course homework, labs, exams, quizzes, or in
class/out of class exercises are allowed.

Your work (and your code) must **always** be your own.
Normal documentation of all resources utilized is required.

### Lab Flow

This lab *iterates* to first implement a Basic Elevator Controller,
and then expand that to an Advanced Elevator Controller.

#### Basic Elevator Controller

The **initial phase** of the lab only implements [ICE5](https://usafa-ece.github.io/ece281-book/ICE/ICE5.html)
Basic Elevator Controller, shown in {numref}`basic-elevator-controller`

```{figure} img/lab4_basic_controller.png
---
name: basic-elevator-controller
---
Basic Elevator Controller user interface
```

- The elevator’s current floor is shown on the second seven-segment display
- The elevator takes 0.5 seconds to move between floors
- The display updates according to the output
- The elevator must not teleport... no skipping floors
- Three push buttons are to be used for resets
- The Master Reset button resets both the FSM **and** the clock.
- The reset floor is **floor 2**.

This is the functionality who's schematic you must design in the prelab.

#### Advanced Elevator Controller

The **final phase** of the lab expands the basic functionality.

- The elevator will travel from floor 1 to floor 16
- Use the two left-most digits of the display to show the floor in decimal
- All other functions will stay the same as in Basic Elevator Controller.

```{hint}
Revisit ICE6 for using TDM to drive a seven segment display.

Reference [Basys3 Seven Segment Display](https://digilent.com/reference/basys3/refmanual#seven_segment_display)
```

This functionality should not be "hooked-up" in the Prelab.
However, the top level entity will reflect this advanced functionality from the start.

## Prelab

Create a Top Level Schematic showing your architecture
and **submit one submission per team on Gradescope.**

This schematic must *only cover the Basic Elevator Controller functionality*.
You will be expected to update the schematic prior to implementing the
Advanced Elevator Controller.

This schematic should consist of the **top_basys3** entity, its I/O,
the various modules inside it, and the connecting signals and logic.

The top-level entity in `top_basys3.vhd` will be:

```vhdl
-- Lab 4
entity top_basys3 is
    port(
        -- inputs
        clk     :   in std_logic; -- native 100MHz FPGA clock
        sw      :   in std_logic_vector(15 downto 0);
        btnC    :   in std_logic; -- GO
        btnU    :   in std_logic; -- master_reset
        btnL    :   in std_logic; -- clk_reset
        btnR    :   in std_logic; -- fsm_reset

        -- outputs
        led     :   out std_logic_vector(15 downto 0);
        -- 7-segment display segments (active-low cathodes)
        seg     :   out std_logic_vector(6 downto 0);
        -- 7-segment display active-low enables (anodes)
        an      :   out std_logic_vector(3 downto 0)
    );
end top_basys3;
```

The entity contains port you will not use for the Basic Elevator functionality,
but leave them in the schematic because you will need them for the Advanced Elevator.

```{warning}
Changing interfaces incurs massive engineering costs. This is because
**interfaces are the promises you make to external systems.**

Even though we want to iterate (rather than "hit a home run"), we save
a lot of time by making our I/O large enough for advanced functionality from
the start. Imagine if you had to change the size of a std_logic_vector each
time you wanted to tweak something.

But there is a cost! First, this requires additional hardware that may never be used.
Second, you may waste effort solving problems that don't actually need to be solved
because by the time you get to it the project requirements may have changed or
the implementation strategy may have changed.

Long story short, these sorts of decisions are hard and why Engineers get 💰
```

You need to include a clock divider module, and the remaining modules you
need will come from your previous in-class exercises and labs
(MooreElevatorController and sevenSegDecoder).

- Do not hand draw the schematic; rather, use a tool such as
    [Excalidraw](https://excalidraw.com/), [draw.io](https://app.diagrams.net/), or PowerPoint.
- The components inside your top level should remain black boxes.
    The expectation is you will update this schematic at the end of this lab to reflect your final design.
- Label **all** wires not connected to the top-level entity ports.
    These will be additional signals you will create in your VHDL code.
- Only turn on seven-segment display 2 (display 0 is the far right).
    Force the others OFF by disabling their anodes. Reference earlier
    activities regarding how to turn on/off displays.
- Turn OFF (ground) all LEDs, since you do not need them for the basic
    elevator functionality, but they are in the entity for variations on
    the functionality.
- Just like in the last lab, the clock divider uses *generics* so that
    you can redefine characteristics at instantiation. For instance, you
    can redefine the number of times the clock divider divides a clock
    for each instance of the module. Be sure to specify the value for
    your clock divider to obtain 2 Hz. For an idea on how to accomplish
    this, look at the generic mapping of the clock divider in ICE4 and Lab 3.
- All component names should match the labels given to them
    (ex: MooreElevatorController should be labeled MooreElevatorController)
- All components should have their internal ports labeled so it is clear
    what a signal is being connected to.

<!--
## Lab

Clone code template from GitHub Classroom.
Then open the project by double clicking `****.xpr`.

```{tip}
Much of this code can be extracted from previous projects.
Rather than open another Vivado project (and the confusion that comes with it),
we suggest you **view your previous project's GitHub repository in your web browser**.
```

## Deliverables

Below are the deliverables and point distributions for the Lab 3:

| Deliverable           | Points |
|-----------------------|--------|
| Prelab                | 20     |
| Hardware Demo         | 25     |
| Passing GitHub Action | 25     |
| Written Report        | 30     |

**Documentation statements** will be in the README for any help on the Lab itself.

```markdown

## Documentation
Your statement here.
```

Otherwise, the statement will be submitted on Gradescope
(for the prelab and report).

### Hardware Demo

- Show your instructor your waveform *before* demoing the hardware.
- Using left and right switches generates correct LED patterns with **[changes occurring at a rate of 4 Hz**
- Holding the clock reset freezes the light patterns
- Pressing the FSM reset *immediately* (i.e. asynchronously) resets the light patterns
- Demo can be performed live with an instructor OR via a video link
- You must show good test cases for full credit!

### GitHub Actions

- All `.vhd`, `.xdc`, `.xpr`, and `.bit` files committed to repository
- Headers are modified appropriately
- *Multiple* commits with *good* messages
- **Remove extraneous code and comments**
- Your comments explain what you are doing and why
- Student testbench passes for `thunderbird_fsm_tb.vhd`
- Instructor testbench passes for `top_basys3_tb.vhd`
- README includes `## Documentation` statement
- README includes a `.png` Waveform

### Report

The report will be submitted on Gradescope. Subsections are described below.
The report should be kept to three pages, including diagrams.

```{important}
This report should be written in a professional, technical style.
This means, among other things:
- ~~Active voice should be used~~ Use **active voice**, in most cases. Occasionally passive voice is more appropriate,
    but it should be a deliberate choice.
- The use of **we** is appropriate.
- Write sentences that make information understandable; such sentences are beautiful.

Know your audience: the instructors have a strong background in the lab.
We would rather see you showcase your ability to communicate your hard-earned knowledge
than be bored with broad summaries.
**The true art is to 1) provide *context* and 2) follow it with *consequence*.**
```

#### Front Matter

- Title, Authors, page numbers, ect.
- Documentation Statement for the Report only.
- A two or three sentence summary of the **key takeaway** from this lab.
    Don't drone on, make it a punchy answer to "So what? Who cares?"

#### Simulation Results

- Discuss your **overall approach** to testing.
- Include your waveform.
- Discuss **how simulation aided in testing** and iterating.
- Show that you actually used the simulation to assist before moving to hardware.

The waveform should:

- Begin with a reset test to start the FSM in a known state
- Clearly show well thought out test cases with correct results
- Include all required signals, (i_clk, i_reset, i_left, i_right,
    o_lights_L, o_lights_R, and your current and next state bits).
- Light output busses expanded to easily see progression of lights
- Ensure the simulation results waveforms have visible values for inputs and outputs.

#### Architecture Design

- Discuss your **overall approach** to architecture design and schematic construction.
- Did you implement one-hot encoding or binary encoding? **Evaluate** your choice.
- Include your neat (not hand-drawn) schematic.
- Include the RTL schematic.
- Discuss any differences between the two schematics.

#### Conclusion

- Your main takeaway!
- What approaches will you continue or change when it comes Lab 4?
- Feedback for the instructors on the lab. -->

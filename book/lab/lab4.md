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

The **final phase** of the lab expands the basic functionality, see below.

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
        btnU    :   in std_logic; -- master_reset
        btnL    :   in std_logic; -- clk_reset
        btnR    :   in std_logic; -- fsm_reset

        -- outputs
        led :   out std_logic_vector(15 downto 0);
        -- 7-segment display segments (active-low cathodes)
        seg :   out std_logic_vector(6 downto 0);
        -- 7-segment display active-low enables (anodes)
        an  :   out std_logic_vector(3 downto 0)
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
- LED 15 should be connected to the clock signal that drives the FSM
- All other LEDs should be grounded.
- Just like in the last lab, the clock divider uses *generics* so that
    you can redefine characteristics at instantiation. For instance, you
    can redefine the number of times the clock divider divides a clock
    for each instance of the module. Be sure to specify the value for
    your clock divider to obtain 2 Hz. For an idea on how to accomplish
    this, look at the generic mapping of the clock divider in ICE4 and Lab 3.
- All component names should match the labels given to them
    (ex: elevator_controller should be labeled elevator_controller)
- All components should have their internal ports labeled so it is clear what a signal is being connected to.

## Lab

Clone code template from GitHub Classroom.
Then open the project by double clicking `elevator_fsm.xpr`.

```{tip}
Much of this code can be extracted from previous projects.
Rather than open another Vivado project (and the confusion that comes with it),
we suggest you **view your previous project's GitHub repository in your web browser**.
```

### Basic Functionality

First, directly implement the pre-lab scematic.

#### Add files

You will need to add VHDL files from your previous labs and ICEs to the project.
Reference {ref}`manual-add-to-vivado-project`

You need to add any design sources that you plan to bring in as a module.
You do not need to bring in the testbench for these modules, since
we have tested them in previous labs. However, you can if you'd like.

#### Complete top level

Using your Prelab drawing, complete the top-level module. Remember this basically involves:

1. Copying in component definitions
2. Creating additional signal wires
3. Instantiating your component instances and connecting all wires

For the clock divider, what value for k_DIV do you need to produce a 2 Hz clock?

#### Simulation

*A brief discussion on testing philosophy.*

The type of testing you choose results in a tradeoff of **complexity** vs.
how much **confidence** your test gives you that your design works.

Each component within your top_level already has been simulated and tested.
This is known as **unit testing** because you make sure that each
unit (component) does what it is supposed to.
Unit tests give you confidence that a single thing works and they are
quick to write and run. But unit tests don't give you any information on how components
will or won't work together.

**Integration testing** tests if multiple units work together;
for example, does the clock_divider indeed provide a slow clock to the FSM?
While this type of testing is *awesome* for software design,
it's difficult to do in hardware design.
Rather than use a VHDL simulation, we have done our integration testing
in previous ICEs and Labs by actually squirting to hardware.
This is more complex and takes more time than unit tests do, but in turn
we get greater confidence that the system works.

For Lab 4, the only thing that isn't already tested is if the entire thing works!
This type of testing is **end-to-end** testing. If the test passes then we know
the entire system works. Unfortunately, end-to-end tests are also very complex to design and maintain.
Because of this **you do not need to write a simulation test bench for this lab.**

As engineers, part of our job is to determine what is the appropriate level of confidence
and justify designing tests to meet that level. For this particular application,
the cost of simulating an end-to-end test is too high, so we will debug on hardware (yikes!).

#### Deliver

- Don't forget to uncomment lines in you XDC file.
- Generate bitstream and program device!
- Show it to your instructor
- Commit to git and **tag** (see below)

> Demonstrate your basic functionality to your instructor.

After you have demonstrated basic functionality, you want to make sure you
save that result in case you ever need to go back to it. To do this we will
use [git tag](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag).

The normal flow:

1. Type `git status` to see the current state
2. Add all hardware files with `git add src/hdl/*.vhd`
3. Add your constraints file with `git add src/hdl/*.xdc`
4. Add you `.xpr` file.
5. Add any other files you need to commit (like your bitstream)
6. Run `git status` again to make sure you are good.
7. Commit and push!
8. Make sure your github action looks good

```{note}
Because we did not make a testbench to run, the GitHub Action simply runs
[GHDL Synthesis](https://ghdl.github.io/ghdl/using/Synthesis.html#synthesis)

This is an experimental feature.
```

Now for the annotated tag. The `-a` tells git to expect a message.
The message can be whatever you'd like, but please be sure to tag as `basicFunc`.

```bash
git tag -a basicFunc -m "Four floor controller"
```

Now push the tag to GitHub

```bash
git push --follow-tags
```

You should now see the tag in your repository.

```{figure} img/lab4_git_tag.png
A tag saved in a GitHub repository
```

You can go back to this saved point at any time!

```bash
git checkout basicFunc
```

The point of a tag is that you cannot edit the branch.
If for some reason you need to... use Google.

> Push your tagged basic functionality to GitHub.

### Advanced Functionality

*After* demonstrating your basic functionality *and after* pushing your `basicFunc` tag to GitHub,
it is time to implement advanced functionality!

- The elevator will travel from floor 1 to floor 16
- Use the two left-most digits of the display to show the floor in decimal
- All other functions will stay the same as in Basic Elevator Controller.

```{hint}
Revisit ICE6 for using TDM to drive a seven segment display.

Reference [Basys3 Seven Segment Display](https://digilent.com/reference/basys3/refmanual#seven_segment_display)
```

**Before** you start to code, update your schematic. This schematic will be submitted in the Lab 4 Report.

> (Optional but recommended) Show your schematic to your instructor

**After** you have finished your schematic, make it work!

> Implement advanced functionality and demonstrate to instructor.

Commit & push your changes.

> Push changes for advanced functionality to GitHub.

## Deliverables

Below are the deliverables and point distributions for the Lab 3:

| Deliverable           | Points |
|-----------------------|--------|
| Prelab                | 20     |
| Basic Controller Demo | 10     |
| Advanced Demo         | 30     |
| Passing GitHub Action | 10     |
| Written Report        | 30     |

The report will be very similar to Lab 3 Report requirements, with the following changes:

Things to Exclude:

- Do not include RTL schematics or discussion
- There is no need to include a waveform

Things to Add:

- Share your thoughts on mudlarity and abstraction
- Discuss how your schematic changed (and what stayed the same) between Basic and Advanced Controllers
- Discuss how you used TDM to get the Seven Segment Display functioning
- What is the problem with the MASTER RESET?

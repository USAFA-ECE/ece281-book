# Lab 0: Install Vivado

That's right, it takes an entire lab to make sure Vivado is setup!

This lab will have you:

1. Install Vivado
2. Setup a Vivado project
3. Test our ability to squirt a bitstream to our FPGA
4. Ensure GitHub classroom is setup

## Install Vivado

Follow the instructions in `Vivado Installation Instructions.pdf` under the Teams Files tab.

## Setup a Vivado project

### Clone a repository with git

Clone [GitHub USAFA-ECE helloLed](https://github.com/USAFA-ECE/helloLed)

### Build the Vivado project

We will dive into what's going on here later in ICE 2 {ref}`setup-vivado-project`.
But for now,

1. Run the `build.bat` file
2. Double click the generated `helloLed.xpr` to open it in Vivado

### Test Vivado project

In the Flow Navigator pane on the left hand side:

1. Under SIMULATION, click "Run Simulation" then **Run Behavioral Simulation**.
2. Verify that a waveform shows up.
3. Under PROGRAM AND DEBUG, click **Generate Bitstream**.
4. Click "Yes" when asked if it is OK to launch implementation. Then click "OK"
5. Wait for the popup telling you Bitstream Generation Complete. This may take some time, see the spinning green circle in the very top right corner of the window. Click "OK".

### Program FPGA

We will go into more detail with {ref}`download-onto-fpga`.
For now just:

1. Plug in your FPGA
2. See the pretty lights. This is the built-in demo program.
3. Under PROGRAM AND DEBUG
4. Click Open Hardware Manager
5. Click **Open Target** then **Auto Connect**
6. Click on **Program device**
7. By default the Bitstream (`.bit`) file that you last created will be selected for programming. Click **Program**

Test that the program worked by flipping the rightmost switch (`SW0`) on and off. The LED directly above it should match. None of the other switches should do anything.

> Demo this to your instructor.

## GitHub setup

If you have not already completed [](../ICE/ICE0.md), do so now.

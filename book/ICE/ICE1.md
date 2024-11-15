# ICE 1: OR Gates

```{contents}
:local:
:depth: 2
```

## Overview

Due: Lesson 6

In this course you have learned how to design basic combinational logic circuits. In this ICE you will implement a basic logic design using transistor-transistor logic (TTL) chips. This will help you prepare for Lab 1.

### Objectives

1. Implement and test a simple logic design in hardware.
2. Gain experience using hardware.

### Agenda

1. Build the required circuit.
2. Test your circuit.
3. Demonstrate the circuit's operation to an instructor.

### Supplies

- 5x $1 kΩ$ resistors.
- 1x red LED.
- 1x 74LS08 2-Input AND gate logic chip.
- 1x 74LS86 2-Input XOR gate logic chip.
- 1x breadboard power supply.
- 1x breadboard.
- 2x single pole double throw switches.
- Jumper wires.

### Collaboration

For this lab, **work in teams of two**. You may seek help from any ECE 281 instructor or authorized online resource, but may not utilize any student other than your partner. Documentation is required (you do not need to document work with your partner). We expect all code or design work to be your own.

## Background

### The Half Adder Logic

You will implement the logic of a half adder using transistor-transistor logic (TTL) chips:

The half adder takes two inputs, \( A \) and \( B \), and produces two outputs:
**Sum** (\( S \)) — represents the sum of \( A \) and \( B \).
 **Carry** (\( C \)) — represents the carry-out when \( A \) and \( B \) are both high (logic one).

The logic equations for the half adder outputs are:
$$
S = A \oplus B
$$
$$
C = A \cdot B
$$
where "$\oplus$" represents the XOR operation (Sum) and "$\cdot$" represents the AND operation (Carry).

### The Chips

You will use two TTL chips to build the half adder:
1. **74LS08** — A quad, 2-input AND gate, used to produce the Carry output.
2. **74LS86** — A quad, 2-input XOR gate, used to produce the Sum output.

```{figure} img/ice01_image2.png
---
name: LS08-Pin-Layout
---
74LS08 Pin Out
```

```{figure} img/ice1_74LS86.png
74LS86 Pin Out
```

Data sheets for these chips (74LS08.pdf and 74LS86.pdf) are available under **Data Sheets** on the Team. The `74LS08-TTL-Layout` and `74LS86-TTL-Layout` diagrams provide the chip layouts.

### Connecting the Chips

We will create a half adder circuit using the following process:
Connect the first AND gate from the 74LS08 to the inputs \( A \) and \( B \) to produce the Carry output.

Connect the first XOR gate from the 74LS86 to the inputs \( A \) and \( B \) to produce the Sum output.

We will get the following logic:

```{figure} img/ice01_image3.jpg
---
name: Half Adder logic
---
Half adder implementation
```

This setup will give you a half adder circuit, where:
**Sum (S)** is high when exactly one of the inputs is high.
**Carry (C)** is high only when both inputs are high.


### Breadboards

A breadboard is a prototyping device that allows you to easily connect different portions of a circuit together {numref}`breadboard-connections` depicts how the
breadboard used in this ICE is connected. Each row (on either side of the center line) is a single node. This means two devices can be connected together by inserting them in the same row. Additionally, each of the columns on the outside of are connected the entire length of the board. These columns are ideal for supplying power and ground to your circuits.

```{figure} img/ice01_image4.jpg
---
name: breadboard-connections
---
Breadboard connections
```

## Build the circuit

### Power Supply

You will use a USB power supply to power your circuit. Attach the
power supply to the top of the breadboard so the red vertical lines
align with the 5V and 3.3V pins and the blue lines align with the
ground pin according to {numref}`power-supply-placement`. Insert the USB cable into your
computer. Press the white button to turn on the power supply.

```{figure} img/ice01_image5.jpg
---
name: power-supply-placement
---
Power supply placement
```

You will use a digital multi-meter (DMM) to test the power supply.
Turn the multimeter to the DC voltage setting and place the black lead into the ground rail and the red lead into the 5V rail according to {numref}`test-power-supply`.

```{figure} img/ice01_image6.jpg
---
name: test-power-supply
---
Testing power supply with DMM
```

> The meter should read about 5V. Once you have successfully tested,
turn off your power supply by depressing the white button.

### Switches and Resistors

Next you are going to configure your input switches. This portion of the circuit will allow us to provide a logic `0` to the circuit (0 V or low) when the switch is not active and a logic `1` to the circuit(5 V or high) when the switch is active. The switches and resistors are going to be in a pull down configuration or active high. This means when the switch is **open** there is a direct connection between
the circuit and ground through a resistor which pulls the value to `0`.

This provides a logic `0`. However, when the switch is **closed**, the circuit is now connected to the power supply (5 V). This provides a logic `1`. We will use 2x $1 kΩ$ resistors and 2-switch component.

```{figure} img/ice01_image7.jpg
---
name: pull-down-resistor
---
Pull-down resistor configuration
```

{numref} shows how to connect the single pole double throw switches. 

```{figure} img/ice01_image8.png
---
name: Single throw double pole switch
---
Switch wiring configuration
```

Now you will test the circuit.

Connect the black lead of your DMM to ground and connect the red lead to one of the switches on the
"switched" side of the switch (middle node). Select the DC voltage option on your meter and turn on your USB power supply.

The meter should read about 5 V.
When active, the switch acts just like a wire, therefore, we should see the same value from the voltage source. The meter should read 0V when the switch is not active.

> Repeat this testing for the other switched value (B). When complete, turn off the power supply.

### Chip Installation

You will now add the 74LS08 chip, and the 74LS86 chip.

Place the chip along the center spacing to ensure each lead of the chip is on its own node (and not connected). Note the indentation and orientation of the chip. Wire the +5V of the 74LS08 to the corresponding Vcc pin (pin 14 on the data sheet). Repeat this with the 74LS86. Connect the ground pin (pin 7) of the 74LS08 to the ground rail. Repeat this with the 74LS86.

The switch outputs will be wired to pins 1 and 2 of the 74LS08. 

- Connect the switch 1 output to pin 1 on the 74LS08.
- Connect the switch 2 output to pin 2 on the 74LS08.

Next, we will connect the 74LS08 AND chip to the 74LS86 XOR chip. Please note -- these will be connected on the same node as the switch outputs. 

- Connect pin 1 (A) of the 74LS08 to pin 1 of the 74LS86.
- Connect pin 2 (B) of the 74LS08 to pin 2 of the 74LS86.

```{figure} img/ice01_image9.png
---
name: Chip wiring
---
Chip wiring
```

According to our equation, if any single switch is active, the **SUM** operation will be high. 

> Connect the black lead of the DMM to ground and the red lead to the output of pin of the XOR gate (Pin 3). Turn on the power and test your circuit by turning a single switch on.

According to our equation, if both switches are active, the **CARRY** operation will be high. 

> Connect the black lead of the DMM to ground and the red lead to the output of pin of the AND gate (Pin 3). Turn on the power and test your circuit by turning both switches on.

Notice that the meter is reading closer to 4.5 V and not 5 V. Fortunately, in both the 74LS08 and 74LS86, anything above 2 V is considered high, or logic `1`. Similarly, anything less than 0.8 V would register as a logic `0`.

### Wire LED

The final step is to wire the LED. This LED will indicate if the
circuit output is a logic `1` (LED is on) or logic `0` (LED is off).

```{warning}
To keep from destroying the LED due to a potential current spike, you
will wire the LED **in series** with a $1 kΩ$ resistor.
```

LEDs are directional components. The *longer* of the LED's two leads are where current enters, where the shorter end is where current exits. Place the LEDs across two empty nodes, connecting the shorter ends to ground. Ensure the positive ends are on empty nodes. 

Next, connect pin 3 (output pin) of the 74LS08 and 74LS86 to two different empty nodes. Use a 1 kΩ resistor to connect these nodes to the the nodes where the positive ends of the LEDs are connected. *You should have a 1 kΩ resistor between the LED and the output pin nodes*. 

> Wire up the LED according to {numref}`led-wire`

```{figure} img/ice01_image11.png
---
name: led-wire
---
LED wiring
```

For your final test, simply turn on the power and test your switches.

If any single the switch is turned **on** the LED connected to the XOR output will light up (**Sum**). If both switches are turned **on**, the LED connected to the AND output will light up (**Carry**). If the switches are **off**, the LEDs should remain off.

Congratulations! You have successfully completed In Class Exercise (ICE) 1.
This will be directly applicable to your circuit building in lab 1!

> Demonstrate your operational circuit to your instructor.

## Exit Criteria

The instructor will check the circuit accurately implements a half adder function:

- When all switches are **off**, LED is off.
- When any one switch (A or B) is turned **on**, the LED connected to the 74LS86 will light up.
- When both switches (A and B) are turned **on**, the LED connected to the 74LS06 will light up.

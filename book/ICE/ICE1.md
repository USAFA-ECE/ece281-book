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
- 1x SN7432 2-Input OR gate logic chip.
- 1x breadboard power supply.
- 1x breadboard.
- 1x 4-dip switch, 2x 2-dip switch, or 4x 1-dip switch.
- Jumper wires.

### Collaboration

For this lab, **work in teams of two**. You may seek help from any ECE 281 instructor or authorized online resource, but may not utilize any student other than your partner. Documentation is required (you do not need to document work with your partner). We expect all code or design work to be your own.

## Background

### The logic equation

You will implement the simple logic equation using transistor-transistor logic (TTL) chips:

$$
Y = A + B + C + D
$$

where `+` represents OR. If any input, $A$, $B$, $C$, or $D$, is high (logic one), then the output, $Y$, is high (logic one). If all four inputs are low (logic zero), then the output is low (logic zero).

### The chip

You will use a SN7432 TTL chip to build this logic equation, which includes four, 2-Input OR Gates. You can find the data sheet for the chip (sn74ls32.pdf) on the Team under Data Sheets. {numref}`SN7432-TTL-Layout` provides the chip layout.

```{figure} img/ice01_image2.jpg
---
name: SN7432-TTL-Layout
---
SN7432 TTL Layout
```

```{figure} img/ice1_7432inard.png
Inside makeup of SN7432
```


The SN7432 chip only contains 2-Input OR gates, but the logic equation
requires a 4-Input OR gate. Boolean algebra can be used to find an
arrangement of 2-Input OR gates that will work. The simple logic
equation can now be represented as:

$$
Y = (A + B) + (C + D)
$$

```{figure} img/ice01_image3.jpg
---
name: 4-Input-OR
---
4-Input OR Implementation
```

The 4-Input OR equation is now implemented using three, 2-Input OR  gates.

### Breadboards

A breadboard is a prototyping device that allows you to easily connect
different portions of a circuit together.
{numref}`breadboard-connections` depicts how the
breadboard used in this ICE is connected. Each row (on either side of
the center line) is a single node. This means two devices can be
connected together by inserting them in the same row. Additionally,
each of the columns on the outside of are connected the entire length
of the board. These columns are ideal for supplying power and ground
to your circuits.

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
Turn the multimeter to the DC voltage setting and place the black lead
into the ground rail and the red lead into the 5V rail according to
{numref}`test-power-supply`.

```{figure} img/ice01_image6.jpg
---
name: test-power-supply
---
Testing power supply with DMM
```

> The meter should read about 5V. Once you have successfully tested,
turn off your power supply by depressing the white button.

### Switches and Resistors

Next you are going to configure your input switches. This portion of
the circuit will allow us to provide a logic `0` to the circuit (0 V
or low) when the switch is not active and a logic `1` to the circuit
(5 V or high) when the switch is active. The switches and resistors
are going to be in a pull down configuration or active high. This
means when the switch is **open** there is a direct connection between
the circuit and ground through a resistor which pulls the value to `0`.
This provides a logic `0`. However, when the switch is **closed**, the
circuit is now connected to the power supply (5 V). This provides a
logic `1`. We will use 4x $1 kΩ$ resistors and 4-switch component.

```{figure} img/ice01_image7.jpg
---
name: pull-down-resistor
---
Pull-down resistor configuration
```

{numref}`2-dip-switch` shows how to connect the 4-dip switch (via 2 x double switch
or 4 x single switch) and 4 resistors on the breadboard.

```{figure} img/ice01_image8.jpg
---
name: 2-dip-switch
---
2x 2-Dip switch and resistor configuration (Left) and 4x 1-DIP configuration (Right)
```

Now you will test the circuit.

Connect the black lead of your DMM to
ground and connect the red lead to one of the switches on the
"switched" side of the switch (right side for the blue switch, middle
for the black switch). Select the 20V DC voltage option on your meter
and turn on your USB power supply.

The meter should read about 5 V.
When active, the switch acts just like a wire, therefore, we should
see the same value from the voltage source. The meter should read 0V
when the switch is not active.

> Repeat this testing for each of the
four switched values (A-D). When complete, turn off the power supply.

### 7432 Chip

You will now add the 7432 chip, which provides 4 logic OR gates.

Place the chip along the center spacing to ensure each lead of
the chip is on its own node (and not connected). Note the indentation
on the switch, this is the OP. You will wire the pin to 5 V and the
pin to ground (the negative 1-1 on the power supply).

Each of the switch outputs will be wired to one of the gate inputs and the outputs
of the two gates will be connected to the inputs of a third gate. The
switch outputs are located between the resistor and the switch.

- Connect the switch 1 output (top-most switch) to pin 1 on the chip
- Connect the switch 2 output to pin 2 on the chip.
- Connect the switch 3 output to pin 4 of the chip.
- Connect the switch 4 output (bottom-most switch) to pin 5 of the chip.

Next you need to connect the outputs of the first two OR gates to the third OR gate. Pin 3 needs to be connected to pin 10 and pin 6 needs to be connected to pin 9. {numref}`sn7432-wiring` demonstrates how the switch outputs should be connected. The blue bar is ground (the negative on the USB) and the red bar is $V_{cc}$ (5V).

```{figure} img/ice01_image9.jpg
---
name: sn7432-wiring
---
SN7432 Wiring
```

According to our equation, if any of the switches are active, the
output of the chip should be high (logic 111 or 5V).

> Connect the black lead of the DMM to ground and the red lead to the output of pin of the
final OR gate (Pin 8). Turn on the power and test your circuit.

Notice that the meter is reading closer to 4.5 V and not 5 V. Luckily, according to
the data sheet seen in {numref}`sn7432-high-low`, anything above 2 V is considered high
or logic `1`. This means that 4.5 V would register as a logic `1` and
anything less than 0.8 V would register as a logic `0`.

```{figure} img/ice01_image10.jpg
---
name: sn7432-high-low
---
SN7432 High and low voltage ratings
```

### Wire LED

The final step is to wire the LED. This LED will indicate if the
circuit output is a logic `1` (LED is on) or logic `0` (LED is off).

```{warning}
To keep from destroying the LED due to a potential current spike, you
will wire the LED **in series** with a $1 kΩ$ resistor.
```

LEDs are directional components.
The *longer* of the LED's two leads should be on the *positive* side `+5 V`)
of the circuit (the side receiving the voltage or Pin 8 on the chip).
The other indicator of the LEDs polarity is the flat edge on the LED which
should be placed towards ground.

> Wire up the LED according to {numref}`led-wire`

```{figure} img/ice01_image11.jpg
---
name: led-wire
---
LED wiring
```

For your final test, simply turn on the power and test your switches.

If any of the switches are turned **on** the LED will light up. If all
of the switches are **off**, the LED should remain off.

Congratulations! You have successfully completed In Class Exercise (ICE) 1.
This will be directly applicable to your circuit building in lab 1!

> Demonstrate your operational circuit to your instructor.

## Exit Criteria

The instructor will check the circuit accurately implements a 4-input
OR gate function:

- When all switches are **off**, the output is `0`, LED is off.
- When any switch (A-D) is turned **on**, the output is `1`, LED will
light.

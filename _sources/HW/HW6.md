# HW 6 - Digital Schematics

The intent of this HW is to teach you how to draw and simulate digital logic schematics.

## Download Digital

1. Download [Digital](https://github.com/hneemann/Digital) from GitHub.
2. Unzip Digital and copy the entire unzipped it to someplace convenient.
3. You don't actually need to install Digital; you can just run the `Digital.exe` by double clicking on it.
4. You may be prompted to install Java. Please do so and then try running digital again.

## Half adder, again

Let's revisit the half adder!

1. Run Digital
2. It should open an empty file, but if it doesn't create a new file.
3. Now, create a half adder!
    - Components > IO > Input
    - Components > IO > Output
    - Components > Logic > AND
    - Components > LOGIC > XOR
4. Connect everything with wires. For example, click the red dot on the right side of one input, then click the blue dot on the left side of a logic gate. While dragging a new wire you can click on the grid to make it turn 90 degrees. You can also click on an existing wire to start a wire from that point.
A dot (solder joint) on two wires shows that they are connected at that dot!
5. Right click on each of the inputs and outputs and give them a Label (`i_A`, `i_B`, `o_S`, and `o_Cout`).
6. File > Save As and name your file **halfAdder.dig**

You can now simulate the circuit to see if it works as expected.

Simulation > Start of Simulation or the play button in the center tool bar.

Then click inputs to see if outputs behave as expected!

## Submission

Upload halfAdder.dig to the gradescope assignment.

```{important}
You **must** use the exact label names that we specified above,
otherwise the autograder will not work.

This is actually very realistic; engineers have to design interfaces
that work with external systems according to published standards!
```

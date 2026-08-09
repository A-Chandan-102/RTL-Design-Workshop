# Theory: Introduction to Yosys and Logic Synthesis

This session introduced **Yosys, RTL design, logic synthesis, standard cell libraries, and speed-area-power trade-offs** in digital circuits.

## Yosys

**Yosys** is an open-source RTL synthesis framework used to process Verilog RTL and convert it into a **gate-level netlist**.

It can optimize the RTL and map the required logic to cells available in a technology library.

### Important Yosys Commands

```text
read_verilog  → Reads the RTL design
read_liberty  → Reads the standard cell library
write_verilog → Writes the synthesized netlist
```

## RTL Design

**RTL (Register Transfer Level)** is a behavioral representation of the required hardware specification using an HDL such as Verilog.

RTL describes how data moves between registers and how the required logic operates.

## Logic Synthesis

Logic synthesis is the process of converting **RTL code into a gate-level representation** using the standard cells available in a technology library.

The resulting gate-level circuit is represented as a **netlist** containing gates and their connections.

## Standard Cell Library (.lib)

A `.lib` file is a collection of characterized standard cells used during synthesis.

It provides information such as cell functionality, timing, power, and area characteristics.

Different flavours of the same gate can be available:

- Slow
- Medium
- Fast

## Faster Cells vs Slower Cells

Digital circuits have capacitive loads that need to be charged and discharged.

```text
Wider Transistors → Higher Current → Lower Delay
                  → More Area + More Power

Narrower Transistors → Lower Current → Higher Delay
                     → Less Area + Less Power
```

Therefore, faster cells provide lower delay but generally require more **area and power**.

## Why Different Gate Flavours?

The combinational delay of a logic path determines the maximum operating speed of a digital circuit.

```text
Tclk > Tcq + Tcombi + Tsetup
```

Different cell flavours allow the synthesis tool to choose suitable cells depending on the timing requirements of the design.

## Simulation and Synthesis Verification

The synthesized netlist can be verified using the **same test bench** used for the RTL design because the primary inputs and outputs remain the same.

The waveform output should match the expected RTL behaviour if the synthesis is functionally correct.

## Learning Outcomes

- Understood the purpose and basic working of **Yosys**.
- Learned the concept of **RTL-to-gate-level synthesis**.
- Understood the role of **`.lib` standard cell libraries**.
- Learned about different cell flavours and their **speed-area-power trade-offs**.
- Understood how synthesis produces a **gate-level netlist**.
- Learned how to verify the synthesized netlist using the **same test bench**.

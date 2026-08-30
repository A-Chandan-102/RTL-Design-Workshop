# Class 2 – Mux, Synthesis and Netlist Basics

## 1. Good Mux vs Bad Mux

I learned how the sensitivity list in an `always` block affects simulation behaviour.

### Good Mux

```verilog
module good_mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*)
begin
    if (sel)
        y <= i1;
    else
        y <= i0;
end

endmodule
```

`always @(*)` automatically includes all signals used inside the block in the sensitivity list. Therefore, changes in `i0`, `i1`, or `sel` cause the block to execute and the output is updated correctly.

### Bad Mux

The bad mux used an incomplete sensitivity list:

```verilog
always @(sel)
begin
    if (sel)
        y <= i1;
    else
        y <= i0;
end
```

Here, the block responds only when `sel` changes. A change in `i0` or `i1` alone does not trigger the block.

This caused the simulated output to remain unchanged even when the selected input changed.

### Correction

I changed:

```verilog
always @(sel)
```

to:

```verilog
always @(*)
```

This corrected the simulation behaviour.

---

## 2. Icarus Verilog Simulation

I used Icarus Verilog to compile the RTL design together with its testbench.

```bash
iverilog good_mux.v tb_good_mux.v
```

The generated executable was then run:

```bash
./a.out
```

The simulation produced a VCD file that could be inspected using GTKWave.

```bash
gtkwave tb_good_mux.vcd
```

The waveform was used to verify the relationship between:

```text
i0
i1
sel
y
```

---

## 3. Synthesis Basics

After simulation, I introduced the basic synthesis flow using **Yosys**.

The RTL is converted into a logic representation and then into a technology-specific gate-level netlist using the selected standard-cell library.

Basic flow:

```text
Verilog RTL
     ↓
Yosys
     ↓
RTL representation
     ↓
Logic optimisation
     ↓
Technology mapping
     ↓
Gate-level netlist
```

---

## 4. Library and Synthesis

The standard-cell library was loaded before technology mapping.

```tcl
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The Verilog design was then read:

```tcl
read_verilog good_latch.v
```

The design was synthesised and mapped using Yosys commands such as:

```tcl
synth -top good_latch
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

I have chosen to synthesize the good_latch program and show its layout diagram.
The latch RTL was synthesised and its resulting structure was inspected.
The generated netlist showed that the RTL behaviour was represented using standard cells from the SKY130 library, including the latch structure and required logic for reset/control signals.

---

## 6. Netlist Inspection

Yosys was also used to generate a graphical representation of the synthesised design.

```tcl
show
```

This produced a Graphviz-based schematic showing:

- Input signals
- Standard cells
- Internal connections
- Output signals

The netlist view helped connect the original RTL description with the actual gate-level implementation.

---

## 7. Layout / GDS Overview

The synthesis and netlist flow was also related to the physical design stage.

```text
RTL
 ↓
Synthesis
 ↓
Gate-level Netlist
 ↓
Physical Design
 ↓
GDS
```

A GDS file represents the physical layout of the chip, including cells, routing and different physical layers.

---

# Class 3 – Functional Simulation of BabySoC

## 1. Functional Simulation

I performed functional simulation of the BabySoC design using Icarus Verilog.

The simulation was performed from the:

```bash
BabySoC_Simulation
```

directory.

---

## 2. Compilation

The testbench and required RTL files were compiled using:

```bash
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
```

The options used were:

```text
-o ./pre_synth_sim.out
    → specifies the output simulation executable

-DPRE_SYNTH_SIM
    → enables the PRE_SYNTH_SIM compilation mode

-I src/include
    → adds the include directory

-I src/module
    → adds the module directory
```

---

## 3. Running the Simulation

The generated simulation executable was run using:

```bash
./pre_synth_sim.out
```

The simulation generated:

```text
pre_synth_sim.vcd
```

---

## 4. Viewing the Waveform

The generated waveform was opened in GTKWave:

```bash
gtkwave pre_synth_sim.vcd
```

The waveform contained signals associated with the BabySoC simulation, including:

```text
CLK
reset
REF
OUT
VCO_IN
VCO_IN / VCO related signals
RV_TO_DAC[9:0]
```

The waveform was used to observe the functional behaviour of the system over time.

---

## 5. RV-to-DAC Activity

The `RV_TO_DAC[9:0]` signals were examined in GTKWave to observe the changing digital values being supplied toward the DAC interface.

The waveform showed multiple bits changing at different times, demonstrating the activity of the digital signals during functional simulation.

---

# Key Takeaways

- I learned the importance of a correct Verilog sensitivity list.
- I understood why `always @(*)` is preferred for combinational RTL.
- I learned the basic Icarus Verilog → VCD → GTKWave simulation flow.
- I learned the basic Yosys synthesis flow and technology mapping.
- I inspected synthesised gate-level netlists using `show`.
- I connected RTL, standard-cell netlists and physical GDS representation.
- I performed functional simulation of the BabySoC design and inspected its waveforms.

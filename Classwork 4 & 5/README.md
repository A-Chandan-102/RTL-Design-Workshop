# Class 4 & 5 – Pre-Synthesis, Synthesis and Gate-Level Simulation (GLS)

## 1. Good Mux: Pre-Synthesis vs Post-Synthesis

I compared the behaviour of the RTL design before synthesis with its gate-level implementation after synthesis.

### Step 1 – Functional Simulation

```bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

The RTL was simulated first to verify the expected functional behaviour of the `good_mux`.

```text
i0 ─┐
    ├── MUX ──> y
i1 ─┘
     ↑
    sel
```

The GTKWave waveform showed that `y` follows the selected input correctly.

---

## 2. Synthesis using Yosys

I then synthesised the RTL and converted it into a technology-mapped gate-level design.

```bash
yosys
```

### Read the SKY130 Liberty File

```tcl
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Read the RTL

```tcl
read_verilog good_mux.v
```

### Synthesis

```tcl
synth -top good_mux
```

### Technology Mapping

```tcl
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

### View the Synthesised Schematic

```tcl
show
```

The synthesised design was reduced to the required standard cell implementation. For `good_mux`, the resulting structure contains a SKY130 2-input multiplexer cell.

### Export Gate-Level Netlist

```tcl
write_verilog -noattr good_mux_netlist.v
```

---

## 3. Gate-Level Simulation (GLS)

The generated gate-level netlist was simulated using the SKY130 primitive and cell models.

```bash
iverilog ../my_lib/verilog_model/primitives.v \
../my_lib/verilog_model/sky130_fd_sc_hd.v \
good_mux_netlist.v \
tb_good_mux.v
```

Run the simulation:

```bash
./a.out
```

View the waveform:

```bash
gtkwave tb_good_mux.vcd
```

The GLS waveform was compared with the original RTL waveform. The functional behaviour remained consistent, confirming that the synthesised implementation preserved the intended logic.

---

## 4. Good Mux Synthesis Results

The Yosys synthesis output showed:

```text
Number of cells: 1
$__MUX_ : 1
```

After technology mapping, the design corresponded to a SKY130 multiplexer cell.

This demonstrates the flow:

```text
RTL
 ↓
Functional Simulation
 ↓
Yosys Synthesis
 ↓
Technology Mapping
 ↓
Gate-Level Netlist
 ↓
Gate-Level Simulation
```

The pre-synthesis and post-synthesis waveforms can therefore be compared to verify functional equivalence.

---

# 5. BabySoC – Pre-Synthesis and Post-Synthesis Flow

I also applied the synthesis and simulation flow to the BabySoC design.

## Pre-Synthesis Functional Simulation

```bash
cd BabySoC_Simulation

iverilog -o ./pre_synth_sim.out \
-DPRE_SYNTH_SIM \
src/module/testbench.v \
-I src/include \
-I src/module/

./pre_synth_sim.out

gtkwave pre_synth_sim.vcd
```

The pre-synthesis simulation was used as the reference functional behaviour of the BabySoC.

The waveform included signals such as:

```text
CLK
reset
ENb_CP
ENb_VCO
OUT
REF
VCO_IN
VREFH
VREFL
RV_TO_DAC[9:0]
```

---

## BabySoC Synthesis

I loaded the BabySoC RTL and its associated modules into Yosys.

```tcl
read_verilog src/module/vsdbabysoc.v
read_verilog -I src/include/src/module/rvmyth.v
read_verilog -I src/include/src/module/clk_gate.v
```

The required libraries were loaded:

```tcl
read_liberty -lib src/lib/avsdpll.lib
read_liberty -lib src/lib/avsddac.lib
read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The top-level module was synthesised:

```tcl
synth -top vsdbabysoc
```

### Flip-Flop Mapping

```tcl
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Technology Mapping

```tcl
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The resulting hierarchy and cell statistics were inspected to understand the synthesised BabySoC implementation.

---

## 6. BabySoC Gate-Level Simulation

After synthesis, the post-synthesis design was simulated using the gate-level models.

```bash
cd BabySoC_Simulation

iverilog \
-DFUNCTIONAL \
-DUNIT_DELAY=#1 \
-DPOST_SYNTH_SIM \
-o ./post_synth_sim.out \
./src/module/testbench.v \
-I ./src/include \
-I ./src/module \
-I ./src/gls_model \
-I ./src/
```

Run the simulation:

```bash
./post_synth_sim.out
```

View the waveform:

```bash
gtkwave post_synth_sim.vcd
```

The post-synthesis waveform was compared with the pre-synthesis waveform to check whether the synthesised design preserved the expected functional behaviour.

---

## 7. BabySoC Netlist and Layout

The synthesis process produced a large gate-level representation containing standard cells and the major BabySoC blocks.

The physical implementation was also inspected through the generated layout/GDS representation.

---

# Key Takeaways

- I learned to compare RTL simulation with gate-level simulation.
- I understood the complete Yosys synthesis and technology-mapping flow.
- I generated and inspected a gate-level netlist.
- I used SKY130 standard-cell libraries during synthesis.
- I verified `good_mux` behaviour before and after synthesis.
- I performed pre-synthesis and post-synthesis simulation of BabySoC.
- I learned how GLS can be used to verify that synthesis has preserved the intended functionality.
- I inspected synthesised netlists and the corresponding physical layout representation.

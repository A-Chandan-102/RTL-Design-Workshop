# Assessment 1 – Sequence Detector

## Objective

The given **sequence detector RTL code** was analysed and verified through simulation and synthesis. The assessment involved understanding the RTL code, answering questions based on it, performing RTL simulation, synthesis, Gate-Level Simulation (GLS), and inspecting the generated netlist and layout.

---

## 1. RTL Code Analysis

The provided sequence detector was first studied to understand:

- Inputs and outputs
- State registers and state transitions
- `always` blocks
- Reset behaviour
- Next-state logic
- Sequence detection logic
- Number of states and required flip-flops

The state and transition behaviour was also verified from the simulation waveform.

---

## 2. RTL Simulation

The original RTL design was simulated to verify its functional behaviour.

The simulation waveform was observed in **GTKWave**, showing signals such as:

The waveform was checked to confirm that the state transitions and sequence detection followed the RTL specification.

---

## 3. Synthesis using Yosys

The sequence detector was synthesised using **Yosys** and the SKY130 standard-cell library.

### Basic Synthesis Flow

```tcl
read_liberty -lib ../../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog <sequence_detector_file>.v
synth -top sequence_detector
abc -liberty ../../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The synthesised design was inspected using:

```tcl
show
```

The schematic showed the logic gates, multiplexing logic and flip-flops used to implement the sequence detector.

---

## 4. Synthesis Statistics

The Yosys statistics were examined to determine the hardware resources used by the design.

The synthesised sequence detector contained:

- Flip-flops for storing the FSM state
- Combinational logic for next-state generation
- Logic gates for sequence detection
- Reset-related logic

The final synthesis statistics were checked using:

```tcl
stat
```

The synthesis completed successfully with:

```text
Found and reported 0 problems.
```

---

## 5. Gate-Level Netlist

The synthesised design was exported as a gate-level Verilog netlist.

```tcl
write_verilog -noattr sequence_detector_netlist.v
```

The generated netlist was inspected to understand how the RTL FSM was converted into actual standard-cell level hardware.

The netlist included SKY130 cells such as:

```text
D Flip-Flops
AND gates
NOR gates
OR gates
Clock inverter
Other mapped logic cells
```

---

## 6. Gate-Level Simulation (GLS)

The generated gate-level netlist was simulated using the SKY130 Verilog cell models.

```bash
iverilog \
../my_lib/verilog_model/primitives.v \
../my_lib/verilog_model/sky130_fd_sc_hd.v \
sequence_detector_netlist.v \
tb_sequence_detector.v
```

The simulation was then executed:

```bash
./a.out
```

The resulting waveform was opened using:

```bash
gtkwave <generated_vcd_file>.vcd
```

The GLS waveform was compared with the original RTL simulation to verify that the synthesised implementation preserved the required functionality.

---

## 7. Netlist and Synthesis Results

The synthesised sequence detector was inspected through:

```text
Yosys synthesis
      ↓
Technology mapping
      ↓
Gate-level netlist
      ↓
GTKWave GLS
```

The generated schematic showed the actual hardware implementation, including the state flip-flops and combinational logic used for state transitions and detection.

---

## 8. Assessment Learning Outcomes

- I analysed an FSM-based sequence detector from RTL code.
- I understood the relationship between state, next-state and output logic.
- I performed RTL functional simulation using Icarus Verilog and GTKWave.
- I synthesised the RTL using Yosys with the SKY130 library.
- I inspected synthesis statistics and the generated gate-level schematic.
- I generated and examined the gate-level Verilog netlist.
- I performed Gate-Level Simulation and compared it with RTL behaviour.
- I gained practical understanding of the RTL-to-netlist verification flow.

---

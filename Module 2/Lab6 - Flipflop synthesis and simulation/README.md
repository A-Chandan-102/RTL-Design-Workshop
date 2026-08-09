# Lab 4 – Flip-Flop Synthesis and Simulation

## Objective

To simulate different flip-flop RTL designs using **Icarus Verilog** and **GTKWave**, and then synthesize them using **Yosys** to observe how different flip-flop coding styles are mapped to SKY130 standard cells.

---

## Flip-Flop Designs Used

The lab used the following Verilog designs:

- `dff_async.v` – Asynchronous reset flip-flop
- `dff_async_set.v` – Asynchronous set flip-flop
- `dff_syncres.v` – Synchronous reset flip-flop
- `dff_asyncres.v` – Flip-flop with asynchronous reset

Corresponding testbench files were used for simulation.

---

## 1. RTL Simulation Using Icarus Verilog

Each flip-flop design was first simulated using **Icarus Verilog** along with its corresponding testbench.

The basic simulation procedure was:

```bash
iverilog -o <output>.out <design>.v <testbench>.v
vvp <output>.out
```

The waveform was then viewed using **GTKWave**:

```bash
gtkwave <waveform>.vcd
```

The waveforms were checked for:

- Clock (`clk`)
- Data (`d`)
- Reset/set signals
- Output (`q`)

This verified the functional behavior of the RTL designs before synthesis.

---

## 2. Files Used

The main RTL files used were:

```text
dff_async.v
dff_async_set.v
dff_syncres.v
dff_asyncres.v
```

The corresponding testbench files were also used
The simulations generated corresponding `.vcd` waveform files.

---

## 3. Synthesis Using Yosys

After simulation, the RTL designs were synthesized using **Yosys**.

### Loading the SKY130 Library

First, the SKY130 standard-cell Liberty library was loaded:

```text
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The Liberty file provides Yosys with information about the available SKY130 standard cells.

### Reading the RTL

The Verilog design was then read:

```text
read_verilog dff_async.v
```

### Synthesizing the Design

The required top-level module was selected using:

```text
synth -top dff_async
```

The same process was repeated for the other flip-flop designs.

---

## 4. Viewing the Synthesized Netlist

The synthesized circuit was viewed using:

```text
show
```

Yosys generated a Graphviz `.dot` representation of the synthesized design and opened it in the Dot Viewer.

The `.dot` representation allowed the actual SKY130 standard cells and their connections to be observed.

---

## 5. Asynchronous Reset Flip-Flop

The asynchronous reset RTL was mapped to a flip-flop with a dedicated reset input.

The important observation is that `async_reset` is connected directly to the reset control of the flip-flop.

Therefore, the reset operation is independent of the clock.

The synthesized netlist observed a SKY130 flip-flop cell with a `RESET_B` control pin.

---

## 6. Asynchronous Set Flip-Flop

The asynchronous set design was mapped to a standard-cell flip-flop with a dedicated set input.

The asynchronous set signal is connected to the dedicated set input of the standard-cell flip-flop.

The observed synthesized cell contained a `SET_B` pin.

---

## 7. Synchronous Reset Flip-Flop

The synchronous reset design produced a different synthesized structure.

Instead of connecting the reset directly to a dedicated reset pin of the flip-flop, synthesis created combinational logic before the D input.

---

## 8. Asynchronous Reset Netlist

The `dff_asyncres` design was also synthesized and its generated `.dot` netlist was inspected.

The synthesized circuit showed the asynchronous reset signal being connected to the reset control of the SKY130 flip-flop.


## 9. Comparison of Synthesized Netlists

| RTL Design | Synthesized Implementation |
|------------|----------------------------|
| Asynchronous reset | DFF with dedicated reset pin |
| Asynchronous set | DFF with dedicated set pin |
| Synchronous reset | Combinational logic + DFF |
| Asynchronous reset with additional logic | Logic followed by DFF reset control |

The `.dot` netlists showed that different RTL descriptions can result in different hardware structures after synthesis.

---

## Learning Outcomes

After completing this lab, I was able to:

- Simulate different flip-flop RTL designs using Icarus Verilog.
- Verify RTL behavior using GTKWave.
- Load the SKY130 standard-cell library into Yosys.
- Synthesize RTL designs using `synth -top`.
- Generate and inspect synthesized netlists using `show`.
- Observe how asynchronous reset is mapped to a dedicated flip-flop reset pin.
- Observe how asynchronous set is mapped to a dedicated flip-flop set pin.
- Compare the synthesized structures of synchronous and asynchronous reset designs.
- Understand how Yosys maps RTL flip-flop descriptions to SKY130 standard cells.
- Relate the RTL coding style to the resulting synthesized hardware structure.

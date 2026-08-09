# Lab 3: Invoke Yosys and Synthesize the Design

In this lab, I learned how to invoke **Yosys**, read the RTL design and standard cell library, synthesize the `good_mux` design, and generate the corresponding gate-level netlist.

## Design Used

- `good_mux.v` – RTL implementation of a 2:1 multiplexer.
- Technology library – **Sky130 standard cell `.lib` file**.

## Invoking Yosys

Yosys was launched from the terminal using:

```bash
yosys
```

## Yosys Synthesis Commands

The commands were executed in the following order:

### 1. Read the Standard Cell Library

```text
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The `.lib` file provides Yosys with the available Sky130 standard cells and their characteristics.

### 2. Read the RTL Design

```text
read_verilog good_mux.v
```

This reads the Verilog RTL description of the multiplexer into Yosys.

### 3. Synthesize the Design

```text
synth -top good_mux
```

This performs RTL synthesis and identifies `good_mux` as the top-level module.

### 4. Map the Design to Standard Cells

```text
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

ABC performs logic optimization and technology mapping using the specified Sky130 standard cell library.

### 5. View the Synthesized Design

```text
show
```

The `show` command generates a graphical representation of the synthesized circuit.

### 6. Generate the Gate-Level Netlist

```text
write_verilog good_mux_netlist.v
```

This writes the synthesized gate-level design into `good_mux_netlist.v`.

## Synthesized Netlist

The original RTL:

```verilog
if(sel)
    y <= i1;
else
    y <= i0;
```

is converted by Yosys into a gate-level implementation using Sky130 standard cells.

For the `good_mux` design, the synthesis result uses a Sky130 multiplexer cell:

```text
sky130_fd_sc_hd__mux2_1
```

The synthesized netlist therefore represents the original RTL functionality using actual technology-specific standard cells.

## Learning Outcomes

- Learned how to invoke **Yosys** from the terminal.
- Learned to read a **Verilog RTL file** and a **Sky130 `.lib` file**.
- Performed RTL synthesis using `synth -top`.
- Used **ABC** for technology mapping to Sky130 standard cells.
- Generated and viewed the synthesized gate-level netlist.
- Understood how a simple RTL multiplexer is converted into a technology-specific standard cell implementation.

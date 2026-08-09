# Lab 2: Working with Icarus Verilog and GTKWave

In this lab, I learned how to compile and simulate a Verilog design using **Icarus Verilog** and analyze the simulation results using **GTKWave**.

## Files Used

- `good_mux.v` – Verilog RTL design of a multiplexer.
- `tb_good_mux.v` – Test bench used to provide inputs and verify the multiplexer.

## Simulation using Icarus Verilog

The design and test bench were compiled using Icarus Verilog:

```bash
iverilog good_mux.v tb_good_mux.v
```

The generated simulation was then executed:

```bash
./a.out
```

A **VCD (Value Change Dump)** file was generated during simulation to store the signal transitions.

## Viewing Waveforms using GTKWave

The generated VCD file was opened in GTKWave:

```bash
gtkwave tb_good_mux.vcd
```

Inside GTKWave, the required signals were selected from the **left-side signal panel** and dragged into the **black waveform window**.

This allowed me to observe the input and output signal transitions and verify the behavior of the multiplexer.

## Learning Outcomes

- Learned how to compile and run Verilog designs using **Icarus Verilog**.
- Learned how a **test bench** is used to verify an RTL design.
- Learned how to generate and use **VCD files**.
- Learned to view and analyze RTL waveforms using **GTKWave**.
- Verified the behavior of a multiplexer through waveform analysis.

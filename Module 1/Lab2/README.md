# Lab 2: Working with Icarus Verilog, GTKWave and Gvim

In this lab, I learned how to compile and simulate a Verilog design using **Icarus Verilog** and analyze the simulation results using **GTKWave**


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

## Exploring Verilog Files using GVim

Used **GVim** to open and examine the Verilog source files and understand their structure.

### Understanding the Code

### `good_mux.v`

```verilog
module good_mux (input i0, input i1, input sel, output reg y);

always @(*) 
begin
    if(sel) 
        y <= i1;
    else 
        y <= i0;
end

endmodule
```

- `module` defines the hardware design.
- `i0` and `i1` are the two data inputs.
- `sel` is the select input.
- `y` is the output.
- `always @(*)` describes combinational logic.
- When `sel = 1`, `i1` is selected; otherwise, `i0` is selected.

### `tb_good_mux.v`

The test bench instantiates the `good_mux` design and provides different input combinations.

```verilog
good_mux uut (
    .sel(sel),
    .i0(i0),
    .i1(i1),
    .y(y)
);
```

The test bench also generates signal changes and records them in a VCD file:

```verilog
$dumpfile("tb_good_mux.vcd");
$dumpvars(0, tb_good_mux);
```

The inputs are changed at different time intervals using:

```verilog
always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
```

This creates different input conditions that can later be observed in GTKWave.


## Learning Outcomes

- Learned how to compile and run Verilog designs using **Icarus Verilog**.
- Learned how a **test bench** is used to verify an RTL design.
- Learned how to generate and use **VCD files**.
- Learned to view and analyze RTL waveforms using **GTKWave**.
- Verified the behavior of a multiplexer through waveform analysis.
- Learned to explore and understand Verilog files using **GVim**.
- Understood the basic structure of a Verilog **module and test bench**.
- Learned how a **2:1 multiplexer** is described using RTL.

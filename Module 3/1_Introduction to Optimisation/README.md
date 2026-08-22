## Combinational and Sequential Logic Optimisation
- This class covered optimisation techniques used during RTL synthesis to reduce unnecessary logic and improve **area, power, and overall circuit efficiency**.
## 1. Combinational Logic Optimisation

### Constant Propagation Optimisation
For example:

```verilog
assign y = a ? 1'b0 : b;
```

If `a = 1`, the output is always:

```text
y = 0
```
- Identifies signals with constant values (`0` or `1`).
- Propagates these constants through the logic to simplify the circuit.
- Removes unnecessary logic and reduces circuit complexity.
- Helps reduce **area and power consumption**.

### Boolean Logic Optimisation
For example:

```verilog
assign y = a ? (b ? c : (c ? a : 1'b0)) : (!c);
```
- Simplifies Boolean expressions using Boolean algebra.
- Eliminates redundant logic while maintaining the same functionality.
- Reduces the number of gates and logic operations required.

---

## 2. Sequential Logic Optimisation
For example:

```verilog
always @(posedge clk)
    q <= 1'b0;
```
### Sequential Constant Propagation *(Main Focus)*

- Propagates constant values through sequential elements such as flip-flops.
- Detects registers whose values are fixed or predictable.
- Removes unnecessary sequential logic and simplifies the circuit.
- Can reduce **cell count, area, and power consumption**.

### State Optimisation

- Reduces the number of states required in an FSM.
- Equivalent or redundant states can be merged without changing functionality.

### Cloning

- Creates additional copies of logic or sequential elements.
- Can reduce fanout and improve timing and physical implementation.

### Retiming

- Moves flip-flops across combinational logic while maintaining functionality.
- Used mainly to improve timing and balance pipeline stages.

---

## Learning Outcomes

- Understand **combinational and sequential logic optimisation**.
- Apply **constant propagation and Boolean optimisation**.
- Understand basic **sequential optimisation techniques**.
- Learn how optimisation reduces **area, power, and logic complexity**.

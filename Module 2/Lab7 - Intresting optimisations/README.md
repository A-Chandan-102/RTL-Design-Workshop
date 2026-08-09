# Lab 7 – Optimization in Synthesis

## Objective

To understand how **Yosys optimizes RTL during synthesis** by simplifying constant arithmetic operations and observing the resulting optimized netlist.

---

## RTL Files Used

The following RTL files were used:

```text
mult_2.v
mult_8.v
```

These designs implement multiplication by constant values.

- `mult_2.v` → multiplication by 2
- `mult_8.v` → multiplication by 8

---

## Optimization Concept

Multiplication by a constant does not always require a dedicated multiplier circuit.

For example:

```text
a × 2
```

can be implemented as a **1-bit left shift**:

```text
a × 2 = a << 1
```

Similarly:

```text
a × 8 = a << 3
```

Therefore, Yosys can optimize these operations into simple **bit connections/wiring** instead of generating multiplier cells.

---

## Synthesis of `mult_2.v`

The `mult_2.v` RTL was read into Yosys and synthesized.

The resulting design was essentially:

```text
a[2:0] ──────> 2 × ──────> y[3:0]
```

Since multiplication by 2 is equivalent to shifting the input left by one bit, the operation can be represented as:

```text
a[2:0] × 2 = {a[2:0], 1'b0}
```

Thus, no actual multiplier hardware is required.

The synthesized representation showed the direct connection between the input and output bits.

---

## Synthesis of `mult_8.v`

The `mult_8.v` design was also synthesized.

Multiplication by 8 is equivalent to shifting left by three bits:

```text
a × 8 = a << 3
```

Therefore, the operation can be implemented using direct bit connections rather than multiplier logic.

The synthesized design was displayed using Yosys' graphical representation.

---

## Yosys Synthesis Flow

The RTL files were first read into Yosys:

```text
read_verilog mult_2.v
```

or:

```text
read_verilog mult_8.v
```

The design was then synthesized:

```text
synth -top mul2
```

or:

```text
synth -top mul8
```

After synthesis, the design statistics were checked.

---

## Cell Inference

The synthesis results showed:

```text
Number of cells: 0
```

This is an important result.

No standard-cell logic was inferred because Yosys optimized the constant multiplication into simple wiring/bit manipulation.

Therefore, there was no actual multiplier cell or combinational logic that required technology mapping.

---

## Why ABC Was Not Required

Normally, the synthesis flow can use:

```text
abc -liberty <library_file>
```

to perform technology mapping and optimize combinational logic using the target standard-cell library.

However, in this case:

```text
Number of cells = 0
```

because the constant multiplication was already reduced to direct connections.

Therefore, there was no meaningful combinational cell network for ABC to map.

The optimization was effectively completed during the RTL synthesis stage itself.

---

## Viewing the Optimized Netlist

The optimized design was viewed using:

```text
show
```

Yosys generated a Graphviz representation of the synthesized design.

The `.dot` representation was then inspected using the Dot Viewer.

The generated netlist confirmed that the constant multiplication had been simplified instead of being implemented using a multiplier circuit.

---

## Netlist Generation

The optimized RTL netlist was also written to a Verilog file using:

```text
write_verilog -noattr mul2_net.v
```

The generated netlist was then opened to cross-check the optimized implementation.

This confirmed that the synthesized design consisted mainly of direct assignments/bit connections.

---

## Optimization Observation

```text
RTL
 |
 |  Constant multiplication
 v
Yosys Optimization
 |
 |  ×2 → shift left by 1
 |  ×8 → shift left by 3
 v
Direct Bit Connections
 |
 v
0 Synthesized Cells
```

The main observation is that **RTL coding does not necessarily translate directly into equivalent hardware blocks**.

Yosys analyzes the functionality and removes unnecessary hardware wherever possible.

---

## Key Result

| Design | Operation | Optimized Implementation | Cells Inferred |
|--------|-----------|--------------------------|----------------|
| `mult_2.v` | `a × 2` | Left shift / bit connection | 0 |
| `mult_8.v` | `a × 8` | Left shift / bit connection | 0 |

---

## Conclusion

The lab demonstrated synthesis optimization of constant multiplication.

Yosys recognized that multiplication by powers of two can be implemented using simple bit shifting and optimized the RTL accordingly. Since the optimized designs contained no actual logic cells, the number of inferred cells was **zero**, making further ABC technology mapping unnecessary.

The generated `.dot` files and synthesized Verilog netlists were inspected to verify the optimized hardware structure.

---

## Learning Outcomes

After completing this lab, I was able to:

- Understand optimization performed by Yosys during synthesis.
- Understand how constant multiplication can be simplified.
- Recognize that multiplication by 2 and 8 can be implemented using bit shifts.
- Observe how synthesis can reduce arithmetic operations to simple wiring.
- Interpret Yosys synthesis statistics.
- Understand why zero cells were inferred for these optimized designs.
- Understand why ABC technology mapping was not required.
- Generate and inspect synthesized `.dot` netlists.
- Cross-check the optimized Verilog netlist generated by Yosys.
- Understand how RTL optimization can significantly reduce hardware complexity.

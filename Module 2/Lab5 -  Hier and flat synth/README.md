# Lab 5 – Hierarchical vs Flat Synthesis

## Objective

To understand the difference between **hierarchical synthesis** and **flat synthesis** in Yosys, and to observe how the generated netlist changes when the design hierarchy is preserved or removed.

The example used is `multiple_modules.v`, which contains a top module `multiple_modules` and two submodules, `sub_module1` and `sub_module2`.

---

## 1. Design Used

The RTL design contains two submodules connected together:

```verilog
module sub_module1(input a, input b, output y);
    assign y = a & b;
endmodule

module sub_module2(input a, input b, output y);
    assign y = a | b;
endmodule

module multiple_modules(
    input a,
    input b,
    input c,
    output y
);

    wire net1;

    sub_module1 u1 (
        .a(a),
        .b(b),
        .y(net1)
    );

    sub_module2 u2 (
        .a(net1),
        .b(c),
        .y(y)
    );

endmodule
```

The design is hierarchical because `multiple_modules` contains instances of `sub_module1` and `sub_module2`.

---

#2. Reading the Design into Yosys

Start Yosys:

```bash
yosys
```

Read the standard-cell library:

```yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Read the RTL:

```yosys
read_verilog multiple_modules.v
```

The `read_verilog` command parses the RTL and creates Yosys's internal RTL representation of all the modules.

Check the design:

# 3. Hierarchical Synthesis

First synthesize the complete top-level module without flattening the hierarchy:

This tells Yosys that `multiple_modules` is the top-level design.

The hierarchy is retained, so the synthesized design still contains:

```text
multiple_modules
├── u1 → sub_module1
└── u2 → sub_module2
```

The synthesis statistics show the individual modules and the complete design hierarchy.

---

# 4. Viewing the Hierarchical Netlist

Display the synthesized design:

```yosys
show multiple_modules
```

The generated netlist maintains the relationship between the top module and its submodules.

Conceptually, it appears as:

Each submodule can be treated as an independent block.

---

# 5. Why Hierarchical Synthesis Is Useful

Hierarchical synthesis is useful when the design contains:

- Multiple reusable modules
- Large blocks
- Repeated instances
- IP blocks
- Complex system-level designs

The hierarchy makes the design easier to understand, debug, modify and manage.

For example:

```text
multiple_modules
        │
        ├── sub_module1
        │
        └── sub_module2
```

Each block can be analyzed separately instead of dealing with one large flat circuit.

---

# 6. Flat Synthesis

In flat synthesis, the hierarchy is removed and the logic inside the submodules is brought into the top-level module.

Use:

```yosys
synth -top multiple_modules -flatten
```
It tells Yosys to flatten the design hierarchy during synthesis.

The resulting design no longer needs:

```text
sub_module1
sub_module2
u1
u2
```

as separate logical blocks.

Instead, the gates inside those modules become part of the top-level module.

# 7. Hierarchical Netlist vs Flat Netlist

## Hierarchical Netlist

With:

```yosys
synth -top multiple_modules
```

the design retains its hierarchy:

```text
multiple_modules
│
├── u1
│    └── sub_module1
│
└── u2
     └── sub_module2
```

The top-level module contains instances of the submodules.

---

#8 Flat Netlist

With:

```yosys
synth -top multiple_modules -flatten
```

the hierarchy is removed:

```text
multiple_modules
│
├── AND cell
│
└── OR cell
```

The internal logic of `u1` and `u2` is directly present inside the top-level netlist.

---

# 9. Why Does the Netlist Look Different?

The two netlists represent the same logical function:

```text
y = (a & b) | c
```

However, they organize the logic differently.

### Hierarchical

```text
multiple_modules
      │
      ├── u1 → AND
      │
      └── u2 → OR
```

### Flat

```text
multiple_modules
      │
      ├── AND
      │
      └── OR
```

In the hierarchical version, Yosys preserves the module boundaries.

In the flat version, the boundaries are removed and the internal cells are directly connected in the top-level module.

---

flattening can provide greater optimization freedom.

However, the resulting netlist can become much larger and harder to debug.

Hierarchical Synthesis is particularly useful for:

- Large SoCs
- Reusable IP blocks
- Repeated modules
- Block-level design
- Hierarchical physical design

---

Both approaches implement the same functionality, but the **structure of the synthesized netlist is different**.

---

# 10. Conclusion

In this lab, hierarchical and flat synthesis were studied using Yosys.

The command:

```yosys
synth -top multiple_modules
```

synthesizes the design while retaining its module hierarchy.

The command:

```yosys
synth -top multiple_modules -flatten
```

removes the hierarchy and produces a flat representation of the design.

The hierarchical approach is useful for **design organization, reuse and debugging**, while flat synthesis provides the synthesis tool with greater visibility for **cross-module optimization**.

The experiment also demonstrated how RTL operators are converted into **technology-specific SKY130 standard cells** during synthesis.

# `if` Statement and Inferred Latch

## Objective

To observe the effect of an **incomplete `if` statement** during RTL simulation and synthesis, and understand how Yosys infers a latch when an output is not assigned for every possible condition.

---

## File Used

```text
incomp_if.v
```

The design uses an `if` statement where the output does not receive a value in every possible condition.

---

## 1. RTL Simulation

The design was first simulated using Icarus Verilog and the waveform was viewed using GTKWave.

The waveform shows the inputs changing over time and the output `y` retaining its previous value when the `if` condition is not satisfied.

This behaviour is important because an output that must **remember its previous value** indicates storage behaviour rather than purely combinational logic.

---

## 2. Why Does a Latch Get Inferred?

Consider an incomplete combinational `if`:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
end
```

When `sel = 0`, there is no assignment to `y`.

Therefore, `y` must retain its previous value:

```text
sel = 1  → y = i1
sel = 0  → y keeps previous value
```

To implement this behaviour in hardware, synthesis requires a storage element.

Hence:

```text
Incomplete if
      ↓
Output retains previous value
      ↓
Latch inference
```

---

## 3. Synthesis

The RTL design was synthesized using Yosys and the generated netlist was inspected using the `show` command.

The synthesized design contains a latch cell:

```text
$_DLATCH_*
```

This confirms that Yosys interpreted the incomplete `if` statement as **level-sensitive storage**.

The synthesized structure contains:

- Multiplexer logic for selecting the required input.
- Logic controlling the latch enable.
- A latch that stores the output when the condition is not active.

---

## 4. Synthesized Netlist Observation

The synthesized schematic can be viewed as:

```text
Inputs
  ↓
Combinational Logic
  ↓
Latch Enable / Data
  ↓
Latch
  ↓
y
```

The presence of the latch is a direct result of the incomplete assignment in the RTL.

---

## 5. Waveform Observation

The GTKWave waveform shows that `y` does not necessarily change whenever the inputs change.

Instead, when the `if` condition is inactive, the output retains its previous value.

```text
Input condition active
        ↓
      y updates

Input condition inactive
        ↓
      y holds value
```

This behaviour matches the operation of a level-sensitive latch.

---

## 6. Avoiding Unintended Latches

For combinational logic, assign the output in every possible condition.

### Incomplete

```verilog
always @(*)
begin
    if (sel)
        y = i1;
end
```

### Complete

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

The `else` ensures that `y` always receives a value.

```text
Complete if/else
       ↓
All conditions covered
       ↓
Combinational logic
       ↓
No unintended latch
```

---

## Key Takeaways

- An incomplete `if` statement can cause **latch inference** during synthesis.
- A latch is inferred when an output needs to **retain its previous value**.
- RTL simulation can show the output holding its previous value.
- Yosys exposes the inferred latch in the synthesized netlist.
- For combinational logic, every output should be assigned for **all possible conditions**.
- Using a complete `if-else` structure prevents unintended latch inference.

---

## Learning Outcomes

- Understood how incomplete `if` statements can infer latches.
- Observed latch behaviour using GTKWave.
- Identified an inferred latch in the synthesized Yosys netlist.
- Learned how to write complete combinational `if-else` logic.
- Understood the difference between combinational logic and storage behaviour.

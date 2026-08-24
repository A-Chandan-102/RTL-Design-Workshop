# `if` and `case` Constructs in Verilog

## 1. `if` Construct

The `if` construct is used to implement **conditional logic** in Verilog.

### Basic Syntax

```verilog
always @(*)
begin
    if (condition)
        statement;
    else
        statement;
end
```

The `if` condition has priority over the `else` condition.

### `if - else if - else` Syntax

Multiple conditions can be checked using `else if`.

```verilog
always @(*)
begin
    if (cond1)
        y = c1;
    else if (cond2)
        y = c2;
    else if (cond3)
        y = c3;
    else
        y = c4;
end
```

The first true condition determines the output.

---

## 2. Danger of Incomplete `if` Statements

When an output is not assigned for every possible condition, the synthesis tool may infer a **latch**.

For example:

```verilog
always @(*)
begin
    if (cond1)
        y = a;
    else if (cond2)
        y = b;
end
```

There is no assignment when:

```text
cond1 = 0
cond2 = 0
```

Therefore, `y` must retain its previous value.

This behaviour requires storage, so synthesis can infer a latch.

```text
Incomplete if
     ↓
Output must retain previous value
     ↓
Latch inferred
```

### Avoiding the Inferred Latch

Provide an `else` condition:

```verilog
always @(*)
begin
    if (cond1)
        y = a;
    else if (cond2)
        y = b;
    else
        y = c;
end
```

Now `y` receives a value for every possible input condition.

---

## 3. `if` as Priority Logic

An `if - else if` structure represents **priority logic**.

For example:

```verilog
always @(*)
begin
    if (cond1)
        y = c1;
    else if (cond2)
        y = c2;
    else if (cond3)
        y = c3;
    else
        y = c4;
end
```

If both `cond1` and `cond2` are true, `cond1` gets priority.

Therefore:

```text
cond1 > cond2 > cond3 > else
```

The synthesized hardware can therefore contain cascaded multiplexers implementing this priority.

---

# 4. `case` Statement

The `case` statement is useful when one variable is compared against multiple possible values.

### Basic Syntax

```verilog
always @(*)
begin
    case (sel)
        2'b00: y = c1;
        2'b01: y = c2;
        2'b10: y = c3;
        2'b11: y = c4;
    endcase
end
```

Here, `sel` selects one of the four possible inputs.

Conceptually:

```text
        c1 ──┐
        c2 ──┤
        c3 ──┤──> MUX ──> y
        c4 ──┘
              ↑
             sel
```

---

## 5. `case` with `default`

A `default` branch can be used to specify what happens when none of the listed cases match.

```verilog
always @(*)
begin
    case (sel)
        2'b00: y = c1;
        2'b01: y = c2;
        2'b10: y = c3;
        default: y = c4;
    endcase
end
```

Using `default` ensures that the output receives a value for all remaining input combinations.

This helps avoid unintended latch inference when the case items are incomplete.

---

## 6. Danger of Incomplete `case`

Consider:

```verilog
always @(*)
begin
    case (sel)
        2'b00: y = c1;
        2'b01: y = c2;
    endcase
end
```

For:

```text
sel = 2'b10
sel = 2'b11
```

there is no assignment to `y`.

The output must therefore retain its previous value, which can cause a latch to be inferred.

```text
Incomplete case
      ↓
Missing output assignment
      ↓
Output must hold previous value
      ↓
Latch inference
```

### Safer Version

```verilog
always @(*)
begin
    case (sel)
        2'b00: y = c1;
        2'b01: y = c2;
        default: y = c3;
    endcase
end
```

---

# 7. Partial Assignment in `case`

A case statement can also contain multiple outputs.

For example:

```verilog
reg [1:0] sel;
reg x, y;

always @(*)
begin
    case (sel)
        2'b00: begin
            x = a;
            y = b;
        end

        2'b01: begin
            x = c;
            y = d;
        end

        2'b10: begin
            x = e;
        end

        default: begin
            x = f;
            y = g;
        end
    endcase
end
```

In the `2'b10` case, `y` is not assigned.

Therefore, `y` must retain its previous value for that condition, which can result in a latch.

The important rule is:

```text
Every output must be assigned
for every possible condition.
```

---

# 8. `if` vs `case`

| Feature | `if / else if` | `case` |
|---|---|---|
| Main use | Conditional / priority logic | Multiple value selection |
| Priority | Yes | Normally no priority between case items |
| Typical hardware | Priority MUX | MUX / decode logic |
| `default` / `else` | `else` recommended | `default` recommended |
| Incomplete assignments | Can infer latch | Can infer latch |

---

## Learning Outcomes

- Understand the syntax and operation of **`if`, `else if`, and `else`**.
- Understand how `if` statements can create **priority logic**.
- Understand the syntax and use of **`case` statements**.
- Learn why incomplete `if` and `case` statements can infer **unintended latches**.
- Understand the importance of assigning outputs for **all possible conditions**.
```

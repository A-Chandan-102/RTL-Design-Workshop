# Lab 4: Walkthrough of the Sky130 Standard Cell Library

In this lab, I explored the **Sky130 standard cell `.lib` file** to understand how standard cells are characterized and how information such as PVT conditions, timing, power, area, and functionality is defined.

## Opening the Sky130 Library

The Sky130 library was opened using **GVim** to examine its structure and understand the information provided for each standard cell.

The library begins with a `library` definition that specifies the name and overall characteristics of the technology library.

## PVT – Process, Voltage and Temperature

**PVT** stands for **Process, Voltage, and Temperature**. These conditions affect the electrical and timing behaviour of a standard cell.

- **Process:** Represents manufacturing variations in the semiconductor fabrication process.
- **Voltage:** Specifies the operating supply voltage of the cell.
- **Temperature:** Specifies the operating temperature.

The library explored uses:

```text
Process     → Typical (tt)
Voltage     → 1.80 V
Temperature → 25°C
```

Different PVT combinations can be characterized separately because cell delay and power vary with operating conditions.

## Delay Model

The library uses the following delay model:

```text
delay_model : "table_lookup"
```

The **table lookup** model stores timing information in lookup tables based on parameters such as input transition and output load.

This allows the synthesis and timing tools to estimate cell delay under different operating conditions.

## Technology

The library specifies:

```text
technology : "cmos"
```

**CMOS (Complementary Metal-Oxide-Semiconductor)** is the underlying technology used to implement the digital standard cells.

## Units Used in the Library

The `.lib` file defines the units used for different electrical and physical parameters.

```text
Time       → ns
Voltage    → V
Current    → mA
Resistance → kohm
Capacitance → pF
Leakage Power → mW
```

Using defined units ensures that timing, power, and electrical characteristics are interpreted consistently by synthesis and analysis tools.

## Cell Definition

The keyword:

```text
cell ("cell_name") {
```

marks the **beginning of a standard cell definition**.

Each cell definition contains information about that particular cell, including its area, power characteristics, pins, functionality, timing, and electrical properties.

## Different Flavours of the Same Gate

The library contains multiple flavours of the same logic gate, such as different versions of an AND gate.

These different flavours provide different **drive strengths**, resulting in different delay, area, and power characteristics.

Therefore, the appropriate cell flavour can be selected depending on the timing and power requirements of the design.

## Comparison of Three AND Gate Flavours

During the library walkthrough, three different flavours of the **2-input AND gate** were compared:

```text
sky130_fd_sc_hd__and2_0
sky130_fd_sc_hd__and2_2
sky130_fd_sc_hd__and2_4
```

The main difference between these cells is their **drive strength**, which affects their area, power, and timing characteristics.

| AND Gate | Area | Drive Strength |
|---|---:|---|
| `and2_0` | 6.25 | Lower |
| `and2_2` | 7.50 | Medium |
| `and2_4` | 8.75 | Higher |

As the drive strength increases, the cell generally becomes larger and can provide more current, resulting in **better drive capability and lower delay**, but with increased area and power.

## Learning Outcomes

- Learned how to explore a **Sky130 `.lib` standard cell library**.
- Understood **PVT (Process, Voltage, Temperature)** conditions.
- Understood the **table lookup delay model** used for cell characterization.
- Learned about the **CMOS technology** and different units used in the library.
- Understood how the `cell` keyword defines the beginning of a standard cell description.
- Learned why different **flavours and drive strengths** of the same gate are provided.
- Explored different types of **standard logic cells** available in the library.

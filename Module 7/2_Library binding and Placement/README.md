# Library Binding and Placement

## 1. Library Binding – Binding the Netlist with Physical Cells

After **logic synthesis**, the RTL is converted into a **gate-level netlist**.

The next step is to map every logical gate in the netlist to an actual **physical standard cell** available in the technology library.

### What is Library Binding?

**Library binding** means:

> Matching each logical cell in the synthesized netlist with its corresponding physical implementation from the standard-cell library.

## 2. Different Flavours of a Cell

Similarly, different versions of flip-flops, multiplexers and logic gates can exist.

**Why are different flavours required?**

Different parts of a chip have different requirements.

Therefore, the tool selects an appropriate cell depending on:

- Timing
- Load capacitance
- Wire length
- Power
- Area
- Slew requirements

The screenshots show the same logical structures being represented by different physical cells/flavours during implementation.

## 3. Placement

After cells are bound to physical library cells, they must be assigned physical locations inside the floorplan.

Placement decides:

> Where should each standard cell be physically located?

Placement considers:

- Cell locations
- Connectivity between cells
- Wire length
- Congestion
- Timing
- Power
- Design rules

A good placement keeps highly connected cells reasonably close together.

## 4. Placement Optimization

Initial placement is generally not the final placement.

The tool estimates:

- Wire length
- Wire capacitance
- Delay
- Slew

and then modifies the placement to improve the design.

The objective is to obtain a design with acceptable:

- Timing
- Power
- Area
- Congestion

## 5. Repeaters / Buffers

### What is a Repeater?

A repeater is a buffer or chain of buffers inserted along a long signal path to improve signal transmission.

Without a repeater:

```
Driver ───────────────────────────────> Load
          Long wire
```

A long wire has significant resistance and capacitance, causing:

- Large delay
- Poor slew
- Signal degradation

With a repeater:

```
Driver ─────────> BUF ─────────> Load
```

or

```
Driver ─────> BUF ─────> BUF ─────> Load
```

The repeaters divide the long interconnect into smaller sections.

**Why are repeaters inserted?**

They are inserted based mainly on:

- Wire length
- Capacitance
- Timing requirements
- Slew requirements

The screenshot shows repeaters being inserted after estimating wire length and capacitance.

## 7. What is Slew?

Slew is the time taken by a signal to transition between logic levels.

For example, a rising transition:

```
0 ────────────────╲
                    ╲
                     ╲──────── 1
```

The transition from 0 → 1 is not instantaneous.

Similarly, a falling transition:

```
1 ────────────────╲
                    ╲
                     ╲
                      ──────── 0
```

**Rise Slew**

Time taken for a signal to rise from a lower voltage to a higher voltage.

**Fall Slew**

Time taken for a signal to fall from a higher voltage to a lower voltage.

Poor slew means a signal is changing too slowly.

## 8. Abutment

Abutment means placing two standard cells directly next to each other so that their boundaries touch.

Example:

```
┌────────┬────────┬────────┐
│  CELL  │  CELL  │  CELL  │
│   A    │   B    │   C    │
└────────┴────────┴────────┘
```

There is no unnecessary gap between adjacent cells.

Standard cells are designed with compatible dimensions and power-rail structures so that they can be placed in rows and abut each other.

**Why is abutment useful?**

It helps:

- Reduce wasted area
- Maintain regular cell rows
- Simplify power distribution
- Improve placement density

---

# Physical Design Flow

The overall flow discussed is:

```
RTL
 ↓
Logic Synthesis
 ↓
Floorplanning
 ↓
Library Binding / Placement
 ↓
Clock Tree Synthesis (CTS)
 ↓
Routing
 ↓
Static Timing Analysis (STA)
```

## 1. Logic Synthesis

Converts RTL into a gate-level representation using cells from the technology library.

```
RTL
 ↓
Logic synthesis
 ↓
Gate-level netlist
```

### 2. Floorplanning

Defines the physical organization of the chip:

### 3. Placement

Places the standard cells inside the floorplan.

### 4. Clock Tree Synthesis – CTS

Creates and balances the clock distribution network.

### 5. Routing

Creates physical metal connections between placed cells.

The router uses available metal layers and vias while satisfying design rules.

### 6. Static Timing Analysis – STA

STA checks whether the implemented design satisfies timing requirements.

Important checks include:

# Lab – Placement

## Types of Placement

There are mainly two important stages/types of placement:

### 1. Global Placement

Global placement determines the approximate locations of all cells.

The main objective is to obtain a good overall arrangement while considering:

- Wire length
- Timing
- Congestion
- Density
- Connectivity

The cells are not necessarily placed at their final legal locations yet.

It focuses on the overall distribution of cells.

### 2. Detailed Placement

Detailed placement takes the result of global placement and moves cells to legal, exact locations.

It ensures that cells:

- Fit inside placement rows
- Follow legal site positions
- Do not overlap
- Maintain required spacing
- Satisfy placement constraints

## Lab Flow – Placement in OpenLane

The placement stage is performed after synthesis and floorplanning.

The lab uses the OpenLane flow and its generated run directory.

A typical OpenLane run creates a directory such as:

```
runs/
└── <run-date-and-time>/
    ├── logs/
    ├── results/
    ├── reports/
    └── tmp/
```

The generated placement information can be found under the placement-related results/log directories.

## Reviewing the Placement

The lab involves opening the generated layout using the layout viewer and examining the placement.

The layout shows:

- Core boundary
- Standard-cell rows
- Standard cells
- Power structures
- Macros / preplaced blocks
- Decoupling cells
- I/O pins
- Metal layers

At a zoomed-out level, the placement can appear as a dense collection of cells.

At a higher zoom level, individual cells and their metal connections can be inspected.

The layer/toolbox panel can be used to:

- Enable/disable layer visibility
- Identify a selected layer
- Inspect objects on a particular layer
- Understand how different physical structures are represented

The lab screenshot shows using the layout viewer/toolbox and the console to inspect selected layers and objects.

# Key Takeaways

1. Library binding maps logical gates to physical standard cells.

2. A library contains many flavours of the same logical function, differing in drive strength, delay, area and power.

3. Placement assigns physical locations to the cells inside the floorplan.

4. Global placement finds approximate cell locations.

5. Detailed placement makes those locations legal and exact.

6. Long wires increase resistance and capacitance.

7. Large capacitance can result in poor slew and increased delay.

8. Repeaters/buffers are inserted on long/high-capacitance paths to improve signal quality and timing.

9. Abutment means placing compatible standard cells directly next to each other without unnecessary gaps.

10. The major physical-design flow is:

    ```
    Synthesis → Floorplanning → Placement → CTS → Routing → STA
    ```

11. During the placement lab, the generated layout should be inspected for cell distribution, congestion, macros, decaps, pins, power structures and metal layers.

12. The standard-cell library provides the common collection of gates and sequential cells used throughout the implementation flow.

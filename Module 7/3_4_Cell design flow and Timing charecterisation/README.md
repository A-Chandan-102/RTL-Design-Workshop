# Cell Design and Characterization

A **standard cell** is a pre-designed, reusable logic building block used during digital ASIC implementation. Standard-cell libraries contain cells with different functionalities and different physical sizes/flavours so that the design can be optimized for **area, power and timing**.

---

# 1. Standard Cell Flavours

The same functionality can be available in different sizes and threshold-voltage flavours.

### Different functionality

### Different sizes

### Different threshold-voltage flavours

Cells can also have different threshold voltages, such as:

- **LVT** → Low Threshold Voltage → Faster, higher leakage
- **HVT** → High Threshold Voltage → Slower, lower leakage

Thus, a library provides multiple choices of the same basic functionality.

---

# 2. Cell Design Flow

The standard-cell design flow can be divided into three major sections:

```
                 CELL DESIGN FLOW
                        │
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
     INPUTS          DESIGN STEPS      OUTPUTS
```

## Inputs

The major inputs are:

1. Process Design Kits (PDKs)
2. DRC rules
3. LVS rules
4. SPICE device models
5. Existing library information
6. User-defined specifications

The PDK provides the technology-specific information required to design and verify the cell.

---

# 3. Design Steps

The main design steps are:

1. Circuit Design
2. Layout Design
3. Characterization

## 3.1 Circuit Design

The required logic function is first implemented at transistor level.

The circuit is simulated using SPICE models to verify its electrical behaviour.

Important parameters include:

- Voltage
- Current
- Delay
- Power
- Input capacitance
- Output behaviour

## 3.2 Layout Design

The transistor-level circuit is converted into a physical layout.

Important concepts used during layout include:

- Euler's Path
- Stick Diagram
- Transistor Arrangement
- Metal Routing
- Contacts / Vias

The layout must satisfy the technology rules and should be optimized for:

- Area
- Parasitics
- Performance
- Power

### Euler's Path

Euler's path is used to arrange transistors so that a continuous diffusion path can be obtained, reducing diffusion breaks and potentially reducing area.

### Stick Diagram

A stick diagram is a simplified representation of the physical layout showing:

- Diffusion
- Polysilicon
- Metal
- Contacts
- VDD
- GND

It acts as an intermediate planning stage before the actual layout.

---

# 4. Outputs of Cell Design

The completed cell design produces several files/models:

1. CDL
2. GDSII
3. LEF
4. Extracted SPICE netlist
5. Timing library
6. Power library
7. Noise library
8. Functional information

## Important files

**CDL**
→ Circuit Description Language representation of the cell.

**GDSII**
→ Complete physical layout database.

**LEF**
→ Abstract physical information used during placement and routing.

**Extracted SPICE netlist**
→ Netlist containing extracted parasitic information from layout.

---

# 5. Characterization

After circuit and layout design, the cell must be characterized.

Characterization determines how the cell behaves under different:

- Input slew
- Output load capacitance
- Input transitions
- Output transitions

The obtained data is used to generate library models for synthesis, placement, routing and timing analysis.

# 6. Characterization Flow

A typical characterization setup contains:

1. Device SPICE models
2. Cell netlist
3. Input stimulus
4. Output load
5. Control statements
6. SPICE simulation
7. Extraction of timing/power/noise data

The shown characterization flow uses:

Tools such as GUNA are used in the characterization process to generate library models containing:

- Timing
- Power
- Noise
- Functional information

---

# 7. Timing Characterization

Timing characterization determines the timing behaviour of a standard cell.

The important timing quantities are:

1. Timing thresholds
2. Propagation delay
3. Transition time / Slew

## 7.1 Timing Threshold Definitions

The following thresholds are used:

```
slew_low_rise_thr
slew_high_rise_thr
slew_low_fall_thr
slew_high_fall_thr

in_rise_thr
in_fall_thr

out_rise_thr
out_fall_thr
```

The slew thresholds define the beginning and end points of a signal transition.

---

# 8. Propagation Delay

Propagation delay is the time taken for a change at the input of a cell to produce the corresponding change at its output.


# 9. Transition Time / Slew

Slew or transition time describes how quickly a signal changes between its low and high voltage levels.

### Rising Slew

$$ Rise\ Slew = time(slew\_high\_rise\_thr) - time(slew\_low\_rise\_thr) $$

### Falling Slew

$$ Fall\ Slew = time(slew\_low\_fall\_thr) - time(slew\_high\_fall\_thr) $$

Therefore:


---

# 10. Complete Cell Design Flow

```
                  PDK
                   ↓
        DRC / LVS Rules
        SPICE Models
        Library Specifications
                   ↓
            Circuit Design
                   ↓
             Simulation
                   ↓
             Layout Design
                   ↓
       DRC / LVS Verification
                   ↓
          Parasitic Extraction
                   ↓
           Characterization
                   ↓
       ┌───────────┼───────────┐
       ↓           ↓           ↓
    Timing       Power        Noise
       ↓           ↓           ↓
              Standard Cell
                 Library
                   ↓
       Synthesis / Placement /
         Routing / STA
```

---

# 11. Key Points

1. Standard cells are reusable physical logic building blocks.

2. A library contains multiple functionalities such as AND, OR, BUF, INV, DFF, LATCH and ICG.

3. The same functionality can have different sizes and threshold-voltage flavours.

4. Cell design consists mainly of: Circuit design → Layout design → Characterization.

5. PDKs provide technology-specific rules, models and data.

6. Euler's path and stick diagrams help in layout planning.

7. Major cell-design outputs include CDL, GDSII, LEF and extracted SPICE netlists.

8. Characterization generates timing, power, noise and functional library information.

9. Timing characterization uses threshold definitions, propagation delay and transition time.

10. Propagation delay:

    $$ delay = output\ threshold\ time - input\ threshold\ time $$

11. Slew / transition time measures how quickly a signal rises or falls.

12. Delay and slew depend on input slew and output load.

13. The characterized library is later used by synthesis, placement, routing and STA tools.

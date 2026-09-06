SoC Design and OpenLane

## 1. SoC Design

A **System-on-Chip (SoC)** integrates multiple functional blocks onto a single chip instead of using separate chips for each function.

A typical SoC can contain:

- **Processor / CPU core**
- **SRAM / memory macros**
- **GPIO and peripheral interfaces**
- **PLL / clock generation**
- **DAC / ADC and other IPs**
- **I/O pads**
- **Power and ground connections**

The processor communicates with these blocks through defined interfaces, allowing the complete system to operate as a single integrated chip.

---

## 2. Open-Source ASIC Design

Open-source ASIC design combines openly available:

- **RTL designs**
- **EDA tools**
- **PDKs**
- **IP blocks**
- **Design flows**

This makes it possible to move from RTL to a manufacturable chip using an open design ecosystem.

The **Sky130 PDK** provides the technology-specific information required to implement a design using the SkyWater 130 nm process.

---

## 3. EDA Tools

EDA (**Electronic Design Automation**) tools are used throughout the ASIC design process.

---

## 4. Simplified RTL-to-GDSII Flow

The overall ASIC flow can be represented as:

```text
RTL + PDK
   ↓
Synthesis
   ↓
Floorplanning + Power Planning
   ↓
Placement
   ↓
Clock Tree Synthesis (CTS)
   ↓
Routing
   ↓
Sign-off
   ↓
GDSII
```

### Main stages

**Synthesis:** Converts RTL into a gate-level representation using cells from the target technology.

**Floorplanning:** Determines the physical arrangement of the core area, macros, I/O and power structures.

**Placement:** Places standard cells within the floorplan.

**CTS:** Builds the clock distribution network to deliver the clock to sequential elements.

**Routing:** Creates physical connections between the placed cells.

**Sign-off:** Performs final checks such as timing, DRC, LVS and other physical verification steps before generating the final layout.
---

## 6. OpenLane ASIC Flow

The OpenLane flow includes several important stages:

```text
Design RTL
    ↓
RTL Synthesis
    ↓
STA / DFT
    ↓
Floorplanning
    ↓
Placement
    ↓
Optimization
    ↓
Clock Tree Synthesis
    ↓
Global Routing
    ↓
Detailed Routing
    ↓
LEC / Physical Verification
    ↓
RC Extraction
    ↓
STA
    ↓
GDSII
```

OpenLane also provides **design exploration**, allowing the design to be iterated and optimized based on factors such as timing, area and routing results.

---

## 5. Key Learning Outcomes

- I understood the basic structure and purpose of an **SoC**.
- I learned how **EDA tools** are used across the ASIC design flow.
- I understood the complete **RTL-to-GDSII** process.
- I learned how **OpenLane** automates major stages of digital ASIC implementation.
- I understood the role of the **Sky130 PDK** in technology-specific chip design.
- I connected RTL design, synthesis, physical implementation and final **GDSII layout** into one complete flow.

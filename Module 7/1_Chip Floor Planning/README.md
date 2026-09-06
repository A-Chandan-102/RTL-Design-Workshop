# Theory: Chip Floor Planning

## 1. What is Floor Planning?

Floor planning is the stage of physical design in which the overall physical organization of a chip is decided before detailed placement and routing.

The main objectives are to determine:

- Core and die dimensions
- Core utilization
- Aspect ratio
- Locations of large/pre-placed cells and IPs
- Power distribution structure
- I/O pin locations
- Placement regions and blockages

A good floorplan should provide enough space for standard cells and routing while keeping important connections short and avoiding congestion.

The basic physical-design sequence can be viewed as:

```
Netlist
   ↓
Floor Planning
   ↓
Power Planning
   ↓
Placement
   ↓
CTS
   ↓
Routing
```

## 2. Core and Die

The die is the complete piece of silicon containing the chip.

The core is the main internal region where the functional logic cells are placed.

Conceptually:

```
+--------------------------------------+
|                  DIE                 |
|                                       |
|    +----------------------------+    |
|    |            CORE            |    |
|    |                            |    |
|    |   Standard cells / IPs     |    |
|    |                            |    |
|    +----------------------------+    |
|                                       |
+--------------------------------------+
```

The space between the core and die boundary provides room for:

- I/O structures
- Power distribution
- Routing
- Physical margins
- Other required boundary structures

The images demonstrate that a die is essentially the larger physical region containing the core.

## 3. Width and Height

The dimensions of the core/die are represented by:

$$ Area = Width \times Height $$

For example, the image shows a rectangular core with dimensions represented in units.

The dimensions must be selected carefully because increasing the available area generally makes placement and routing easier, while excessive area increases chip size.

## 4. Utilization Factor

The utilization factor indicates how much of the available core area is occupied by logical standard cells.

The basic relationship is:

$$ Utilization = \frac{\text{Area occupied by cells}}{\text{Available core area}} $$

or, as a percentage,

$$ Utilization(\%) = \frac{\text{Cell area}}{\text{Core area}}\times100 $$

For example, the image illustrates:

**Utilization Factor = 0.5**

This means approximately 50% of the core area is occupied by the cells, leaving the remaining area for routing, buffering, optimization and other physical requirements.

**Why not use 100% utilization?**

Because routing requires free space.

Very high utilization can cause:

- Routing congestion
- Difficulty in placing cells
- Timing problems
- Increased wire length
- Difficulty in power distribution
- Reduced optimization freedom

Therefore, some whitespace is deliberately maintained.

## 5. Aspect Ratio

The aspect ratio is the relationship between core height and core width.

A commonly used definition is:

$$ Aspect\ Ratio = \frac{Height}{Width} $$

The image gives:

**Aspect Ratio = 0.5**

For example:

```
Width  = 4 units
Height = 2 units

Aspect Ratio = 2/4 = 0.5
```

Thus:

```
             Width = 4
      ┌────────────────────┐
      │                    │
      │       CORE         │ Height = 2
      │                    │
      └────────────────────┘

Aspect Ratio = Height / Width = 2 / 4 = 0.5
```

A suitable aspect ratio helps produce a practical layout with reasonable routing distances and congestion.

## 6. Relationship Between Utilization and Core Size

Suppose the total cell area is fixed.

If utilization is increased:

$$ Core\ Area = \frac{Cell\ Area}{Utilization} $$

For example, with a fixed cell area:

- Higher utilization → smaller core
- Lower utilization → larger core

A larger core provides more whitespace and routing flexibility, but increases die area.

Therefore floorplanning is a trade-off between:

**Area ↔ Routing ↔ Timing ↔ Power**

## 7. Pre-placed Cells

A chip does not consist only of ordinary standard cells.

Large functional blocks such as:

- Memory
- Clock-gating structures
- Comparators
- MUX structures
- Analog blocks
- Other IP blocks

may need specific physical locations.

These are called **pre-placed cells** or **pre-placed IPs** because their locations are determined before automated placement.

The image specifically shows different blocks being separated and treated as modules/IPs.

The general idea is:

```
+---------------------------+
|                           |
|   Block 1       Block 2   |
|                           |
|                           |
|      Standard cells       |
|                           |
+---------------------------+
```

The automated placement tool then places the remaining standard cells around these fixed regions.

## 8. Black Boxes

A black box represents a block whose internal implementation is not being considered at that stage.

The floorplanning tool only needs to know:

- Its dimensions
- Its input/output ports
- Its physical location
- Its connectivity

Its internal logic is abstracted away.

For example:

```
        a  ──►
        b  ──►
        c  ──►   +-------------+
        d  ──►   |    BLOCK    | ──► o
                 +-------------+
```

Internally, the block may contain many gates, but for floorplanning it can be treated as a single physical object.

This is particularly useful for:

- IP blocks
- Memories
- Analog macros
- Reusable modules

## 9. Floorplanning of IPs

The arrangement of large IPs/blocks inside the chip is part of floorplanning.

For example:

```
+--------------------------------+
|                                |
|  +------+       +------+       |
|  | IP 1 |       | IP 2 |       |
|  +------+       +------+       |
|                                |
|       Standard-cell area       |
|                                |
|  +------+                      |
|  | SRAM |                      |
|  +------+                      |
|                                |
+--------------------------------+
```

The locations are selected to:

- Reduce long interconnects
- Reduce congestion
- Improve timing
- Provide routing channels
- Simplify power distribution

## 10. Decoupling Capacitors

A decoupling capacitor (decap) is used to stabilize the local power supply.

The image illustrates the problem caused by simultaneous switching.

When multiple logic cells switch at the same time, they may suddenly demand a large amount of current.

Because the power network contains resistance and inductance, the voltage at the circuit can temporarily fall.

This is known as:

**Voltage droop**

Simplified:

```
Power supply
     │
     R + L
     │
     ├──────────────► Logic
     │
    Cdecap
     │
    GND
```

## 11. Working of a Decoupling Capacitor

A decoupling capacitor is connected between:

```
VDD ── C ── VSS
```

The capacitor stores charge.

When the circuit suddenly demands current:

```
Logic switches
      ↓
Current demand increases
      ↓
VDD begins to droop
      ↓
Decap supplies local current
      ↓
Voltage droop is reduced
```

When the switching event is over, the power network recharges the capacitor.

Therefore decaps act as a local temporary charge reservoir.

## 12. Why Decaps Are Important in Floorplanning

Decap cells are placed strategically across the core.

Their purpose is to:

- Reduce local voltage fluctuations
- Improve power integrity
- Reduce supply noise
- Support cells during transient current demand

However, decaps also consume physical area, so their insertion must be balanced with area and routing requirements.

## 13. Power Planning

After deciding the rough physical organization, the chip requires a proper power distribution network (PDN).

The purpose is to distribute:

- VDD
- VSS/GND

throughout the core.

The image illustrates power being distributed through horizontal and vertical power structures.

A simplified structure is:

```
             VDD
══════════════════════════
│    │    │    │    │
│    │    │    │    │
══════════════════════════
             VSS
```

The power network provides low-resistance paths to the cells.

## 14. Why Power Planning Is Necessary

A standard cell may be physically far from the main power source.

If current travels through long or narrow metal paths, resistance and inductance cause voltage drop.

The approximate resistive voltage drop is:

$$ V_{drop}=I R $$

Thus higher current or higher resistance produces greater voltage drop.

Power planning attempts to keep this drop within acceptable limits.

## 15. Power Grid

The image shows a grid structure formed using horizontal and vertical metal layers.

For example:

```
VDD  ─────────┬──────────────
              │
VSS  ─────────┼──────────────
              │
VDD  ─────────┼──────────────
              │
VSS  ─────────┴──────────────
```

The overlapping power network distributes current across the chip rather than depending on a single narrow path.

This improves:

- IR-drop performance
- Current distribution
- Power integrity
- Reliability

## 16. Single Power Tap Problem

The image demonstrates an important point.

Suppose several capacitive loads are initially at different voltages and all suddenly need to charge through one VDD access point.

Then a large transient current flows through the power network.

This can produce:

```
Large transient current
        ↓
IR drop + inductive effects
        ↓
Temporary VDD reduction
        ↓
Voltage droop at loads
```

A properly designed PDN, together with decaps, helps reduce this effect.

## 17. Pin Placement

I/O pins must also be assigned appropriate locations around the die.

The image shows pins such as:

- Din1
- Din2
- Din3
- Din4
- CLK1
- CLK2
- Dout1
- Dout2
- Dout3
- Dout4
- ClkOut

Pins can be placed along different sides of the die.

Good pin placement attempts to:

- Reduce wire length
- Reduce routing congestion
- Group related signals
- Make power connections easier
- Avoid crossing critical routes unnecessarily

## 18. Why Pin Placement Matters

Consider two blocks:

```
Input ─────────────────────────────► Block
```

versus:

```
Input ──► Block
```

The second arrangement has a much shorter interconnection.

Therefore the position of I/O pins can significantly influence:

- Delay
- Routing congestion
- Wire length
- Power
- Overall timing

Pin placement should therefore consider both logical connectivity and physical location.

## 19. Metal Layers

Physical implementation uses multiple metal layers for interconnection.

The screenshots and OpenLane configuration show the use of multiple Sky130 metal layers.

The flow identifies layers such as:

- li1
- met1
- met2
- met3
- met4
- met5

Different layers can be used for different routing directions and purposes.

For example:

- Lower metals → local/inter-cell routing
- Higher metals → longer/global connections and power distribution

The exact usage depends on the technology rules and OpenLane configuration.

## 20. Important OpenLane Floorplanning Variables

OpenLane exposes several floorplanning variables.

From the provided configuration information, important ones include:

**FP_CORE_UTIL**

Defines target core utilization.

```tcl
set ::env(FP_CORE_UTIL) 35
```

**FP_ASPECT_RATIO**

Defines core aspect ratio.

```tcl
set ::env(FP_ASPECT_RATIO) 1
```

**FP_SIZING**

Controls whether relative or absolute sizing is used.

**DIE_AREA**

Can be used to specify a fixed die rectangle.

**FP_IO_HMETAL**

Specifies the metal layer used for horizontal I/O pins.

**FP_IO_VMETAL**

Specifies the metal layer used for vertical I/O pins.

**BOTTOM_MARGIN_MULT**

Controls the bottom core margin.

**TOP_MARGIN_MULT**

Controls the top margin.

**LEFT_MARGIN_MULT**

Controls the left margin.

**RIGHT_MARGIN_MULT**

Controls the right margin.

These variables allow the floorplan to be customized according to the design requirements.

---

# LAB — Running Floorplanning in OpenLane

## Aim

To run the OpenLane floorplanning stage for the picorv32a design, examine the generated floorplan files and logs, inspect the placement of the core, rows and I/O pins, and verify the physical metal layers used by the design.

## 1. Enter the OpenLane Environment

Start the OpenLane environment and enter interactive mode:

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
```

Then:

```bash
./flow.tcl -interactive
```

This starts the OpenLane interactive shell.

## 2. Select the Design

The design used in this exercise is:

```
picorv32a
```

The corresponding design directory is:

```
designs/picorv32a/
```

It contains the RTL and configuration required by OpenLane.

## 3. Run the Design Preparation

The design configuration is loaded from:

```
designs/picorv32a/config.tcl
```

The configuration defines parameters such as:

- DESIGN_NAME
- VERILOG_FILES
- CLOCK_PERIOD
- CLOCK_PORT
- FP_CORE_UTIL
- FP_ASPECT_RATIO

OpenLane then prepares the design and generates the required temporary files.

## 4. Create the Run Directory

When the flow starts, OpenLane creates a new run directory based on the execution date/time.

For example:

```
runs/06-09_16-47/
```

The run directory contains:

```
runs/
└── 06-09_16-47/
    ├── logs/
    ├── results/
    ├── reports/
    └── tmp/
```

This is important because the floorplanning output and logs are stored inside this particular run.

## 5. Examine Floorplanning Configuration

The floorplanning-related variables can be viewed in the OpenLane configuration.

Important parameters include:

- FP_CORE_UTIL
- FP_ASPECT_RATIO
- FP_SIZING
- DIE_AREA
- FP_IO_HMETAL
- FP_IO_VMETAL
- FP_PIN_ORDER_CFG
- FP_PDN_CORE_RING
- FP_PDN_VPITCH
- FP_PDN_HPITCH

These control the physical dimensions, I/O arrangement and power-network construction.

## 6. Floorplan Generation

OpenLane generates the floorplan using the synthesized design and the physical information from the PDK.

The floorplanning process determines:

- Die dimensions
- Core dimensions
- Standard-cell rows
- I/O pin locations
- Block locations
- Initial power-grid information

A DEF file is generated to describe the physical arrangement.

The provided screenshot shows the generated file:

```
picorv32a.floorplan.def
```

## 7. DEF File

DEF — Design Exchange Format describes the physical implementation information of the design.

The generated floorplan DEF contains information such as:

- Die area
- Rows
- Components
- Pins
- Nets
- Locations
- Physical placement data

The screenshot shows statements such as:

```
DIEAREA (...)
ROW ROW_0 ...
ROW ROW_1 ...
ROW ROW_2 ...
...
```

This indicates the floorplan has been divided into standard-cell rows.

## 8. Standard-Cell Rows

The floorplan contains repeated rows for placement of standard cells.

Conceptually:

```
ROW 0  ─────────────────────────
ROW 1  ─────────────────────────
ROW 2  ─────────────────────────
ROW 3  ─────────────────────────
ROW 4  ─────────────────────────
       ...
```

The rows are aligned according to the standard-cell architecture of the Sky130 library.

The placement tool later places standard cells into these rows.

## 9. I/O Placement

The OpenLane flow also performs I/O placement.

The provided ioPlace log shows the I/O-placement stage.

This stage determines the physical positions of the design's input and output pins around the chip boundary.

The pin placement should satisfy:

- Design constraints
- Metal-layer constraints
- Pin-order requirements
- Routing considerations

## 10. Reviewing the Floorplan

After the floorplan is generated, it can be opened in a physical-design layout viewer.

The provided screenshot shows the floorplan loaded into the layout viewer.

At a high level, the layout contains:

```
+--------------------------------------+
|             Die Boundary             |
|  ┌──────────────────────────────┐    |
|  │                              │    |
|  │            CORE              │    |
|  │                              │    |
|  │    Standard cell rows        │    |
|  │                              │    |
|  └──────────────────────────────┘    |
|                                       |
+--------------------------------------+
```

The layout viewer allows us to inspect the actual physical representation rather than only the textual DEF file.

## 11. Checking the Layers

One of the important lab activities is checking which layers are present in the floorplan.

The layout viewer provides a Layers panel where individual physical layers can be selected or hidden.

The Sky130 flow uses multiple layers, including:

- li1
- met1
- met2
- met3
- met4
- met5

The layer viewer is useful for understanding:

- Which metal layer a shape belongs to
- Power-grid structures
- Standard-cell connections
- Routing layers
- Contacts and vias

## 12. Layer Visibility

In the layout tool, layers can be selectively enabled or disabled.

For example:

```
met1 → visible
met2 → visible
met3 → hidden
```

This makes it easier to inspect a particular portion of the physical design.

The provided screenshot demonstrates the layer-selection and visibility controls of the layout viewer.

## 13. What Was Observed in the Layout

The floorplan contains many repeated structures corresponding to:

- Standard-cell rows
- Power structures
- Physical boundaries
- Pins
- Routing-related shapes
- Decap/endcap structures

The physical layout is much more detailed than the logical RTL because every logical component eventually requires a physical representation.

## 14. Floorplan Log

OpenLane records the floorplanning activity in the run logs.

The provided log shows OpenROAD processing the merged LEF and generating the floorplan data.

Important messages include:

```
Reading LEF file
Created technology layers
Created technology vias
Created ... technology cells
Finished LEF file
Reading DEF file
Created ... components
Created ... nets
...
```

This demonstrates that the physical-design engine is reading the technology and design information and constructing the physical representation.

## 15. Floorplan Result

The floorplanning stage produces important outputs such as:

```
results/floorplan/
```

Typical files include:

```
picorv32a.floorplan.def
```

and associated intermediate files.

The DEF gives the physical description of the floorplan and can be used by subsequent placement and routing stages.

---

# Key Takeaways

### Core and Die

The die is the complete chip area, while the core is the main region containing the functional logic.

### Utilization

$$ Utilization = \frac{Cell\ Area}{Core\ Area} $$

Lower utilization leaves more whitespace for routing and optimization.

### Aspect Ratio

$$ Aspect\ Ratio = \frac{Height}{Width} $$

It determines the shape of the core.

### Pre-placed Cells

Large IPs/macros can be given fixed locations before automated placement.

### Black Boxes

A black box hides internal implementation while exposing only its interface and physical requirements.

### Decaps

Decoupling capacitors provide local charge during switching events and help reduce voltage droop.

### Power Planning

A power grid distributes VDD and VSS across the core while reducing IR-drop and power-integrity problems.

### Pin Placement

Pin positions strongly affect wire length, congestion and timing.

### Layers

Different physical layers are used for interconnect, power distribution and other layout structures.

### DEF

The DEF file is a textual representation of physical design information such as die area, rows, components and connectivity.

---

# Overall Lab Flow

```
Start OpenLane
      ↓
Select picorv32a
      ↓
Load config.tcl
      ↓
Load Sky130 PDK
      ↓
Prepare design
      ↓
Create timestamped run directory
      ↓
Merge / read LEF data
      ↓
Generate floorplan
      ↓
Generate I/O placement
      ↓
Generate floorplan DEF
      ↓
Review layout
      ↓
Inspect layers
      ↓
Check floorplan logs/results
```

# Result

The picorv32a design was successfully taken through the floorplanning stage of the OpenLane flow. The experiment demonstrated the selection of core utilization and aspect ratio, generation of the die/core floorplan, placement of I/O pins, creation of standard-cell rows, generation of the floorplan DEF file, and inspection of the resulting layout and Sky130 metal layers.

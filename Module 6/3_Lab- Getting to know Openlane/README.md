# Lab – Getting to Know OpenLane

## Aim

To understand the basic usage of **OpenLane** for an ASIC design by invoking the flow on the `picorv32a` design, examining the design files and configuration, merging LEF files, performing synthesis, checking synthesis statistics and generated results, and inspecting the synthesized netlist.

---

## Design Used

- **Design:** `picorv32a`
- **Main configuration:** `config.tcl`

The `picorv32a` design is a RISC-V based processor core used as the design example for understanding the OpenLane flow.

---

# 1. Invoking OpenLane

OpenLane can be run using the OpenLane Docker environment.

The flow can be started using:

```bash
./flow.tcl -interactive
```

This opens OpenLane in interactive mode.

The terminal output confirms the OpenLane version and initializes the flow environment.

---

# 2. OpenLane Directory Structure

The OpenLane working directory contains several important directories.

The `designs` directory contains the individual designs supported by the flow.

The `picorv32a` design directory contains the RTL source and configuration required to run the design through OpenLane.

---

# 3. Design Directory – picorv32a

The design-specific directory is:

```text
openlane/designs/picorv32a/
```

Important files include:

```text
picorv32a/
├── src/
│   └── picorv32a.v
├── config.tcl
└── ...
```

The RTL file contains the Verilog implementation of the processor core.

The `config.tcl` file specifies parameters and settings used by OpenLane for this particular design.

---

# 4. Configuration File

The configuration file defines the basic information required by OpenLane.

Important settings include:

```tcl
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) \
    "$::env(DESIGN_DIR)/src/picorv32a.v"

set ::env(SDC_FILE) \
    "$::env(DESIGN_DIR)/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "5.000"

set ::env(CLOCK_PORT) "clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT)
```

### Meaning of important parameters

### `DESIGN_NAME`

Specifies the top-level design name.

```tcl
set ::env(DESIGN_NAME) "picorv32a"
```

### `VERILOG_FILES`

Specifies the RTL Verilog files used by the design.

### `SDC_FILE`

Specifies the timing constraints file.

### `CLOCK_PERIOD`

Defines the target clock period.

For the example:

```tcl
set ::env(CLOCK_PERIOD) "5.000"
```

### `CLOCK_PORT`

Specifies the clock input port.

```tcl
set ::env(CLOCK_PORT) "clk"
```

These parameters allow OpenLane to understand which RTL design is being implemented and what timing constraints should be applied.

---

# 6. LEF Files

**LEF (Library Exchange Format)** files contain physical information about cells and macros.

LEF information is required by the physical-design stages for:

- Cell dimensions
- Cell pin locations
- Routing blockages
- Metal layers
- Macro information
- Physical abstracts

OpenLane processes the available LEF files before physical implementation.

---

# 7. Merging LEF Files

OpenLane merges the required LEF information from the technology and standard-cell libraries.

The flow output shows the merging process:

```text
Merging LEF Files...
```

During this stage:

- Technology information is collected.
- Standard-cell LEFs are processed.
- Macro LEFs are processed.
- The required physical abstracts are combined.

The resulting merged LEF is then used by later physical-design stages.

A merged LEF file is generated as part of the OpenLane run.

---

# 8. OpenLane Run Directory

Whenever an OpenLane flow is executed, a new run directory is created.

The run directory contains a timestamp/date-based name, for example:

```text
runs/
└── 06-09_16-47/
```

The exact directory name depends on the date and time at which the flow is executed.

This directory stores the intermediate and final results of that particular OpenLane run.

Typical contents include:

```text
runs/
└── <run_directory>/
    ├── tmp/
    ├── results/
    ├── reports/
    ├── logs/
    └── ...
```

This makes each OpenLane run reproducible and keeps the results of different runs separate.

---

This confirms that OpenLane has successfully initialized the design environment.

---

# 10. Synthesis

After preparation, the RTL design is synthesized.

The purpose of synthesis is to convert:

```text
RTL
  ↓
Gate-level representation
```

The RTL description of `picorv32a` is converted into a network of technology-specific standard cells.

# 11. Technology Mapping

For the `picorv32a` design, the final mapped network contains a large number of Sky130 cells.

The screenshot of the synthesis statistics shows that the design contains:

```text
Number of wires:      14596
Number of wire bits:  14978
Number of public wires: 1565
Number of public wire bits: 1947
Number of memories:   0
Number of processes:  0
Number of cells:      14876
```

The exact numbers belong to the particular synthesis run shown in the screenshot.

---

# 12. Synthesis Statistics

OpenLane/Yosys generates statistics describing the synthesized design.

# 13. Synthesized Netlist

The synthesis stage generates a gate-level Verilog netlist.

The synthesized netlist contains:

- Top-level module
- Input and output ports
- Internal wires
- Standard-cell instances
- Cell connections

Instead of describing functionality using high-level RTL constructs, the synthesized netlist describes the circuit using technology-mapped cells.

A simplified example is:

```verilog
module example (
    input  a,
    input  b,
    output y
);

wire _00001_;

sky130_fd_sc_hd__nand2_2 u1 (
    .A(a),
    .B(b),
    .Y(_00001_)
);

sky130_fd_sc_hd__inv_2 u2 (
    .A(_00001_),
    .Y(y)
);

endmodule
```

The actual `picorv32a` synthesized netlist is much larger and contains many thousands of internal wires and cell instances.

---

# 14. Inspecting the Netlist

The synthesized netlist can be opened using a text editor.

This demonstrates that the original RTL has been transformed into a gate-level representation.

---

# 15. Checking Reports

OpenLane stores reports separately from the generated design files.

The reports directory can contain information about:

- Synthesis
- Timing
- Area
- Power
- Placement
- Routing
- Design-rule checks
- Other implementation stages

A typical structure is:

```text
runs/<run_directory>/
├── reports/
│   ├── synthesis/
│   ├── placement/
│   ├── routing/
│   └── ...
```

These reports are used to evaluate the quality and correctness of the implementation.

---

# 16. Understanding the OpenLane Run

The general sequence followed in the experiment is:

```text
OpenLane
   ↓
Load design configuration
   ↓
Load Sky130 PDK
   ↓
Prepare design
   ↓
Merge LEF files
   ↓
Check and prepare configuration
   ↓
RTL synthesis
   ↓
Technology mapping
   ↓
Generate synthesis statistics
   ↓
Generate synthesized netlist
   ↓
Store reports and results
```

Later OpenLane stages continue from the synthesized design into:

```text
Floorplanning
    ↓
Placement
    ↓
Clock Tree Synthesis
    ↓
Routing
    ↓
Extraction
    ↓
Timing analysis
    ↓
Physical verification
    ↓
GDSII
```

---


# 18. Key Observations

### Observation 1 – OpenLane Initialization

The OpenLane environment was successfully invoked in interactive mode.

```text
./flow.tcl -interactive
```

The tool initialized the Sky130 technology environment.

### Observation 2 – Design Preparation

The `picorv32a` design configuration was successfully read.

The required LEF files were processed and merged.

### Observation 3 – Synthesis

The RTL was synthesized and mapped to Sky130 standard cells.

### Observation 4 – Statistics

The synthesis report showed the number of wires, wire bits and standard-cell instances used in the design.

### Observation 5 – Netlist

A synthesized gate-level Verilog netlist was generated and inspected.

---


# Key Takeaways

- **OpenLane** automates a large portion of the digital ASIC implementation flow.
- The **PDK** provides technology-specific information needed by the ASIC flow.
- **LEF files** provide physical abstracts required for physical implementation.
- **Liberty files** provide timing and cell characterization information.
- A **configuration file** tells OpenLane which design, RTL files, clock and constraints to use.
- Every OpenLane execution creates a separate **run directory** containing logs, reports and results.
- **Yosys** performs RTL synthesis and **ABC** assists with optimization and technology mapping.
- The synthesized design is represented as a network of **Sky130 standard cells**.
- The **synthesis statistics** help estimate the size and complexity of the design.
- The **synthesized netlist** is the gate-level representation that is passed to subsequent physical-design stages.
- The complete flow ultimately progresses from **RTL to GDSII**, which represents the physical layout of the chip.

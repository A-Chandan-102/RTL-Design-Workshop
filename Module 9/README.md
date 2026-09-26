# Pre-layout Timing Analysis and Importance of Good Clock Tree

**Reference repo for the labs below:** [github.com/nickson-jose/vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign) — contains everything needed to run the RTL2GDSII flow using OpenLane, plus the procedure for creating a custom LEF file and plugging it into the OpenLane flow.

---

## steps to convert grid info to track info

- In Magic's `tkcon` console, the current routing grid spacing for the loaded cell is queried:
  ```tcl
  % grid 0.46um 0.34um 0.23um 0.17um
  ```
  These four numbers are the horizontal pitch, vertical pitch, and the horizontal/vertical offsets of the routing grid overlaid on the layout — visible on-screen as the faint grid lines the `sky130_inv` cell sits on.

- Before this grid information is useful to OpenLane, it has to be re-expressed **per metal layer, per direction (X/Y), as an offset and a pitch** — this is the `tracks.info` format OpenLane's router reads:
  ```
  li1  X 0.23 0.46
  li1  Y 0.17 0.34
  met1 X 0.17 0.34
  met1 Y 0.17 0.34
  met2 X 0.23 0.46
  met2 Y 0.23 0.46
  met3 X 0.34 0.68
  met3 Y 0.34 0.68
  met4 X 0.46 0.92
  met4 Y 0.46 0.92
  met5 X 1.70 3.40
  met5 Y 1.70 3.40
  ```
  Format: `<layer> <direction> <offset> <pitch>`

- Why this matters: every pin on a **custom standard cell** must land exactly on one of these track coordinates. If a pin is off-grid, the router can't land a wire on it without a DRC violation — so converting "grid info" (from Magic) into "track info" (for OpenLane) is what makes a hand-drawn cell actually routable inside the automated flow.

- This lab also covers **labelling the cell's ports** directly in Magic, using the label/text tool (`textphelper` dialog):
  - `Text string`: the pin name (e.g. `A`)
  - `Attach to layer`: the physical layer the label sits on (here, `local` → the `li1` local-interconnect layer)
  - `Port`: checkbox enabled, with a port index/order number — this is what later lets Magic treat the label as an actual exportable pin rather than just a text annotation
- The four ports of the inverter (`VPWR`, `VGND`, `A`, `Y`) are labelled this way, each attached to the correct layer, matching the pin names later seen in the LEF.

---

## steps to convert magic layout to std cell LEF

- Before exporting a LEF, every labelled port needs a **port class** (its direction) and a **port use** (what kind of signal it carries) assigned via `tkcon`. Walking through each port of the `my_sky_inv` cell:
  ```tcl
  % what
  Selected mask layers:
      metal1  ( Topmost cell in the window )
  Selected label(s):
      "VGND" is attached to metal1 in cell def my_sky_inv
  % port class inout
  % port use ground

  % what
  Selected mask layers:
      nwell  pdiff  pdiffc  nsubdiff  nsubdiffcont  locali  viali  metal1
  Selected label(s):
      "VPWR" is attached to metal1 in cell def my_sky_inv
  % port class inout
  % port use power

  % what
  Selected mask layers:
      nmos  pmos  poly  polycont  locali
  Selected label(s):
      "A" is attached to locali in cell def my_sky_inv
  % port class input
  % port use signal

  % what
  Selected mask layers:
      locali  ( Topmost cell in the window )
  Selected label(s):
      "Y" is attached to locali in cell def my_sky_inv
  % port class output
  % port use signal
  ```
- Summary of the assigned port properties:

  | Pin | Layer | Class | Use |
  |---|---|---|---|
  | `VGND` | metal1 | inout | ground |
  | `VPWR` | metal1 | inout | power |
  | `A` | locali (li1) | input | signal |
  | `Y` | locali (li1) | output | signal |

- Once every port is classed and used correctly, Magic can write out a valid **LEF (Library Exchange Format)** abstract for the cell — saved here as `sky130_inv.lef`:
  ```lef
  VERSION 5.7 ;
  NOWIREEXTENSIONATPIN ON ;
  DIVIDERCHAR "/" ;
  BUSBITCHARS "[]" ;
  MACRO sky130_inv
      CLASS CORE ;
      FOREIGN sky130_inv ;
      ORIGIN 0.000 0.000 ;
      SIZE 1.380 BY 2.720 ;
      SITE unithd ;
      PIN A
          DIRECTION INPUT ;
          USE SIGNAL ;
          ANTENNAGATEAREA 0.165600 ;
          PORT
              LAYER li1 ;
                  RECT 0.060 1.180 0.510 1.690 ;
          END
      END A
      PIN Y
          DIRECTION OUTPUT ;
          USE SIGNAL ;
          ANTENNADIFFAREA 0.287800 ;
          PORT
              LAYER li1 ;
                  RECT 0.760 1.960 1.100 2.330 ;
                  RECT 0.880 1.690 1.050 1.960 ;
                  RECT 0.880 1.180 1.330 1.690 ;
                  RECT 0.880 0.760 1.050 1.180 ;
                  RECT 0.780 0.410 1.130 0.760 ;
          END
      END Y
      PIN VPWR
          DIRECTION INOUT ;
          USE POWER ;
          PORT
              LAYER nwell ;
                  RECT -0.200 1.140 1.570 3.040 ;
              LAYER li1 ;
                  RECT -0.200 2.580 1.430 2.900 ;
                  RECT  0.180 2.330 0.350 2.580 ;
                  RECT  0.100 1.970 0.440 2.330 ;
              LAYER mcon ;
                  RECT 0.230 2.640 0.400 2.810 ;
                  RECT 1.000 2.650 1.170 2.820 ;
          ...
  ```
- Key fields in this LEF:
  - `CLASS CORE` — marks this as a normal placeable standard cell (not a pad, block, or ring cell)
  - `SIZE 1.380 BY 2.720` — matches the fixed bounding box set earlier during layout (1.38 µm × 2.72 µm)
  - `SITE unithd` — ties the cell to the standard "unit height" placement row definition, so it aligns correctly with every other cell in the row
  - Each `PIN … PORT … RECT` block gives the exact shape(s), on the exact layer(s), where the placer/router is allowed to connect to that pin
  - `ANTENNAGATEAREA` / `ANTENNADIFFAREA` — antenna-effect area values, checked during fabrication sign-off to avoid gate-oxide charge damage from long metal routes

---

## SKY_L3 - Introduction to timing libs and steps to include new cell in synthesis

- OpenLane is launched interactively and the `picorv32a` design is prepped:
  ```tcl
  ./flow.tcl -interactive
  package require openlane 0.9

  prep -design picorv32a -tag 15-09_06-28 -overwrite
  [INFO]: Using design configuration at /openLANE_flow/designs/picorv32a/config.tcl
  mergeLef.py : Merging LEFs
  sky130_fd_sc_hd.lef: SITEs matched found: 0
  sky130_fd_sc_hd.lef: MACROs matched found: 437
  mergeLef.py : Merging LEFs complete
  padLefMacro.py : Padding technology lef file
  Derived SITE width (microns): 0.46
  Derived SITE height (microns): 5.44
  Right cell padding (microns): 3.68
  ...
  Skipping LEF padding for MACRO  sky130_fd_sc_hd__tap_2
  Skipping LEF padding for MACRO  sky130_fd_sc_hd__tapvgnd_1
  Skipping LEF padding for MACRO  sky130_fd_sc_hd__fill_4
  ...
  padLefMacro.py : Finished
  [INFO]: Preparation complete
  ```
  - `prep` merges every standard-cell LEF (437 macros found in `sky130_fd_sc_hd.lef`) and derives the placement **SITE** dimensions the whole flow will use.
  - Special cells — taps, decap cells, fill cells, tap+power/ground combo cells — are explicitly **skipped from padding**, since padding those would break their tight abutment requirements.

- The **custom** standard cell's LEF is then merged into this same run so OpenLane's placer/router can see and use it:
  ```tcl
  set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  /openLANE_flow/designs/picorv32a/src/sky130_vsdinv.lef

  add_lefs -src $lefs
  [INFO]: Merging /openLANE_flow/designs/picorv32a/src/sky130_vsdinv.lef
  ```
  From this point on, `sky130_vsdinv` (the custom inverter) is available to synthesis exactly like any built-in `sky130_fd_sc_hd` cell.

- A LEF alone only tells the tools the cell's **physical** shape — for synthesis and STA to actually use the new cell, it also needs a matching entry in a **Liberty (.lib) timing library**. The header of the standard library file used here, `sky130_fd_sc_hd_tt_025C_1v80.lib`, shows the structure such an entry must fit into:
  ```liberty
  library ("sky130_fd_sc_hd_tt_025C_1v80") {
      technology("cmos");
      delay_model : "table_lookup";
      bus_naming_style : "%s[%d]";
      time_unit : "1ns";
      voltage_unit : "1V";
      leakage_power_unit : "1nW";
      current_unit : "1mA";
      pulling_resistance_unit : "1kohm";
      capacitive_load_unit(1.0000000000, "pf");

      default_max_transition : 1.5000000000;
      default_arc_mode : "worst_edges";
      default_constraint_arc_mode : "worst";

      operating_conditions ("tt_025C_1v80") {
          voltage     : 1.8000000000;
          process     : 1.0000000000;
          temperature : 25.000000000;
          tree_type   : "balanced_tree";
      }
      power_lut_template ("power_inputs_1") {
          variable_1 : "input_transition_time";
          index_1("1, 2, 3, 4, 5, 6, 7");
      }
      power_lut_template ("power_outputs_1") {
          variable_1 : "input_transition_time";
          variable_2 : "total_output_net_capacitance";
          index_1("1, 2, 3, 4, 5, 6, 7");
          index_2("1, 2, 3, 4, 5, 6, 7");
      }
  ```
- Key points:
  - `delay_model : "table_lookup"` — confirms this whole library is characterized using **2D lookup tables** (input slew × output load), exactly the delay-table format covered in the theory below.
  - `operating_conditions ("tt_025C_1v80")` names the **PVT corner**: typical process, 25 °C, 1.8 V — one specific corner out of the many a real library ships (`tt`, `ss`, `ff`, at various voltages/temperatures).
  - The `power_lut_template` blocks predefine the lookup axes (`input_transition_time`, `total_output_net_capacitance`) that every individual cell's power tables in this file will be indexed against — the same two axes used for delay tables.
  - To fully "include" a new custom cell in synthesis, its own `cell(...)` entry — with its own timing and power lookup tables — would need to be added to a library like this one, so STA tools can compute delay/power for it just like a native cell.

---

## Theory: Delay Tables

### Introduction to Delay Tables (Power-Aware CTS)

- A **clock tree** is typically built as multiple **levels of buffering**, fanning out from a single clock source down to every flip-flop's clock pin.
- Two ways to build a level of the tree, contrasted here as "Power-Aware CTS":
  - **Plain buffer tree** — Level 1 buffer feeds two Level-2 buffers (`Cbuf1`, `Cbuf2`), each driving two flip-flop loads (`C1`/`C2` and `C3`/`C4`).
  - **Power-aware (clock-gated) tree** — Level 1 buffer feeds one ordinary buffer (`Cbuf1`) *and* one **clock-gating AND cell** (`Cand1`) with an enable input `EN`. The AND cell can gate off its branch of the clock tree entirely when that logic isn't active, saving dynamic power — while still presenting a comparable load to Level 1 as the plain-buffer version would.
- This is why the technique is called **power-aware CTS**: part of the clock tree's structure is chosen specifically to allow power gating, not purely to minimize delay/skew.

### Delay Table Usage — Worked Example

- A cell's delay is stored as a **2D lookup table**, indexed by:
  - **Input slew** (rows) — how fast the input signal itself transitions
  - **Output load** (columns) — the total capacitance the cell has to drive
- Example tables for two clock-buffer cell types, `CBUF '1'` and `CBUF '2'`:

  **Delay Table for CBUF '1'**

  | Input slew \ Output load | 10fF | 30fF | 50fF | 70fF | 90fF | 110fF |
  |---|---|---|---|---|---|---|
  | 20ps | x1 | x2 | x3 | x4 | x5 | x6 |
  | 40ps | x7 | x8 | **x9** | **x10** | x11 | x12 |
  | 60ps | x13 | x14 | x15 | x16 | x17 | x18 |
  | 80ps | x19 | x20 | x21 | x22 | x23 | x24 |

  **Delay Table for CBUF '2'**

  | Input slew \ Output load | 10fF | 30fF | 50fF | 70fF | 90fF | 110fF |
  |---|---|---|---|---|---|---|
  | 20ps | y1 | y2 | y3 | y4 | y5 | y6 |
  | 40ps | y7 | y8 | y9 | y10 | y11 | y12 |
  | 60ps | y13 | y14 | y15 | y16 | y17 | y18 |
  | 80ps | y19 | y20 | y21 | y22 | y23 | y24 |

  Each entry (`x1`…`x24`, `y1`…`y24`) is one delay value obtained by **SPICE-characterizing** that specific (slew, load) pair — the same propagation-delay measurement process covered in the earlier cell-characterization notes.

- **Worked buffer-tree example**, using `CBUF '1'` as buffer `1`, and both `Cbuf1`/`Cbuf2` as `CBUF '2'`-type buffers:
  - Signal arrives at buffer `1`'s input with **40 ps slew**
  - Buffer `1`'s output (node **A**) drives **`Cbuf1`** and **`Cbuf2`** in parallel → total load at node A = `Cbuf1 + Cbuf2` = 30 fF + 30 fF = **60 fF**
  - `Cbuf1`'s output (node **B**) drives loads `C1` and `C2` → total load at node B = 25 fF + 25 fF = **50 fF**
  - `Cbuf2`'s output (node **C**) drives loads `C3` and `C4` → total load at node C = 25 fF + 25 fF = **50 fF**
  - Looking up buffer `1`'s delay at **40 ps slew / 60 fF load**: 60 fF sits *between* the 50 fF and 70 fF columns, so the tool interpolates between the neighbouring table entries **`x9`** and **`x10`** (highlighted) rather than reading a single exact cell.
  - Looking up `Cbuf1`/`Cbuf2`'s delay (as `CBUF '2'`) at 50 fF is a direct table hit, since `C1+C2` (and `C3+C4`) land exactly on the 50 fF column.
- Two properties that make this kind of hand-calculation tractable for pre-layout timing estimation:
  - **Observation 1 — identical buffers at the same level**: if `Cbuf1` and `Cbuf2` are the same cell/size, only one delay lookup is needed per level, not one per instance.
  - **Observation 2 — every node at a level drives the same total load**: because `C1=C2=C3=C4=25fF` and `Cbuf1=Cbuf2=30fF`, nodes B and C (and, by symmetry, any other node at that level) all present identical loads — so the whole level's delay can be characterized with a single lookup, rather than re-deriving it node-by-node.
- This lookup-table approach — rows of input slew, columns of output load, one delay value per cell — is exactly the `delay_model : "table_lookup"` structure seen in the real `.lib` file above, just applied here by hand to size and estimate a clock buffer tree before layout even exists.

---

## Key Takeaways

- Magic's `grid` command reports the routing grid pitch/offset; this must be reformatted into OpenLane's `tracks.info` (`layer, direction, offset, pitch`) so a custom cell's pins land on-grid and are actually routable.
- Every pin needs a `port class` (input/output/inout) and `port use` (signal/power/ground) set in Magic before a valid LEF can be exported.
- A cell's LEF (`CLASS CORE`, `SIZE`, `SITE`, per-layer `PIN`/`PORT`/`RECT`) only describes its physical shape — OpenLane's `add_lefs` step merges this LEF into the flow so the custom cell becomes usable in synthesis/placement/routing.
- A LEF alone isn't enough for timing-aware synthesis/STA — the cell also needs a matching Liberty (`.lib`) entry, which is why understanding the `.lib` structure (`delay_model : table_lookup`, `operating_conditions`, `power_lut_template`) matters.
- Delay/power in a `.lib` file is stored as 2D lookup tables indexed by input slew and output load; values between table points are interpolated, not measured directly.
- Pre-layout clock-tree delay estimation relies on: (1) summing fanout capacitances level-by-level to get each node's total load, then (2) looking up (or interpolating) each level's delay from its buffer's delay table — simplified further whenever a level uses identical buffers driving identical loads.
- "Power-aware CTS" introduces clock-gating cells (AND/OR + enable) into specific branches of the clock tree so unused logic can have its clock gated off, trading a small structural change in the tree for dynamic power savings.

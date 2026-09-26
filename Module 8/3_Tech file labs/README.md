#Tech File Labs

##steps to create final SPICE deck using Sky130 tech

- Starting point is the raw extracted netlist, `sky130_inv.spice`, generated earlier from `sky130_inv.ext` by `ext2spice`:
- The schematic mirrors this netlist: a PMOS (`M1000`, `pshort`) pulling `Y` up to `VPWR`/VDD (3.3 V), and an NMOS (`M1001`, `nshort`) pulling `Y` down to `VGND`/VSS — the two gates tied together at input `A`, with the extracted parasitic capacitors (`C0`–`C4`) sitting between the labelled nodes.
- The raw `.subckt` is turned into a runnable **testbench** by editing the deck:
  ```spice
- Key edits made to the deck:
  - `.option scale` changed from `10000u` to `0.01u` to match the model file's units
  - `pshort.lib` / `nshort.lib` **included** so the `pshort`/`nshort` device models actually resolve
  - `.subckt` / `.ends` wrapper **commented out** — the netlist is simulated directly as a top-level circuit, not as a callable subcircuit
  - `VDD`/`VSS` sources added to bias `VPWR` at 3.3 V and `VGND` at 0 V
  - `Va` — a `PULSE` source — drives input `A`, toggling 0 V → 3.3 V
  - `.tran 1n 20n` sets a transient analysis from 0 to 20 ns in 1 ns steps
  - `.control … run … .endc` block runs the simulation automatically when the deck is loaded

---

## steps to characterize inverter using sky130 model files

- The finished deck is opened and simulated directly with ngspice:
  ```bash
  vim sky130_inv.spice
  ngspice sky130_inv.spice
  ```
- ngspice reports the initial operating point before running the transient sweep:
  ```
  Circuit: * spice3 file created from sky130_inv.ext - technology: sky130a
  Doing analysis at TEMP = 27.000000 and TNOM = 27.000000
  Warning: va: no DC value, transient time 0 value used

  Node        Voltage
  ----        -------
  y             3.3
  a             0
  vpwr          3.3
  vgnd          0
  va#branch     0
  vss#branch    3.32412e-12
  vdd#branch   -3.32414e-12

  No. of Data Rows : 160
  ```
- Plotting output vs. input from the ngspice prompt:
  ```
  ngspice 1 -> plot y vs time a
  ```
- The resulting waveform confirms correct **inverter behaviour**: the blue trace (`a`, input) toggles as a clean square pulse, and the red trace (`y`, output) switches to the opposite logic level each time, both swinging between 0 V and 3.3 V.
- This closes the loop: **layout → extraction → SPICE netlist → simulation → verified electrical behaviour**, all using the actual Sky130 device models rather than generic/ideal transistors.

---

## introduction to Magic tool options and DRC rules

- Reference: [opencircuitdesign.com/magic](https://opencircuitdesign.com/magic/)
- **Magic** is a venerable, open-source VLSI layout tool, originally written in the 1980s at Berkeley by **John Ousterhout** — who later became famous for creating the **Tcl** scripting language.
- Distributed under a liberal **BSD-style, Berkeley open-source license**, which is a major reason Magic has stayed popular with universities and small companies for decades.
- Widely cited as **the easiest layout tool to learn**, even among engineers who ultimately rely on commercial tools for their production flow — this is credited to its well-thought-out **core algorithms** (e.g. corner-stitching data structures) more than its (dated) GUI.
- Original/core authors: **Gordon Hamachi, Robert Mayo, John Ousterhout, Walter Scott, George Taylor**; current distribution maintained by **Tim Edwards**.
- The open-source license has let engineer-programmers extend Magic with custom features, helping it keep pace with newer fabrication technologies (deep submicron design rules, LEF/DEF, multi-layer metal stacks, etc.).
- Site sections relevant to this lab: **Using Magic** (command reference/usage docs), **Technology Files** (how to read/write/edit a tech file — directly relevant to the DRC-rule labs later in this module), and **Documentation** (historical papers, tutorials).
- Magic uses **lambda (λ)-based, scalable design rules**, which is why layout dimensions in these labs are reported in both **microns** and **lambda units** side by side.

---

##introduction to Sky130 pdk's and steps to download labs

- Lab exercise: **Magic DRC**, using a pre-packaged DRC test-cell archive.
- Lab contents URL:
  ```
  http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
  ```
- Download and unpack the archive:
  ```bash
  wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
  tar xfz drc_tests.tgz
  ```
- This `drc_tests.tgz` archive contains individual `.mag` test cells (one per design rule, e.g. `poly.mag`, `nwell.mag`, `licon.mag`, `met3.mag`, etc.) — each cell purposely contains both **correct** and **deliberately incorrect** geometry so the DRC engine's behaviour can be checked rule-by-rule, which is exactly what the later labs (`SKY_L6`–`SKY_L9`) work through.
- Broader context — the **Sky130 PDK** these tech-rules come from:
  - The **SkyWater Open Source PDK** (`google/skywater-pdk` on GitHub) is a collaboration between **Google** and **SkyWater Technology Foundry** to provide a fully open-source Process Design Kit for manufacturable chip designs.
  - Targets the **SKY130** process node — a mature **130 nm** (technically a 180 nm–130 nm hybrid) technology, originally developed internally by **Cypress Semiconductor** before SkyWater was spun out.
  - Supports internal **1.8 V** operation with **5.0 V I/Os** (also operable at 2.5 V).
  - Ships with many features as **standard** that are usually optional/paid add-ons at other foundries — including **local interconnect**, SONOS non-volatile memory functionality, and MiM (metal-insulator-metal) capacitors.
  - Full documentation lives at `skywater-pdk.rtfd.io`; the repository was originally released (May 2020) as an **experimental preview/alpha**, though the underlying SKY130 process itself has a long track record of commercial silicon.

---

##introduction to Magic and steps to load Sky130 tech-rules

- Loads the Sky130A technology rule deck into Magic and walks the **Metal 3 (`met3`)** DRC test structures: `m3.1` through `m3.6`.
- Console session shows the bounding box of the loaded test cell, in all three unit systems Magic supports:
  ```
  box
  Root cell box:
      width x height (  llx, lly ), (  urx, ury )  area (units^2)
  microns:  4.06 x 2.86  ( 9.74, 9.21 ), (13.80, 12.07)  11.61
  lambda:   406.00 x 286.00  ( 974.00, 921.00 ), (1380.00, 1207.00)  116116.00
  internal: 812 x 572   ( 1948, 1842 ), ( 2760, 2414 )  464464
  ```
- Selecting a labelled sub-cell and asking Magic why it's flagged:
  ```
  select area
  goto m3.1
  drc why
  Metal3 spacing < 0.9um (met3.2)
  Loading DRC CIF style.
  ```
- Observed labels in the test layout:
  - `m3.1`, `m3.4` — marked **"correct by design"**
  - `m3.2` — the selected structure, flagged for a **Metal3 spacing** violation
  - `m3.3c`, `m3.3d` — marked **"Not implemented"** (no rule check wired up yet for those cases)
  - `m3.5`, `m3.6` — additional metal-3 geometry variants
- Takeaway: Magic's rule checks are looked up per-layer (here `met3.2` = "Metal3 spacing < 0.9 µm"), and a tech file can have rules that are correct, violated, or simply **not yet implemented** — which is exactly the gap the later "find missing/incorrect rules" labs are built to expose.

---

##exercise to fix poly.9 error in Sky130 tech-file

- Working directory listing shows the individual per-rule test cells packaged with the tech file:
  ```
  ccaps.mag   diffttap.mag   drwell.mag   hvtp.mag   hvte4.mag   licon.mag   lvtn.mag
  rcon.mag    mel1.mag       mel2.mag     mel3.mag   mel4.mag    mpc.mag     nsd.mag
  nwell.mag   pad.mag        poly.mag     psd.mag    rpm.mag     sky130A.tech turen.mag varac.mag
  via.mag     via2.mag       via3.mag     via4.mag
  ```
- Loads the poly-layer test cell:
  ```
  load poly
  what
  Selected mask layers:
      poly  ( Topmost cell in the window )
  ```
- The layout view highlights three poly resistor strips sitting over a diffusion/box region, labelled **"Incorrect poly.9"** — i.e. the `poly.9` rule (poly-resistor-to-diffusion or poly-resistor-to-tap spacing) is failing on this geometry as drawn.
- Fixing `poly.9` means correcting the **spacing between the poly resistor and the surrounding diffusion/tap** so it meets the minimum spacing defined in the Sky130 design rules, then re-running `drc check`/`drc why` to confirm the violation clears.

---

##Lab exercise to implement poly resistor spacing to diff and tap

- Extends the previous lab across a full row of poly-resistor test structures: `poly.7`, `poly.8`, `poly.9`, `poly.10`, `poly.11`.
- Bounding-box query on the loaded cell:
  ```
  box
  Root cell box:
      width x height (  llx, lly ), (  urx, ury )  area (units^2)
  microns:   5.68 x 5.39  ( 16.59, 1.99 ), ( 22.28, 7.19 )   30.67
  lambda:    568.50 x 539.50 ( 1650.50, 189.50 ), ( 2228.00, 729.00 )  306705.75
  internal:  1137 x 1079 ( 3301, 379 ), ( 4456, 1458 )  1226823
  ```
- Loading the correct technology file and re-running the check:
  ```
  tech load sky130A.tech
  Input style sky130: scaleFactor=2, multiplier=2
  Scaled tech values by 2 / 1 to match internal grid scaling
  Existing layout may be invalid.

  drc check
  Loading DRC CIF style.
  drc why
  nrpl resistor width < 0.33um (poly.3)
  nhrpoly/nhrpoly resister spacing to diffusion < 0.48um (poly.9)
  poly.resistor spacing to N-tap < 0.48um (poly.9)
  ```
- Two related **poly.9** conditions are being checked at once:
  1. **poly-resistor ↔ diffusion spacing** must be ≥ 0.48 µm
  2. **poly-resistor ↔ N-tap spacing** must be ≥ 0.48 µm
- `poly.7` and `poly.8` are drawn correctly (shown with a passing green highlight); `poly.9` fails both spacing checks; `poly.10` and `poly.11` show further correct/variant geometry for comparison.
- Exercise goal: widen/move the poly-resistor geometry in the failing cell so both the diffusion-spacing and tap-spacing rules pass simultaneously — a classic case of one drawn shape needing to satisfy **two independent DRC rules** at once.

---

##Lab challenge exercise to describe DRC error as geometrical construct

- Loads an **N-well** DRC test cell showing one small green square nested concentrically inside a larger green square, flagged as **`nwell.6`**.
- This nested-square construction is Magic's standard way of expressing a **minimum spacing / enclosure rule geometrically**: the gap between the inner and outer square boundaries directly encodes the minimum required spacing (or enclosure) distance for that rule.
- Console trace of the DRC engine loading and scaling the tech file:
  ```
  tech load sky130A.tech
  Input style sky130: scaleFactor=2, multiplier=2
  Scaled tech values by 2 / 1 to match internal grid scaling
  Existing layout may be invalid.

  drc check
  Loading DRC CIF style.
  load nwell
  cif ostyle drc
  CIF output style is now "drc"
  cif see drwell_shrink
  ```
- The **challenge**: given only the drawn geometry (two nested squares) and no rule text, describe in words what geometric/DRC rule is actually being tested (e.g. "N-well to N-well spacing" or "N-well enclosure of some layer"), purely by reading the shapes and their separation — reinforcing that every DRC rule ultimately reduces to a **geometric construct** (a width, a spacing, an enclosure, or an extension) that can be drawn and measured directly in the layout.

---

##Lab challenge to find missing or incorrect rules and fix them

- Loads another N-well test structure, `nwell.4`, explicitly flagged with the on-screen label **"ERROR: Incorrect Implementation."**
- Console session inspects the same bounding box twice (before/after an edit attempt), and Magic reports a configuration hint at the end:
  1. Identify **which rule in the Sky130 tech file is missing or wrongly written** by comparing the drawn "should pass"/"should fail" geometry against the DRC engine's actual (incorrect) verdict.
  2. Edit the tech-file rule definition itself so the DRC check matches the intended design rule.
  3. Re-run `drc check` / `drc why` to confirm the fix, exactly as done in the earlier `poly.9` and `met3` labs.
- The `"joinNets" switch should be turned on` hint is a good example of the kind of subtle **tech-file/DRC configuration** issue (as opposed to a simple spacing number) that this lab is designed to surface — some rules only evaluate correctly once related nets are electrically joined for the purposes of connectivity-based DRC checks.

---

## Key Takeaways

- A raw `ext2spice` netlist is not directly simulatable — it must be turned into a testbench by adding model includes, bias sources, an input stimulus, and a `.control` block.
- ngspice confirms correct inverter behaviour by plotting output (`y`) against input (`a`) and observing clean, full-swing (0 V–3.3 V) inversion.
- **Magic**, written by John Ousterhout (later of Tcl fame) under a BSD-style license, remains one of the easiest and most widely used open-source layout/DRC tools, thanks to strong core algorithms rather than a modern UI.
- The **Sky130 PDK** (Google × SkyWater) is a fully open-source 130 nm-class process, notable for including local interconnect, SONOS memory, and MiM capacitors as standard features.
- Magic reports geometry simultaneously in **microns, lambda units, and internal grid units** — useful for cross-checking DRC rule values against a technology's real physical dimensions.
- DRC rules in a tech file can be **correct, violated, or simply unimplemented** — all three states show up across these labs (`met3` test cells being the clearest example).
- Every DRC rule — spacing, width, enclosure — ultimately reduces to a **geometric construct** that can be drawn, measured, and debugged directly in the layout, which is the core skill these tech-file labs are built to teach.

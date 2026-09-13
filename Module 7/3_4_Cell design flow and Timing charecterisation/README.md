# Cell Design Flow

## Recap: Standard-Cell Flavours

A **standard-cell library** contains cells implementing different digital functions.

Different sizes are provided mainly to obtain different:

- Drive strengths
- Delays
- Area
- Power characteristics
- Timing performance

There can also be different threshold-voltage flavours, such as:

- **LVT** → Low Threshold Voltage
- **SVT** → Standard Threshold Voltage
- **HVT** → High Threshold Voltage

Thus, a standard-cell library is essentially a collection of physical cells with different functionalities and different performance/size options.

---

# Cell Design Flow

The purpose of cell design is to create a physical standard cell and generate the models/files required for using that cell in an ASIC design flow.

The flow can be divided into:

```
Inputs
   ↓
Design Steps
   ↓
Outputs
```

## 1. Inputs to Cell Design

The major inputs shown in the flow are the Process Design Kits (PDKs) and design specifications.

### PDK

A **Process Design Kit (PDK)** contains technology-specific information required to design and verify cells.

Important PDK contents include:

- DRC rules
- LVS rules
- SPICE models
- Technology information
- Layer information
- Device information

### SPICE Models

SPICE models describe the electrical behaviour of semiconductor devices such as:

- NMOS
- PMOS

They contain device parameters required for circuit simulation.

The screenshot shows model definitions containing parameters such as:

- VT
- KP
- W
- L
- CJ
- CGDO
- CGSO
- ...

These models are used for transistor-level simulation.

### Library and User-Defined Specifications

The cell designer also needs:

- Standard-cell library information
- Cell functionality
- Required drive strength
- Timing requirements
- Power requirements
- Physical design constraints

## 2. Design Steps

The main design steps shown are:

1. Circuit Design
2. Layout Design
3. Characterization

### Step 1 – Circuit Design

First, the required logic function is designed at the transistor level.

For example:

```
Logic function
      ↓
PMOS network
      +
NMOS network
      ↓
CMOS circuit
```

For CMOS combinational circuits:

- PMOS network forms the pull-up network.
- NMOS network forms the pull-down network.

The circuit must implement the required Boolean function correctly.

#### Euler's Path

During CMOS layout design, Euler's path is useful for arranging transistors to minimize diffusion breaks.

The PMOS and NMOS networks are represented as graphs and an Euler path is identified.

A suitable Euler ordering allows transistors with common diffusion to be placed next to each other.

Example from the shown design:

Euler ordering:

```
A - C - E - F - D - B
```

The same ordering can be used to construct the physical transistor layout efficiently.

#### Stick Diagram

A stick diagram is a simplified representation of a layout.

It represents:

- Metal
- Polysilicon
- Diffusion
- Contacts

without showing exact dimensions.

It is used as an intermediate step before actual layout.

### Step 2 – Layout Design

After designing the circuit, the transistor-level circuit is converted into a physical layout.

The layout contains actual geometric shapes representing:

- Diffusion
- Poly
- Metal layers
- Contacts
- Vias

The layout must satisfy the technology's DRC rules.

Typical sequence:

```
Circuit Design
      ↓
Euler Path
      ↓
Stick Diagram
      ↓
Layout
      ↓
DRC
      ↓
LVS
```

The final layout is therefore a manufacturable geometric representation of the circuit.

## 3. Outputs of Cell Design

The screenshots show the following important outputs:

- CDL
- GDSII
- LEF
- Extracted SPICE Netlist (.cir)
- Timing Libraries
- Noise Libraries
- Power Libraries
- Functionality Information

### CDL

**CDL – Circuit Description Language**

It represents the transistor-level circuit/netlist of the cell.

It is useful for:

- LVS
- Circuit verification
- Netlist exchange

### GDSII

GDSII is the physical layout database used to represent the geometrical layout of the cell.

It contains information about:

- Shapes
- Layers
- Coordinates
- Physical geometry

It is ultimately used in the physical design/manufacturing flow.

```
Layout
  ↓
GDSII
  ↓
Physical design / fabrication flow
```

### LEF

**LEF – Library Exchange Format**

LEF provides the physical abstract information of a cell.

It can describe things such as:

- Cell dimensions
- Pin locations
- Pin layers
- Routing information
- Obstructions

Physical-design tools use LEF without needing the complete detailed transistor geometry.

### Extracted SPICE Netlist

The layout can be extracted back into a SPICE representation.

```
Layout
   ↓
Extraction
   ↓
Extracted SPICE netlist
```

This allows post-layout simulation, including parasitic effects.

### Characterization Libraries

Characterization generates library information describing how the cell behaves.

The outputs include:

- Timing library
- Noise library
- Power library
- Functionality information

These models are used during ASIC implementation and timing analysis.

## 4. Eight-Step Cell Characterization Flow

The overall characterization process can be viewed as an eight-step flow:

1. Define / prepare the transistor-level circuit
2. Create the required SPICE simulation setup
3. Apply input stimulus
4. Perform circuit simulation
5. Measure delay and transition characteristics
6. Extract power/noise related characteristics
7. Generate characterized library models
8. Validate and deliver the final cell models

The essential idea is:

```
Circuit + PDK
      ↓
SPICE Simulation
      ↓
Measurements
      ↓
Characterization
      ↓
Timing / Power / Noise Models
```

---

# GUNA

## What is GUNA?

GUNA is a cell-characterization tool used to characterize standard cells.

It takes the cell description, simulation information and required characterization conditions and generates the library models needed for digital implementation.

---

# Key Points to Remember

1. Standard-cell libraries contain cells of different functionality and different physical/performance sizes.

2. PDK provides technology information such as DRC/LVS rules and SPICE models.

3. Cell design has three major stages: Circuit design → Layout design → Characterization.

4. Euler's path helps arrange transistors and reduce diffusion breaks.

5. Stick diagrams are simplified representations used before creating the actual layout.

6. DRC checks whether the layout follows manufacturing rules.

7. LVS checks whether the layout matches the intended circuit.

8. Characterization determines timing, power, noise and functional behaviour of the cell.

9. Major cell-design outputs include: CDL, GDSII, LEF and extracted SPICE netlist.

10. Timing, power and noise libraries are generated from characterization results.

11. GUNA is the characterization tool used to generate these standard-cell library models.

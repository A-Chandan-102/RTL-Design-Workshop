# Class 1 – Basics and Environment Setup

## Overview

The first class introduced the basic workflow and environment used throughout the RTL Design and Synthesis Workshop. The focus was on setting up the required tools, working with GitHub Codespaces, running simple Verilog simulations, viewing waveforms, and inspecting a GDS layout.

---

## 1. GitHub Basics

GitHub was introduced as the platform used to store, manage, and access the workshop repository.

### Basic workflow

```bash
git clone <repository-url>
cd sky130RTLDesignAndSynthesisWorkshop
```

GitHub was used for:

- Repository management
- Accessing RTL and simulation files
- Maintaining workshop work
- Working with GitHub Codespaces

---

## 2. GitHub Codespaces

GitHub Codespaces was used as the development environment for the workshop.

The basic workflow was:

```text
GitHub Repository
        ↓
Create Codespace
        ↓
Wait for environment setup
        ↓
Open terminal / VS Code
        ↓
Work with RTL files and tools
```

The Codespaces environment provided a Linux-based workspace where the required design and simulation tools could be executed.

---

## 3. Basic Linux Terminal Commands

Basic terminal navigation and file handling were used inside Codespaces.

```bash
pwd
ls
cd <directory>
cd ..
```

These commands were used to navigate the workshop repository and locate Verilog files, libraries, scripts, and generated simulation files.

---

## 4. Installing and Checking Prerequisites

The initial environment setup included checking that the required tools were available.

The main tools introduced were:

```text
Icarus Verilog
GTKWave
Yosys
KLayout
Gvim / Vim
```

The tools are used at different stages of the RTL-to-silicon workflow:

```text
RTL Design
   ↓
Icarus Verilog
   ↓
Simulation
   ↓
GTKWave
   ↓
Yosys
   ↓
Synthesis
   ↓
Netlist / Layout files
   ↓
KLayout
```

---

## 5. Basic Icarus Verilog Simulation

Icarus Verilog was introduced for compiling and simulating Verilog designs.

A basic simulation flow was:

```bash
iverilog -o mux verilog_files/tb_good_mux.v
```

The generated simulation executable was then run using:

```bash
./a.out
```

or, depending on the output name:

```bash
./mux
```

The simulation generated a VCD waveform file using `$dumpfile` and `$dumpvars` from the testbench.

---

## 6. GTKWave

GTKWave was introduced for viewing simulation waveforms stored in VCD files.

```bash
gtkwave tb_good_mux.vcd
```

This made it possible to verify that the simulated output changed according to the inputs and select signal.

---

## 7. Understanding a Basic RTL Module

A simple multiplexer was used to understand the structure of a Verilog design.

The example demonstrated:

- Module declaration
- Input and output ports
- `always @(*)`
- Conditional `if-else`
- Assigning the selected input to the output

The same RTL could then be compiled, simulated, and observed in GTKWave.

---

## 8. Working with the Workshop Repository

The repository contained RTL source files, testbenches, libraries, scripts, and generated files.

The terminal was used to enter the required directory and inspect its contents:

```bash
cd sky130RTLDesignAndSynthesisWorkshop
ls
```

---

## 9. Running Workshop Scripts

The class also introduced the use of shell scripts provided with the workshop.

For example:

```bash
./yosys_run.sh
```

## 10. Checking a GDS File

A GDS file represents the physical layout information of an integrated circuit.

The GDS file was opened using **KLayout** to inspect the layout.

```text
GDS file
   ↓
KLayout
   ↓
Physical layout visualization
```

The layout view showed different layers and physical structures of the chip, providing an initial introduction to the physical-design side of the RTL-to-GDS flow.

---

## 11. Basic Compilation Workflow

The basic workflow established in the first class was:

```text
Write / obtain Verilog RTL
        ↓
Compile using Icarus Verilog
        ↓
Run simulation
        ↓
Generate VCD waveform
        ↓
Open VCD in GTKWave
        ↓
Verify signal behaviour
```

This workflow became the foundation for the later synthesis and gate-level simulation labs.

---

## 12. Basic C Compilation

A simple C program was also compiled and executed while becoming familiar with the Codespaces/Linux environment.

```bash
cc -o sum1ton.o sum1ton.c
./sum1ton.o
```

The program produced:

```text
Sum from 1 to 9 is 45
```

## Tools Used

```text
GitHub
GitHub Codespaces
Linux Terminal
VS Code
Icarus Verilog
GTKWave
Yosys
Gvim / Vim
KLayout
```

---

## Key Takeaways

- Learned the basic GitHub workflow for accessing workshop repositories.
- Learned how to create and use a GitHub Codespace.
- Practiced basic Linux terminal navigation and file handling.
- Learned the basic Icarus Verilog simulation flow.
- Used GTKWave to inspect generated VCD waveforms.
- Understood the basic structure of a Verilog module and testbench workflow.
- Learned how GDS files can be inspected using KLayout.
- Understood the initial RTL-to-simulation-to-synthesis workflow used in the workshop.

---

## Learning Outcomes

After completing the class, I was able to:

- Work with the workshop repository through GitHub and Codespaces.
- Navigate the Linux-based development environment.
- Compile and simulate simple Verilog designs using Icarus Verilog.
- Generate and inspect VCD waveforms using GTKWave.
- Recognize the role of Yosys in the synthesis flow.
- Open and inspect GDS layout files using KLayout.
- Understand the basic workflow that connects RTL design, simulation, synthesis, and physical layout.

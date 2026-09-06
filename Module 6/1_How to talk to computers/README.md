# Module 6 – How to Talk to Computers

## 1. From Software to Hardware

A computer system can be understood as a chain that converts high-level software into physical hardware operations.

```text
Application Software
        ↓
System Software / Operating System
        ↓
Compiler
        ↓
Machine Instructions
        ↓
Assembler
        ↓
Binary Instructions
        ↓
Processor / Hardware
```

### How it works

- **Application software** provides the functionality required by the user, such as a stopwatch application.
- **System software** manages hardware resources, memory, I/O operations and low-level system functions.
- A **compiler** converts programs written in languages such as C, C++ or Java into lower-level instructions.
- The **assembler** converts assembly instructions into machine-readable binary instructions.
- The processor finally executes these instructions using digital hardware.

This creates the connection:

```text
Software → Instructions → Processor → Digital Hardware
```

---

## 2. What is RISC-V?

**RISC-V** is an open standard **Instruction Set Architecture (ISA)**.

An ISA defines the instructions and programmer-visible behaviour that a processor understands.

For example:

```text
add x6, x10, x6
```

is a RISC-V instruction that tells the processor to add the contents of registers `x10` and `x6` and place the result in `x6`.

### RISC-V Architecture

RISC-V acts as the interface between software and the processor hardware:

```text
Software
   ↓
RISC-V Instructions
   ↓
RISC-V CPU Implementation
   ↓
Digital Hardware
```

RISC-V is based on the **Reduced Instruction Set Computer (RISC)** approach, using a relatively simple and regular instruction set.

---

## 3. RISC-V and CPU Implementation

An ISA by itself is not the physical processor.

The implementation is created using RTL code that describes hardware capable of understanding and executing the ISA.

```text
RISC-V ISA
    ↓
RTL implementation
    ↓
Synthesis
    ↓
Gate-level Netlist
    ↓
Physical Design
    ↓
Fabricated Chip
```

In the workshop, the **picorv32** core was used as an example of a RISC-V CPU implementation.

---

## 4. Three Major Stages

The complete flow can be viewed in three major parts:

### Part 1 – RISC-V Instruction Set Architecture

Defines the instruction set and the architecture understood by the processor.

```text
Instruction
    ↓
Example: add x6, x10, x6
```

### Part 2 – RTL and Synthesis

The RISC-V processor is described using RTL and then synthesised into a gate-level representation.

```text
RISC-V ISA
    ↓
RTL
    ↓
Synthesis
    ↓
Gate-level Netlist
```

### Part 3 – Physical Design

The synthesised netlist is implemented physically using standard cells and other chip components.

```text
Gate-level Netlist
        ↓
Physical Design
        ↓
Layout
        ↓
Hardware
```

---

## 5. Chip Structure

A complete chip can contain several different types of blocks:

- **Processor / SoC**
- **Macros**
- **Foundry IPs**
- **Memory such as SRAM**
- **I/O and peripheral blocks**
- **Power and ground connections**

A physical chip therefore contains much more than just the processor core.

---

## 6. Key Learning Outcomes

- I understood how software ultimately interacts with digital hardware.
- I understood the role of compilers and assemblers in converting software into machine instructions.
- I learned that **RISC-V is an ISA**, not a specific processor implementation.
- I understood the relationship between **ISA, RTL, synthesis, netlist and physical design**.
- I learned how a RISC-V CPU such as **picorv32** can progress from an instruction set to an actual chip implementation.

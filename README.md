# RTL Design Workshop

Welcome to my RTL Design Workshop repository, where I document my journey of learning RTL coding, simulation, testbench development, and waveform-based debugging of digital circuits. All the data is ordered according to modules learnt.

## 🎯 Main Objective

The goal of this workshop is to build a solid, hands-on understanding of the RTL-to-GDS front-end flow — starting from writing and simulating Verilog RTL designs, moving through logic synthesis and standard-cell mapping using the open-source **SKY130 PDK**, and finally validating designs through **Gate-Level Simulation (GLS)**. Along the way, the focus is on understanding coding-style pitfalls (blocking vs non-blocking assignments, incomplete if/case constructs), synthesis optimizations (constant propagation, cloning, retiming), and how RTL design choices translate into real synthesized hardware.

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **Icarus Verilog (iverilog)** | RTL simulation / compiling Verilog testbenches |
| **GTKWave** | Viewing simulation waveforms ( files) |
| **Yosys** | Logic synthesis and standard-cell mapping |
| **SKY130 PDK** () | Open-source standard-cell library used for synthesis |
| **Graphviz / Yosys Dot Viewer** | Visualizing synthesized netlists ( graphs) |
| **GVim** | Editing RTL and testbench source files |
| **Git & GitHub** | Version control and documentation |

## 📂 Repository Structure
*Each folder below contains its own README.md with detailed notes and screenshots for that topic.*
- 📁 **[Classwork 1/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Classwork%201)**
  - 📄 [basic commands.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/basic%20commands.jpeg)
  - 📄 [cloned repository.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/cloned%20repository.jpeg)
  - 📄 [Codespaces.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/Codespaces.jpeg)
  - 📄 [github repository.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/github%20repository.jpeg)
  - 📄 [gtkwave.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/gtkwave.jpeg)
  - 📄 [KLayout.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/KLayout.jpeg)
  - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/README.md)
  - 📄 [terminal.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%201/terminal.jpeg)
- 📁 **[Classwork 2 & 3/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Classwork%202%20%26%203)**
  - 📄 [latch layout diagram.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%202%20%26%203/latch%20layout%20diagram.jpeg)
  - 📄 [Mux_error.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%202%20%26%203/Mux_error.jpeg)
  - 📄 [presynth_sim.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%202%20%26%203/presynth_sim.png)
  - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%202%20%26%203/README.md)
  - 📄 [RV_to_DAC.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%202%20%26%203/RV_to_DAC.png)
  - 📄 [Source files.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%202%20%26%203/Source%20files.jpeg)
- 📁 **[Classwork 4 & 5/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Classwork%204%20%26%205)**
  - 📄 [BABYSOC.jpeg](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/BABYSOC.jpeg)
  - 📄 [BABYSOC2.jpeg.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/BABYSOC2.jpeg.png)
  - 📄 [babysoc_LAYOUT.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/babysoc_LAYOUT.png)
  - 📄 [cells in good mux.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/cells%20in%20good%20mux.png)
  - 📄 [Comparision of good_mux with and without synthesis.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/Comparision%20of%20good_mux%20with%20and%20without%20synthesis.png)
  - 📄 [goodmux_layout.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/goodmux_layout.png)
  - 📄 [Post_Synth_babysoc.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/Post_Synth_babysoc.png)
  - 📄 [presynth_babysoc.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/presynth_babysoc.png)
  - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Classwork%204%20%26%205/README.md)
- 📁 **[Module 1/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%201)**
  - 📁 **[1Introduction/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%201/1Introduction)**
    - 📄 [iverilog flowchart.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/1Introduction/iverilog%20flowchart.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/1Introduction/README.md)
    - 📄 [testbench flowchat.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/1Introduction/testbench%20flowchat.png)
  - 📁 **[Introduction to Yosys and Logic synthesis/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis)**
    - 📄 [fast vs slow.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/fast%20vs%20slow.png)
    - 📄 [flavours of gate.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/flavours%20of%20gate.png)
    - 📄 [lib.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/lib.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/README.md)
    - 📄 [RTL desing.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/RTL%20desing.png)
    - 📄 [Synthesis illustration.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Synthesis%20illustration.png)
    - 📄 [Synthesis.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Synthesis.png)
    - 📄 [Synthesizer flowchart.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Synthesizer%20flowchart.png)
    - 📄 [Verify synthesis.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Verify%20synthesis.png)
    - 📄 [Yosys flowchart.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Yosys%20flowchart.png)
  - 📁 **[Lab1 - Environment Setup/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%201/Lab1%20-%20Environment%20Setup)**
    - 📄 [git instalation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab1%20-%20Environment%20Setup/git%20instalation.png)
    - 📄 [gtkwave install.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab1%20-%20Environment%20Setup/gtkwave%20install.png)
    - 📄 [gvim installation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab1%20-%20Environment%20Setup/gvim%20installation.png)
    - 📄 [iverilog installation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab1%20-%20Environment%20Setup/iverilog%20installation.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab1%20-%20Environment%20Setup/README.md)
  - 📁 **[Lab2 - Icarus Verilog, GTKWave and Gvim/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%201/Lab2%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim)**
    - 📄 [good mux code.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab2%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/good%20mux%20code.png)
    - 📄 [good_mux waveform.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab2%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/good_mux%20waveform.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab2%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/README.md)
  - 📁 **[Lab3 - Using Yosys/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%201/Lab3%20-%20Using%20Yosys)**
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/README.md)
    - 📄 [Screenshot 2026-08-08 232323osys netlist.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/Screenshot%202026-08-08%20232323osys%20netlist.png)
    - 📄 [Yosis dot viewer.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/Yosis%20dot%20viewer.png)
    - 📄 [yosis.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/yosis.png)
    - 📄 [yosys netlist.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/yosys%20netlist.png)
    - 📄 [yosys vs gvim.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/yosys%20vs%20gvim.png)
    - 📄 [yosys2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/yosys2.png)
    - 📄 [yosys3.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%201/Lab3%20-%20Using%20Yosys/yosys3.png)
- 📁 **[Module 2/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%202)**
  - 📁 **[Flip Flop coding styles/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%202/Flip%20Flop%20coding%20styles)**
    - 📄 [asynchronous code.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Flip%20Flop%20coding%20styles/asynchronous%20code.png)
    - 📄 [asynchronous reset.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Flip%20Flop%20coding%20styles/asynchronous%20reset.png)
    - 📄 [Mix of sync and async.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Flip%20Flop%20coding%20styles/Mix%20of%20sync%20and%20async.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Flip%20Flop%20coding%20styles/README.md)
    - 📄 [synchronous.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Flip%20Flop%20coding%20styles/synchronous.png)
    - 📄 [why flops are used.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Flip%20Flop%20coding%20styles/why%20flops%20are%20used.png)
  - 📁 **[Lab4 - Library walkthrough/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%202/Lab4%20-%20Library%20walkthrough)**
    - 📄 [and power.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/and%20power.png)
    - 📄 [comparision.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/comparision.png)
    - 📄 [differnet flavours of and.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/differnet%20flavours%20of%20and.png)
    - 📄 [Exploration of lib.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/Exploration%20of%20lib.png)
    - 📄 [PVT.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/PVT.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/README.md)
    - 📄 [Starting of cell.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab4%20-%20Library%20walkthrough/Starting%20of%20cell.png)
  - 📁 **[Lab5 -  Hier and flat synth/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth)**
    - 📄 [cmos nand.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/cmos%20nand.png)
    - 📄 [direct instanteation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/direct%20instanteation.png)
    - 📄 [flatten whole_netlist_insideu1 and u2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/flatten%20whole_netlist_insideu1%20and%20u2.png)
    - 📄 [multiple_modules dotviewer.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/multiple_modules%20dotviewer.png)
    - 📄 [multiple_modules nand.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/multiple_modules%20nand.png)
    - 📄 [multiple_modules.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/multiple_modules.png)
    - 📄 [multiple_modules2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/multiple_modules2.png)
    - 📄 [multiple_modules3.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/multiple_modules3.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/README.md)
    - 📄 [synth -top.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab5%20-%20%20Hier%20and%20flat%20synth/synth%20-top.png)
  - 📁 **[Lab6 - Flipflop synthesis and simulation/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation)**
    - 📄 [async dotlib changed.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/async%20dotlib%20changed.png)
    - 📄 [async dotlib.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/async%20dotlib.png)
    - 📄 [crosscheck.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/crosscheck.png)
    - 📄 [files used.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/files%20used.png)
    - 📄 [lab_asynch ff.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/lab_asynch%20ff.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/README.md)
    - 📄 [sync and async.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/sync%20and%20async.png)
    - 📄 [tb_async.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/tb_async.png)
  - 📁 **[Lab7 - Intresting optimisations/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%202/Lab7%20-%20Intresting%20optimisations)**
    - 📄 [gvim crosscheck of mul2_net.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab7%20-%20Intresting%20optimisations/gvim%20crosscheck%20of%20mul2_net.png)
    - 📄 [mult2 dotviewer.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab7%20-%20Intresting%20optimisations/mult2%20dotviewer.png)
    - 📄 [mult2 explanation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab7%20-%20Intresting%20optimisations/mult2%20explanation.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab7%20-%20Intresting%20optimisations/README.md)
    - 📄 [Screenshot 2026-08-09 183302.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%202/Lab7%20-%20Intresting%20optimisations/Screenshot%202026-08-09%20183302.png)
- 📁 **[Module 3/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%203)**
  - 📁 **[1_Introduction to Optimisation/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%203/1_Introduction%20to%20Optimisation)**
    - 📄 [Boolean logic op.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/Boolean%20logic%20op.png)
    - 📄 [combinational logic optimisation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/combinational%20logic%20optimisation.png)
    - 📄 [constant logic op.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/constant%20logic%20op.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/README.md)
    - 📄 [sequential constant.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/sequential%20constant.png)
    - 📄 [Sequential logic optimisation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/Sequential%20logic%20optimisation.png)
    - 📄 [State, cloning and retiming.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/1_Introduction%20to%20Optimisation/State%2C%20cloning%20and%20retiming.png)
  - 📁 **[2_Optimisation/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%203/2_Optimisation)**
    - 📄 [opt_check.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/2_Optimisation/opt_check.png)
    - 📄 [opt_check2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/2_Optimisation/opt_check2.png)
    - 📄 [opt_check3.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/2_Optimisation/opt_check3.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/2_Optimisation/README.md)
  - 📁 **[3_Sequential optimisation techniques/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%203/3_Sequential%20optimisation%20techniques)**
    - 📄 [dff_const1.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/3_Sequential%20optimisation%20techniques/dff_const1.png)
    - 📄 [dffconst1_dotviewer.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/3_Sequential%20optimisation%20techniques/dffconst1_dotviewer.png)
    - 📄 [dffconst3.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/3_Sequential%20optimisation%20techniques/dffconst3.png)
    - 📄 [dffconst3_dotviewer.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/3_Sequential%20optimisation%20techniques/dffconst3_dotviewer.png)
    - 📄 [dffconst_2 dotviewer.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/3_Sequential%20optimisation%20techniques/dffconst_2%20dotviewer.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/3_Sequential%20optimisation%20techniques/README.md)
  - 📁 **[4_Unused output optimisation/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%203/4_Unused%20output%20optimisation)**
    - 📄 [Changed_counter_opt.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/4_Unused%20output%20optimisation/Changed_counter_opt.png)
    - 📄 [combinational logic optimisation.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/4_Unused%20output%20optimisation/combinational%20logic%20optimisation.png)
    - 📄 [constant logic op.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/4_Unused%20output%20optimisation/constant%20logic%20op.png)
    - 📄 [counter_opt.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/4_Unused%20output%20optimisation/counter_opt.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%203/4_Unused%20output%20optimisation/README.md)
- 📁 **[Module 4/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%204)**
  - 📁 **[1_Intro to GLS & Synthesis simulation mismatches/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches)**
    - 📄 [Blocking and non-blocking.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/Blocking%20and%20non-blocking.png)
    - 📄 [caveat with blocking statements.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/caveat%20with%20blocking%20statements.png)
    - 📄 [GLS using iverilog.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/GLS%20using%20iverilog.png)
    - 📄 [GLS.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/GLS.png)
    - 📄 [Missing sensitivity list.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/Missing%20sensitivity%20list.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/README.md)
    - 📄 [Syth simulation mismatch.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/Syth%20simulation%20mismatch.png)
    - 📄 [What we are trying to do with GLS.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/1_Intro%20to%20GLS%20%26%20Synthesis%20simulation%20mismatches/What%20we%20are%20trying%20to%20do%20with%20GLS.png)
  - 📁 **[2_GLS | Synth sim | Mismatch LAB/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB)**
    - 📄 [bad_mux.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB/bad_mux.png)
    - 📄 [gls_bad_mux.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB/gls_bad_mux.png)
    - 📄 [gls_ternary_op.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB/gls_ternary_op.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB/README.md)
    - 📄 [synth_netlist_ternary_op.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB/synth_netlist_ternary_op.png)
    - 📄 [ternary_operator.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/2_GLS%20%7C%20Synth%20sim%20%7C%20Mismatch%20LAB/ternary_operator.png)
  - 📁 **[3_Blocking caveat/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%204/3_Blocking%20caveat)**
    - 📄 [blocking_caveat.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/3_Blocking%20caveat/blocking_caveat.png)
    - 📄 [blocking_caveat_netlist.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/3_Blocking%20caveat/blocking_caveat_netlist.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/3_Blocking%20caveat/README.md)
    - 📄 [synth_mismatch_blocking_caveat.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%204/3_Blocking%20caveat/synth_mismatch_blocking_caveat.png)
- 📁 **[Module 5/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%205)**
  - 📁 **[1_If Case constructs/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%205/1_If%20Case%20constructs)**
    - 📄 [Case statement.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/1_If%20Case%20constructs/Case%20statement.png)
    - 📄 [Dangers of case1.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/1_If%20Case%20constructs/Dangers%20of%20case1.png)
    - 📄 [Dangers of case2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/1_If%20Case%20constructs/Dangers%20of%20case2.png)
    - 📄 [Dangers of if.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/1_If%20Case%20constructs/Dangers%20of%20if.png)
    - 📄 [if statement.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/1_If%20Case%20constructs/if%20statement.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/1_If%20Case%20constructs/README.md)
  - 📁 **[2_If statements/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%205/2_If%20statements)**
    - 📄 [incomp_if.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/2_If%20statements/incomp_if.png)
    - 📄 [incomp_if2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/2_If%20statements/incomp_if2.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/2_If%20statements/README.md)
    - 📄 [synth_incomp_if.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/2_If%20statements/synth_incomp_if.png)
    - 📄 [synth_incomp_if2.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/2_If%20statements/synth_incomp_if2.png)
  - 📁 **[3_case_statements/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%205/3_case_statements)**
    - 📄 [bad_case.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/bad_case.png)
    - 📄 [comp_case.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/comp_case.png)
    - 📄 [gls_bad_case.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/gls_bad_case.png)
    - 📄 [incomp_case.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/incomp_case.png)
    - 📄 [partial_case.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/partial_case.png)
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/README.md)
    - 📄 [synth_compcase.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/synth_compcase.png)
    - 📄 [synth_incomp_case.png](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/3_case_statements/synth_incomp_case.png)
  - 📁 **[4_loop constraints/](https://github.com/A-Chandan-102/RTL-Design-Workshop/tree/main/Module%205/4_loop%20constraints)**
    - 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/Module%205/4_loop%20constraints/README.md)
- 📄 [README.md](https://github.com/A-Chandan-102/RTL-Design-Workshop/blob/main/README.md)

---


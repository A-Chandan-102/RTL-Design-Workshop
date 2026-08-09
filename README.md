# RTL Design Workshop
Welcome to my RTL Design Workshop repository, where I will document my journey of learning RTL coding, simulation, testbench development, and waveform-based debugging of digital circuits. All the data is ordered according to modules learnt

## Repository Structure

## Repository Tree

```text
RTL-Design-Workshop/
│
├── Module 1/
│   │
│   ├── [1Introduction](Module%201/1Introduction/)
│   │   ├── [README.md](Module%201/1Introduction/README.md)
│   │   ├── [iverilog flowchart.png](Module%201/1Introduction/iverilog%20flowchart.png)
│   │   └── [testbench flowchat.png](Module%201/1Introduction/testbench%20flowchat.png)
│   │
│   ├── [Introduction to Yosys and Logic synthesis](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/)
│   │   ├── [README.md](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/README.md)
│   │   ├── [RTL desing.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/RTL%20desing.png)
│   │   ├── [Synthesis illustration.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Synthesis%20illustration.png)
│   │   ├── [Synthesis.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Synthesis.png)
│   │   ├── [Synthesizer flowchart.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Synthesizer%20flowchart.png)
│   │   ├── [Verify synthesis.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Verify%20synthesis.png)
│   │   ├── [Yosys flowchart.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/Yosys%20flowchart.png)
│   │   ├── [fast vs slow.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/fast%20vs%20slow.png)
│   │   ├── [flavours of gate.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/flavours%20of%20gate.png)
│   │   └── [lib.png](Module%201/Introduction%20to%20Yosys%20and%20Logic%20synthesis/lib.png)
│   │
│   ├── [Introduction](Module%201/Introduction/)
│   │   ├── [README.md](Module%201/Introduction/README.md)
│   │   ├── [iverilog flowchart.png](Module%201/Introduction/iverilog%20flowchart.png)
│   │   └── [testbench flowchat.png](Module%201/Introduction/testbench%20flowchat.png)
│   │
│   ├── [Lab 1 - Environment Setup](Module%201/Lab%201%20-%20Environment%20Setup/)
│   │   ├── [README.md](Module%201/Lab%201%20-%20Environment%20Setup/README.md)
│   │   ├── [git instalation.png](Module%201/Lab%201%20-%20Environment%20Setup/git%20instalation.png)
│   │   ├── [gtkwave install.png](Module%201/Lab%201%20-%20Environment%20Setup/gtkwave%20install.png)
│   │   ├── [gvim installation.png](Module%201/Lab%201%20-%20Environment%20Setup/gvim%20installation.png)
│   │   └── [iverilog installation.png](Module%201/Lab%201%20-%20Environment%20Setup/iverilog%20installation.png)
│   │
│   ├── [Lab 2 - Icarus Verilog, GTKWave and Gvim](Module%201/Lab%202%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/)
│   │   ├── [README.md](Module%201/Lab%202%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/README.md)
│   │   ├── [good mux code.png](Module%201/Lab%202%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/good%20mux%20code.png)
│   │   └── [good_mux waveform.png](Module%201/Lab%202%20-%20Icarus%20Verilog%2C%20GTKWave%20and%20Gvim/good_mux%20waveform.png)
│   │
│   └── [Lab 3 - Using Yosys](Module%201/Lab%203%20-%20Using%20Yosys/)
│       ├── [README.md](Module%201/Lab%203%20-%20Using%20Yosys/README.md)
│       ├── [Screenshot 2026-08-08 2323230sys netlist.png](Module%201/Lab%203%20-%20Using%20Yosys/Screenshot%202026-08-08%202323230sys%20netlist.png)
│       ├── [Yosis dot viewer.png](Module%201/Lab%203%20-%20Using%20Yosys/Yosis%20dot%20viewer.png)
│       ├── [yosis.png](Module%201/Lab%203%20-%20Using%20Yosys/yosis.png)
│       ├── [yosys netlist.png](Module%201/Lab%203%20-%20Using%20Yosys/yosys%20netlist.png)
│       ├── [yosys vs gvim.png](Module%201/Lab%203%20-%20Using%20Yosys/yosys%20vs%20gvim.png)
│       ├── [yosys2.png](Module%201/Lab%203%20-%20Using%20Yosys/yosys2.png)
│       └── [yosys3.png](Module%201/Lab%203%20-%20Using%20Yosys/yosys3.png)
│
└── Module 2/
    │
    ├── [Flip Flop Coding Styles](Module%202/Flip%20Flop%20coding%20styles/)
    │   ├── [README.md](Module%202/Flip%20Flop%20coding%20styles/README.md)
    │   ├── [Mix of sync and async.png](Module%202/Flip%20Flop%20coding%20styles/Mix%20of%20sync%20and%20async.png)
    │   ├── [asynchronous code.png](Module%202/Flip%20Flop%20coding%20styles/asynchronous%20code.png)
    │   ├── [asynchronous reset.png](Module%202/Flip%20Flop%20coding%20styles/asynchronous%20reset.png)
    │   ├── [synchronous.png](Module%202/Flip%20Flop%20coding%20styles/synchronous.png)
    │   └── [why flops are used.png](Module%202/Flip%20Flop%20coding%20styles/why%20flops%20are%20used.png)
    │
    ├── [Lab 4 - Library Walkthrough](Module%202/Lab4%20-%20Library%20walkthrough/)
    │   ├── [README.md](Module%202/Lab4%20-%20Library%20walkthrough/README.md)
    │   ├── [Exploration of lib.png](Module%202/Lab4%20-%20Library%20walkthrough/Exploration%20of%20lib.png)
    │   ├── [PVT.png](Module%202/Lab4%20-%20Library%20walkthrough/PVT.png)
    │   ├── [Starting of cell.png](Module%202/Lab4%20-%20Library%20walkthrough/Starting%20of%20cell.png)
    │   ├── [and power.png](Module%202/Lab4%20-%20Library%20walkthrough/and%20power.png)
    │   ├── [comparision.png](Module%202/Lab4%20-%20Library%20walkthrough/comparision.png)
    │   └── [differnet flavours of and.png](Module%202/Lab4%20-%20Library%20walkthrough/differnet%20flavours%20of%20and.png)
    │
    ├── [Lab 5 - Hierarchical and Flat Synthesis](Module%202/Lab5%20-%20Hier%20and%20flat%20synth/)
    │   └── [README.md](Module%202/Lab5%20-%20Hier%20and%20flat%20synth/README.md)
    │
    ├── [Lab 6 - Flip-Flop Synthesis and Simulation](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/)
    │   ├── [README.md](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/README.md)
    │   ├── [async dotlib changed.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/async%20dotlib%20changed.png)
    │   ├── [async dotlib.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/async%20dotlib.png)
    │   ├── [crosscheck.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/crosscheck.png)
    │   ├── [files used.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/files%20used.png)
    │   ├── [lab_asynch ff.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/lab_asynch%20ff.png)
    │   ├── [sync and async.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/sync%20and%20async.png)
    │   └── [tb_async.png](Module%202/Lab6%20-%20Flipflop%20synthesis%20and%20simulation/tb_async.png)
    │
    └── [Lab 7 - Interesting Optimisations](Module%202/Lab7%20-%20Intresting%20optimisations/)
        ├── [README.md](Module%202/Lab7%20-%20Intresting%20optimisations/README.md)
        ├── [Screenshot 2026-08-09 183302.png](Module%202/Lab7%20-%20Intresting%20optimisations/Screenshot%202026-08-09%20183302.png)
        ├── [gvim crosscheck of mul2_net.png](Module%202/Lab7%20-%20Intresting%20optimisations/gvim%20crosscheck%20of%20mul2_net.png)
        ├── [mult2 dotviewer.png](Module%202/Lab7%20-%20Intresting%20optimisations/mult2%20dotviewer.png)
        └── [mult2 explanation.png](Module%202/Lab7%20-%20Intresting%20optimisations/mult2%20explanation.png)

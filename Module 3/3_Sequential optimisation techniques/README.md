
# Sequential Logic Optimisation Techniques

## Theory

Sequential logic optimisation focuses on simplifying flip-flop-based circuits while maintaining their required clock, reset, and output behaviour. During synthesis, Yosys analyses the RTL and identifies constant values, redundant logic, and unnecessary sequential elements.

The optimisation process can reduce the number of gates and simplify the overall circuit. The remaining flip-flops can then be mapped to the available Sky130 standard-cell library using `dfflibmap`.

## Files Used

- `dff_const1.v`
- `dff_const2.v`
- `dff_const3.v`

## Synthesis and Optimisation Flow

The flip-flops were mapped to the Sky130 standard-cell library using:

```tcl
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The resulting netlists were inspected using:

```tcl
show
```

The sequential behaviour was also verified using GTKWave.

## Experiments

### 1. `dff_const1`

The first design demonstrates a flip-flop with a constant data input.

The optimisation process recognises that the data input does not depend on any changing combinational signal. Therefore, unnecessary logic associated with the data path can be removed while retaining the required clock and reset behaviour.

The optimised schematic shows the simplified flip-flop structure with the constant value connected to its data input.

### 2. `dff_const2`

The second design demonstrates optimisation when the output of the sequential circuit can be reduced to a constant value.

Yosys propagates the constant through the logic and removes redundant circuitry. The resulting structure is significantly simpler while maintaining the required sequential behaviour.

The generated schematic was used to observe how the original RTL description was reduced after optimisation.

### 3. `dff_const3`

The third design contains multiple sequential elements along with reset-related logic.

Yosys analyses the complete circuit and removes redundant portions of the design. The remaining flip-flops are then mapped to suitable Sky130 standard cells using `dfflibmap`.

The optimised schematic was inspected using `show`, and the resulting behaviour was verified in GTKWave using the generated simulation waveform.

## Verification

GTKWave was used to observe the clock, reset, and output signals.

The waveforms confirmed that the optimised sequential circuits maintained their intended behaviour while unnecessary logic had been removed during synthesis and optimisation.

## Learning Outcomes

* Understood the concept of sequential logic optimisation.
* Learned how Yosys simplifies constant-driven flip-flop circuits.
* Understood the purpose of `opt_clean -purge`.
* Learned how `dfflibmap` maps flip-flops to Sky130 standard cells.
* Learned to inspect optimised netlists using `show`.
* Verified sequential behaviour using GTKWave.
* Understood how synthesis optimisation can reduce redundant logic while preserving circuit functionality.



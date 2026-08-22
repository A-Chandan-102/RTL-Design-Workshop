# Unused Output Optimisation

## Theory

Unused output optimisation focuses on removing logic that does not contribute to any required output of the design. During synthesis, Yosys traces the logic connected to the module outputs and identifies internal signals or logic whose values are never used.

Removing such logic reduces unnecessary hardware, which can result in lower **area and power consumption** while preserving the behaviour of the required outputs.

## File Used

- `counter_opt.v`

## Objective

To understand how Yosys identifies and removes unused logic and sequential elements when their outputs are not required by the design.

## Implementation

The `counter_opt` design was synthesised using Yosys and the resulting netlist was inspected using the `show` command.

The original design contains a counter with several internal signals and outputs. During optimisation, Yosys analyses which signals actually affect the required output and removes logic that has no observable effect.

The optimised schematic shows that a large portion of the original counter logic is eliminated, leaving only the logic necessary to generate the required output.

## Optimisation

The main idea demonstrated in this lab is **dead logic removal**.

If an internal signal or output is not used anywhere in the final design, the logic required to generate that signal does not need to be implemented in hardware.

For the `counter_opt` design:

- Unused logic is identified during optimisation.
- Logic that does not affect the required output is removed.
- Necessary clock and reset paths are retained.
- The final netlist contains fewer cells and connections.
- The optimised circuit performs the required function with less hardware.

## Result

The `show` command was used to compare the generated netlist and observe the effect of unused output optimisation.

The optimised schematic is significantly simpler than the original structure because the logic associated with unused outputs has been removed.

## Learning Outcomes

- Understood unused output optimisation in RTL synthesis.
- Learned how Yosys identifies dead or unobservable logic.
- Understood how removing unused logic can reduce hardware area and power.
- Learned to inspect optimisation results using the `show` command.
- Understood the importance of writing RTL that does not contain unnecessary logic.

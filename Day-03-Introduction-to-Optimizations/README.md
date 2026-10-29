# Day 3: Introduction to Logic Optimizations

## Objective

Understand combinational and sequential logic optimization techniques in Yosys, including constant propagation, Boolean simplification, sequential constant optimization, and removal of unused logic.

## 1. Combinational Logic Optimization

Combinational logic optimization simplifies digital logic while preserving its functionality.

- **Constant propagation:** Replaces signals with known constant values (`0` or `1`) and simplifies the logic driven by them.
- **Boolean logic optimization:** Applies Boolean algebra and logic transformations to reduce unnecessary gates and simplify expressions.

### Lab 1: `opt_check`

The design contains a multiplexer with one input tied to logic `0`. Simplifying the multiplexer results in an AND gate.

Commands used:

```tcl
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The `opt_clean -purge` command removes unused wires and cells, including certain unused internal signals and objects.

![opt_check synthesized logic diagram](screenshots/1_opt_check.png)

### Lab 2: `opt_check2`

A multiplexer with one input tied to logic `1` simplifies to an OR gate, depending on the select and input connections.

![opt_check2 synthesized logic diagram](screenshots/2_opt_check2.png)

### Lab 3: `opt_check3`

A multiplexer with connections to ground is simplified using constant propagation and Boolean logic optimization, resulting in a three-input AND gate.

![opt_check3 synthesized logic diagram](screenshots/3_opt_check3.png)

### Lab 4: `opt_check4`

The original logic was analyzed and simplified. Yosys produced an XNOR-equivalent implementation, confirming the Boolean simplification.

![opt_check4 synthesized logic diagram](screenshots/4_opt_check4.png)

### Lab 5: `multiple_module`

The original Verilog file was inspected before synthesis. The design hierarchy was flattened and the optimization passes were applied. The resulting logic was simplified to a single cell.

![multiple_module optimized logic diagram](screenshots/5_multiple_module_opt.png)

### Lab 6: `multiple_module_opt2`

A second multiple-module example was analyzed using the same flattening and optimization approach.

![multiple_module_opt2 optimized logic diagram](screenshots/6_multiple_module_opt2.png)

## 2. Sequential Logic Optimization

Sequential logic optimization simplifies circuits containing flip-flops and state elements while preserving their cycle-by-cycle behavior.

- **Constant propagation:** Uses known values on sequential inputs, resets, or states to simplify registers and their surrounding logic.
- **Retiming:** Moves registers across combinational logic to improve timing while preserving the required input-output behavior and latency constraints.
- **Sequential logic cloning:** Duplicates registers or sequential logic when beneficial for timing, fanout, or implementation.
- **State optimization:** Removes redundant states or simplifies state encoding when the resulting behavior remains equivalent.

The applicability of these techniques depends on the RTL, synthesis passes, constraints, and target library.

### 2.1 Constant Flip-Flop Examples

#### `dff_const1` and `dff_const2`

Both examples were simulated and synthesized to study constant propagation in sequential logic.

- In `dff_const1`, the asynchronous reset and constant data value produce a behavior that must be considered with the reset polarity and timing.
- In `dff_const2`, the reset and data values make the output constant under the specified conditions, allowing Yosys to remove unnecessary sequential hardware.

**Waveform comparison**

![dff_const1 and dff_const2 GTKWave waveforms](screenshots/7_dff_const_gtk.png)

**Synthesized logic diagrams**

The diagrams compare the resulting implementations. One retains sequential logic, while the constant-output case can reduce to a constant connection.

![dff_const1 and dff_const2 synthesized logic diagrams](screenshots/8_dff_const.png)

### 2.2 `dff_const3`: Sequential Dependency

This example contains two flip-flops, with one flip-flop's output feeding the set input of another.

When reset is asserted, the first output changes according to its reset behavior. Because the second flip-flop samples its input at a clock edge, it responds to the preceding value according to the clocked behavior and propagation timing.

The sequential dependency means that the registers cannot necessarily be replaced by constants.

**GTKWave observation**

![dff_const3 waveform](screenshots/9_dff_const3_gtk.png)

**Synthesized logic diagram**

![dff_const3 synthesized logic diagram](screenshots/10_dff_const3.png)

Both flip-flops are preserved in this synthesis result.

### 2.3 `dff_const4`: Constant Sequential Optimization

In this example, the reset and set conditions drive both flip-flops toward constant values. Under the conditions represented in the RTL, the sequential elements can be optimized away, leaving constant connections.

![dff_const4 RTL and synthesized logic diagram](screenshots/11_dff_const4.png)

### 2.4 `dff_const5`: Registers Preserved

In this example, both reset conditions are set to `0`. The resulting behavior differs from the previous case, so the registers are preserved in the observed synthesis result.

![dff_const5 synthesized logic diagram](screenshots/12_dff_const5.png)

These experiments demonstrate that a register connected to a constant signal is not automatically removable. Its reset, set, data, control conditions, and sequential dependencies must be considered together.

## 3. Unused-Output Optimization

Synthesis tools remove logic that does not affect observable outputs or other required behavior. This reduces unnecessary hardware.

### 3.1 `counter_opt`

The design contains a three-bit counter, but the output `q` uses only `count[0]`.

The upper two counter bits do not affect the observable output and can be removed. The least significant bit toggles, allowing the optimized implementation to use a flip-flop and inverter.

![counter_opt RTL and synthesized logic diagram](screenshots/13_counter_opt.png)

### 3.2 `counter_opt2`

In this example, the output is changed to a comparison involving the full counter value:

`q = (count == 3'b100)`

All three counter bits contribute to the comparison, so the upper bits are now relevant to the observable output. The counter registers therefore cannot be removed simply as unused bits.

![counter_opt2 RTL and synthesized logic diagram](screenshots/14_counter_opt2.png)

## Key Learnings

- Applied constant propagation and Boolean simplification to combinational logic.
- Observed the effect of `opt_clean -purge` in the Yosys synthesis flow.
- Studied constant propagation and preservation of sequential elements.
- Understood how reset/set behavior and sequential dependencies affect optimization.
- Observed removal of unused counter bits when they do not affect outputs.
- Learned that synthesis optimizations preserve required observable behavior.

## Conclusion

This lab demonstrated how Yosys simplifies combinational logic, evaluates sequential behavior, and removes unnecessary hardware. Understanding these optimizations helps in writing efficient RTL and interpreting synthesized implementations.

## Tools Used

- Verilog HDL
- Yosys
- Icarus Verilog
- GTKWave
- SKY130 standard-cell library

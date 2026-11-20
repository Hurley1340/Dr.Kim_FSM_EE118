# 3-Bit Mod-7 Up/Down FSM Counter

## The Task

Design and build a **synchronous 3-bit mod-7 up/down counter** using 7400-series TTL logic. The counter must follow a custom seven-state sequence, count in either direction, and include an enable input that holds the current state when disabled.

My assigned sequence was **Case 4**, based on the last digit of my student ID. State `000` is omitted.

## The Design

### This is a synchronous 3-bit FSM with a custom counting sequence and enable control <br>

**Count Up (`u = 1`):** `111 → 001 → 101 → 010 → 110 → 011 → 100 → 111` <br>
**Count Down (`u = 0`):** `111 → 100 → 011 → 110 → 010 → 101 → 001 → 111`

Three D flip-flops store the present state (`Qc`, `Qb`, `Qa`). On each active clock edge, the circuit either advances to the next state in the selected direction or holds its current state.

The original design requirements assigned a **D flip-flop** to `Qc` (MSB), **SR flip-flop** to `Qb`, and **T flip-flop** to `Qa` (LSB). I derived the excitation equations from the state-transition table, converted the SR and T behavior into equivalent D-input logic, and further minimized the resulting Boolean expressions. This allowed the entire circuit to use readily available D flip-flops.

The final next-state logic uses different implementations for each bit:

- **`Qc` (MSB):** 4-to-1 multiplexer (`74LS153`)
- **`Qb` (middle bit):** Combinational gate logic
- **`Qa` (LSB):** 3-to-8 decoder (`74LS138`) with supporting logic

A **74LS157 quad 2-to-1 multiplexer** selects between the present-state outputs and the calculated next-state bits. When `EN = 0`, the multiplexer feeds the present state back into the D inputs, so the counter holds its state. When `EN = 1`, the next-state logic is selected and the counter advances on the next clock edge.

**`u = 1`** → Count Up, **`u = 0`** → Count Down <br>
**`EN = 1`** → Count, **`EN = 0`** → Hold

## Components

- D flip-flops (`74LS74`)
- Dual 4-to-1 multiplexer (`74LS153`)
- Quad 2-to-1 multiplexer (`74LS157`)
- 3-to-8 decoder (`74LS138`)
- AND, OR, NOT, and other supporting logic gates
- Up/down direction and enable switches
- Manual clock input

## Concepts Demonstrated

### Custom counter design

- Synchronous sequential logic and finite-state-machine design
- Custom up/down state transitions with an omitted state
- SR and T flip-flop excitation tables and conversion to D-input equations
- Boolean minimization and combinational logic implementation
- Multiplexer- and decoder-based next-state logic
- Enable-controlled state feedback and hold functionality
- Physical TTL circuit construction, testing, and debugging

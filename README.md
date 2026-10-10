# OctaBit Digital Circuit Library (quad_0.1)

A modular, industry-standard educational repository of digital logic circuits, arithmetic units, data routing elements, and sequential storage designs modeled in **Logisim-evolution v5.0.0**. This library scales from fundamental discrete gates up to a multi-operation 8-bit Arithmetic Logic Unit (ALU) and synchronous Register File.

---

## Table of Contents

- [Overview & Objectives](#overview--objectives)
- [Design Tools & Environment](#design-tools--environment)
- [Repository Structure](#repository-structure)
- [Circuit Catalog](#circuit-catalog)
  - [Arithmetic Logic Units (ALU)](#arithmetic-logic-units-alu)
  - [Arithmetic & Adders/Subtractors](#arithmetic--adderssubtractors)
  - [Combinational Logic & Data Routing](#combinational-logic--data-routing)
  - [Sequential Logic & Storage](#sequential-logic--storage)
- [How to Open and Simulate](#how-to-open-and-simulate)
  - [Prerequisites](#prerequisites)
  - [Running Logisim-evolution](#running-logisim-evolution)
  - [Interactive Testing Guide](#interactive-testing-guide)
- [Bill of Materials & Component Breakdown](#bill-of-materials--component-breakdown)
- [PCB & Physical Hardware Implementation](#pcb--physical-hardware-implementation)
- [Known Limitations & Roadmap](#known-limitations--roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Overview & Objectives

The **OctaBit quad_0.1** project is an educational digital systems and computer organization (DDCO) circuit library. The repository provides clean, inspectable, and reproducible logic designs serving as building blocks for computer architecture:

1. **Gate-Level Foundations**: Discrete implementations of adders, subtractors, multiplexers, decoders, and code converters using standard logic gates (AND, OR, NOT, NAND, XOR).
2. **Arithmetic Progression**: From single-bit half adders to 4-bit ripple-carry adders, lookahead carry cells, and multiplexer-synthesized logic.
3. **Data Path & Storage**: Synchronous register files and four configurations of shift registers (SISO, SIPO, PISO, PIPO) alongside bidirectional steering logic.
4. **Processing Units**: 4-bit and 8-bit modular Arithmetic Logic Units executing arithmetic (ADD, SUB, MULT, DIV) and bitwise logic operations (AND, OR, XOR, NOT).

---

## Design Tools & Environment

All circuit models are native XML-based `.circ` files formatted for:

- **EDA Tool**: [Logisim-evolution](https://github.com/logisim-evolution/logisim-evolution)
- **Engine Version**: `v5.0.0` (compatible with Logisim-evolution 3.8.0+)
- **Format**: Native XML (`<project source="5.0.0" version="1.0">`)
- **Dependencies**: 100% self-contained. No external third-party `.circ` or binary library files are required.

---

## Repository Structure

```text
quad_0.1/
├── .gitignore                      # Git ignore rules for Logisim, OS, and IDE files
├── README.md                       # Main repository documentation & catalog
├── docs/
│   └── CIRCUITS.md                 # Complete pinouts, truth tables, and equations
└── circuits/
    ├── alu/                        # Processing units & multi-operation ALUs
    │   ├── ALU.circ                # 8-bit 8-operation Arithmetic Logic Unit
    │   └── ArithmeticUnit.circ     # 4-bit 4-operation Arithmetic Unit
    ├── arithmetic/                 # Adders, subtractors, and arithmetic blocks
    │   ├── Adder_Mux_8_1.circ      # Full adder synthesized with 8:1 MUXes
    │   ├── FullAdder.circ          # 1-bit full adder (discrete gates, SOP)
    │   ├── FullSubtractor.circ     # 1-bit full subtractor (discrete gates, SOP)
    │   ├── HalfAdder.circ          # 1-bit half adder (XOR + AND)
    │   ├── HalfSubtractor.circ     # 1-bit half subtractor (XOR + NOT + AND)
    │   ├── LAC.circ                # Lookahead carry adder cell (P & G logic)
    │   ├── RippleAdder.circ        # 4-bit gate-level ripple carry adder (discrete)
    │   └── RippleAdder_2.circ      # 4-stage modular adder cascade
    ├── combinational/              # Data routing, decoders, encoders, converters
    │   ├── BinaryToGray.circ       # 4-bit binary to Gray code converter
    │   ├── Comparator_equal.circ   # 2-bit equality comparator (SOP)
    │   ├── DeMUX.circ              # 1-to-4 demultiplexer (discrete gates)
    │   ├── De_coder.circ           # 3-to-8 line decoder (discrete gates)
    │   ├── Encoder.circ            # 9-to-4 decimal-to-BCD encoder
    │   └── MUX.circ                # 4-to-1 multiplexer (NAND-NAND with enable)
    └── sequential/                 # Registers, register files, and shift registers
        ├── BiDirectReg.circ        # 4-bit bidirectional shift register
        ├── Registers.circ          # Dual 4-bit register file (Reg A & Reg B)
        └── ShiftReg.circ           # 4-in-1 shift register suite (SISO, SIPO, PISO, PIPO)
```

---

## Circuit Catalog

| Subsystem | Circuit File | Inputs | Outputs | Primary Logic / Components | Description |
|:---|:---|:---:|:---:|:---|:---|
| **ALU** | [`ALU.circ`](circuits/alu/ALU.circ) | `A`[8], `B`[8], `OP`[3] | `RESULT`[8] | 8:1 MUX, Adder, Subtractor, Multiplier, Divider, Logic Gates | 8-bit, 8-operation ALU |
| **ALU** | [`ArithmeticUnit.circ`](circuits/alu/ArithmeticUnit.circ) | `A`[4], `B`[4], `OP`[2] | `Result`[4] | 4:1 MUX, Adder, Subtractor, Multiplier, Divider | 4-bit, 4-operation Arithmetic Unit |
| **Arithmetic** | [`HalfAdder.circ`](circuits/arithmetic/HalfAdder.circ) | `A`, `B` | `S`, `C` | 1 XOR, 1 AND | 1-bit Half Adder |
| **Arithmetic** | [`FullAdder.circ`](circuits/arithmetic/FullAdder.circ) | `A`, `B`, `Cin` | `Sum`, `Cout` | 7 AND, 3 NOT, 2 OR | 1-bit Full Adder (SOP canonical) |
| **Arithmetic** | [`HalfSubtractor.circ`](circuits/arithmetic/HalfSubtractor.circ) | `A`, `Bi` | `S`, `Bo` | 1 XOR, 1 NOT, 1 AND | 1-bit Half Subtractor |
| **Arithmetic** | [`FullSubtractor.circ`](circuits/arithmetic/FullSubtractor.circ) | `A`, `B`, `Bin` | `Diff`, `Bout` | 7 AND, 3 NOT, 2 OR | 1-bit Full Subtractor (SOP canonical) |
| **Arithmetic** | [`LAC.circ`](circuits/arithmetic/LAC.circ) | `A`, `B`, `Cin` | `Sum`, `Cout` | 2 XOR, 2 AND, 1 OR | Carry Lookahead cell ($P$ & $G$ formulation) |
| **Arithmetic** | [`RippleAdder.circ`](circuits/arithmetic/RippleAdder.circ) | $A[3:0]$, $B[3:0]$, $C_0$ | $S[3:0]$, $C[4:1]$ | 28 AND, 12 NOT, 8 OR | 4-bit Ripple Carry Adder (discrete gates) |
| **Arithmetic** | [`RippleAdder_2.circ`](circuits/arithmetic/RippleAdder_2.circ) | $A[4]$, $B[4]$, $Cin[4]$ | 4 Sums[4], $Cout$[1] | 4 4-bit Adders | 4-stage Modular Adder Cascade |
| **Arithmetic** | [`Adder_Mux_8_1.circ`](circuits/arithmetic/Adder_Mux_8_1.circ) | Select[3], Rails | `Sum`, `Carry` | 2 8:1 Multiplexers | Full Adder synthesized via Multiplexers |
| **Combinational** | [`MUX.circ`](circuits/combinational/MUX.circ) | `I0..I3`, `S1..S0`, `G` | `Y` | 5 NAND, 3 NOT | 4-to-1 MUX (NAND logic with enable) |
| **Combinational** | [`DeMUX.circ`](circuits/combinational/DeMUX.circ) | Data, `s1`, `s0` | $Y_0..Y_3$ | 4 AND, 2 NOT | 1-to-4 Demultiplexer |
| **Combinational** | [`De_coder.circ`](circuits/combinational/De_coder.circ) | `A2`, `A1`, `A0` | $Y_0..Y_7$ | 8 AND, 3 NOT | 3-to-8 Line Decoder |
| **Combinational** | [`Encoder.circ`](circuits/combinational/Encoder.circ) | $D_1..D_9$ | $B_0..B_3$ | 4 OR | 9-to-4 Decimal-to-BCD Encoder |
| **Combinational** | [`Comparator_equal.circ`](circuits/combinational/Comparator_equal.circ) | $A_1, A_0, B_1, B_0$ | $A=B$ | 4 AND, 4 NOT, 1 OR | 2-bit Equality Comparator |
| **Combinational** | [`BinaryToGray.circ`](circuits/combinational/BinaryToGray.circ) | $B_3..B_0$ | $G_3..G_0$ | 3 XOR | 4-bit Binary to Gray Code Converter |
| **Sequential** | [`Registers.circ`](circuits/sequential/Registers.circ) | `DATAIN`[4], `Load_A`, `Load_B`, `Clock` | `REG_A_OUT`[4], `REG_B_OUT`[4] | 8 DFFs, Splitters, AND/NOT gates | Dual 4-bit Register File (`RegisterFile`) |
| **Sequential** | [`ShiftReg.circ`](circuits/sequential/ShiftReg.circ) | Serial/Parallel ins, Clocks | Serial/Parallel outs | 16 DFFs, 4 MUXes, 4 Clocks | 4-in-1 Shift Registers (SISO, SIPO, PISO, PIPO) |
| **Sequential** | [`BiDirectReg.circ`](circuits/sequential/BiDirectReg.circ) | Mode, Serial In, Clock | Bit States | 4 DFFs, 8 AND, 4 OR, 1 Clock | 4-bit Bidirectional Shift Register |

---

### Arithmetic Logic Units (ALU)

#### 1. 8-Bit Arithmetic Logic Unit (`ALU.circ`)
Processes two 8-bit input buses (`A` and `B`) to produce an 8-bit output (`RESULT`) driven by a 3-bit operation select bus (`OP`):

```text
       A [8-bit] ──────┬──────────────┬──────────────┬──────────────┬──────────────┐
       B [8-bit] ──┐   │              │              │              │              │
                   ▼   ▼              ▼              ▼              ▼              ▼
                 [ADD] [SUB]        [MULT]         [DIV]          [AND]          [OR] ...
                   │     │              │              │              │              │
                   └─────┴──────────────┴──────┬───────┴──────────────┴──────────────┘
                                               ▼
                                     ┌──────────────────┐
                     OP [3-bit] ────►│ 8:1 Multiplexer  ├────► RESULT [8-bit]
                                     └──────────────────┘
```

- `000` (0): `ADD` ($A + B$)
- `001` (1): `SUB` ($A - B$)
- `010` (2): `MULT` ($A \times B$)
- `011` (3): `DIV` ($A / B$)
- `100` (4): `AND` ($A \land B$)
- `101` (5): `OR` ($A \lor B$)
- `110` (6): `XOR` ($A \oplus B$)
- `111` (7): `NOT A` ($\sim A$)

#### 2. 4-Bit Arithmetic Unit (`ArithmeticUnit.circ`)
A focused arithmetic core executing `ADD` (`00`), `SUB` (`01`), `MULT` (`10`), and `DIV` (`11`) on two 4-bit input buses.

---

### Arithmetic & Adders/Subtractors

- **[`HalfAdder.circ`](circuits/arithmetic/HalfAdder.circ)** & **[`FullAdder.circ`](circuits/arithmetic/FullAdder.circ)**: Foundational single-bit binary adders. The Full Adder is implemented in canonical Sum-of-Products form with discrete AND/OR/NOT logic.
- **[`HalfSubtractor.circ`](circuits/arithmetic/HalfSubtractor.circ)** & **[`FullSubtractor.circ`](circuits/arithmetic/FullSubtractor.circ)**: Single-bit subtraction calculating Difference and Borrow.
- **[`LAC.circ`](circuits/arithmetic/LAC.circ)**: Single-bit Lookahead Carry cell demonstrating Carry Generation ($G = A \cdot B$) and Carry Propagation ($P = A \oplus B$) to mitigate ripple propagation delays.
- **[`RippleAdder.circ`](circuits/arithmetic/RippleAdder.circ)**: 4-bit ripple-carry adder built from 28 AND, 12 NOT, and 8 OR gates with cascading carry lines ($C_0 \to C_1 \to C_2 \to C_3 \to C_4$).
- **[`Adder_Mux_8_1.circ`](circuits/arithmetic/Adder_Mux_8_1.circ)**: Canonical logic synthesis using two 8-to-1 multiplexers with operand inputs serving as address/select bits.

---

### Combinational Logic & Data Routing

- **[`MUX.circ`](circuits/combinational/MUX.circ)**: 4-to-1 Multiplexer built with NAND-NAND logic and an active-low enable gate (`G`).
- **[`DeMUX.circ`](circuits/combinational/DeMUX.circ)**: 1-to-4 Demultiplexer steering one data line to four outputs using 2 select bits.
- **[`De_coder.circ`](circuits/combinational/De_coder.circ)**: 3-to-8 Line Decoder mapping a 3-bit binary address into 8 unique active-high signals.
- **[`Encoder.circ`](circuits/combinational/Encoder.circ)**: 9-to-4 Decimal-to-BCD priority-style encoder using multi-input OR gates.
- **[`Comparator_equal.circ`](circuits/combinational/Comparator_equal.circ)**: 2-bit equality comparator asserting output high when $A_1 A_0 == B_1 B_0$.
- **[`BinaryToGray.circ`](circuits/combinational/BinaryToGray.circ)**: 4-bit Binary to Gray Code converter utilizing 3 cascaded XOR gates ($G_i = B_i \oplus B_{i+1}$).

---

### Sequential Logic & Storage

- **[`Registers.circ`](circuits/sequential/Registers.circ)**: A dual 4-bit register file containing **Register A** and **Register B** (8 edge-triggered D flip-flops total). Supports selective loading via `Load_A` and `Load_B` signals with gated clocking.
  > *Note*: In Logisim-evolution, switch to the `RegisterFile` circuit in the project explorer to simulate the register array.
- **[`ShiftReg.circ`](circuits/sequential/ShiftReg.circ)**: Complete laboratory bench containing four distinct 4-bit shift registers:
  1. **SIPO** (Serial-In Parallel-Out)
  2. **SISO** (Serial-In Serial-Out)
  3. **PISO** (Parallel-In Serial-Out)
  4. **PIPO** (Parallel-In Parallel-Out)
- **[`BiDirectReg.circ`](circuits/sequential/BiDirectReg.circ)**: 4-bit Bidirectional Shift Register using AND-OR steering gates to shift data left or right synchronously on the clock edge.

For complete truth tables, logic expressions, and detailed pinout tables, see [docs/CIRCUITS.md](docs/CIRCUITS.md).

---

## How to Open and Simulate

### Prerequisites

1. **Java Runtime Environment**: Java 17 or higher (Java 21/27 recommended).
2. **Logisim-evolution**: Version 3.8.0 or 5.0.0. Download the standalone jar or installer from the official [Logisim-evolution Releases](https://github.com/logisim-evolution/logisim-evolution/releases).

### Running Logisim-evolution

Launch Logisim-evolution:

```bash
# If using the standalone jar
java -jar logisim-evolution-5.0.0-all.jar

# Or launch the native application
logisim-evolution
```

From the menu, choose **File > Open** and select any `.circ` file from the `circuits/` folder.

### Interactive Testing Guide

1. **Poke Tool**: Click the **Poke Tool** (hand icon with index finger, shortcut `Ctrl+1`) in the toolbar.
2. **Toggle Inputs**: Click on input pins to toggle logic levels between `0` and `1`.
3. **Multi-Bit Buses**: For multi-bit pins (such as `A[8]`, `B[8]`, or `OP[3]` in `ALU.circ`), click on the pin and type the desired hexadecimal or decimal value.
4. **Clock Stepping**: In sequential circuits (`Registers.circ`, `ShiftReg.circ`, `BiDirectReg.circ`), toggle the clock pin using the Poke Tool, or enable automatic ticking via **Simulate > Ticks Enabled** (`Ctrl+K`).
5. **Multi-Circuit Files**: For `Registers.circ`, double-click `RegisterFile` in the left project tree to switch to the active register core.

---

## Bill of Materials & Component Breakdown

Summary of all primitive components across the 19 circuit designs:

| Component Type | Count Across All Designs | Typical Usage |
|:---|:---:|:---|
| **D Flip-Flop** | 32 | Sequential state storage (Registers & Shift Registers) |
| **AND Gate** | 80 | Logic decoding, minterm matching, clock gating |
| **NOT Gate** | 31 | Signal inversion, decoder addresses, comparator logic |
| **OR Gate** | 24 | Logic summing, BCD encoding, multiplexing |
| **XOR Gate** | 9 | Half adders, code conversion, bitwise logic |
| **NAND Gate** | 5 | Universal gate multiplexer synthesis (`MUX.circ`) |
| **Multiplexer (Block)** | 8 | Arithmetic selection, PISO shift register steering |
| **Arithmetic Blocks** | 10 | 4-bit & 8-bit Adders, Subtractors, Multipliers, Dividers |
| **Clock Source** | 6 | Synchronous sequential timing |
| **Bus Splitter** | 4 | 4-bit bus tapping and consolidation |

---

## PCB & Physical Hardware Implementation

The designs in this repository currently exist as digital logic models within Logisim-evolution. 

- **No physical PCB layouts** (e.g. KiCad `.kicad_pcb` or Gerber archives) are included in this version.
- **Hardware Realization**: The discrete gate circuits can be mapped directly to standard 74xx-series TTL/CMOS integrated circuits (e.g., 7408 AND, 7404 NOT, 7432 OR, 7486 XOR, 7474 D-FF, 74151 8:1 MUX).
- **FPGA Synthesis**: Logisim-evolution includes built-in HDL generation (VHDL and Verilog) under the **FPGA** menu, enabling automated synthesis to hardware targets (e.g., Intel Cyclone, Xilinx Spartan/Artix).

---

## Known Limitations & Roadmap

- **Status Flags**: The 8-bit ALU currently computes the 8-bit result bus but does not export dedicated status flags (Zero, Carry, Negative, Overflow). Adding a flags register is planned for version 0.2.
- **Subcircuit Modularization**: Several circuits (`RippleAdder.circ`) are laid out with discrete gates rather than referencing reusable subcircuit blocks. Modularizing into reusable subcircuits will simplify hierarchical designs.
- **Automated Test Benches**: Adding Logisim-evolution automated test vectors (`.csv` test files) to enable headless CLI testing via CI/CD.

---

## Contributing

Contributions, optimizations, and additional circuit modules are welcome!

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/status-flags`).
3. Build and verify your circuit in **Logisim-evolution v5.0.0**.
4. Maintain clean pin labeling and annotate circuits with descriptive labels.
5. Submit a Pull Request with a clear description of the circuit function and truth table.

---

## License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, distribute, and integrate these circuit designs for educational, academic, and commercial purposes.


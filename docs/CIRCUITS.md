# Digital Circuit Reference & Technical Specifications

This document provides technical specifications, pinout descriptions, logic expressions, and operational truth tables for all circuits in the **OctaBit quad_0.1** repository. All circuits are designed and simulated using **Logisim-evolution v5.0.0**.

---

## Table of Contents

- [1. Arithmetic Logic Units (ALU)](#1-arithmetic-logic-units-alu)
  - [8-Bit ALU (`ALU.circ`)](#8-bit-alu-alucirc)
  - [4-Bit Arithmetic Unit (`ArithmeticUnit.circ`)](#4-bit-arithmetic-unit-arithmeticunitcirc)
- [2. Arithmetic & Adder/Subtractor Circuits](#2-arithmetic--addersubtractor-circuits)
  - [Half Adder (`HalfAdder.circ`)](#half-adder-halfaddercirc)
  - [Full Adder (`FullAdder.circ`)](#full-adder-fulladdercirc)
  - [Half Subtractor (`HalfSubtractor.circ`)](#half-subtractor-halfsubtractorcirc)
  - [Full Subtractor (`FullSubtractor.circ`)](#full-subtractor-fullsubtractorcirc)
  - [Lookahead Carry Adder Cell (`LAC.circ`)](#lookahead-carry-adder-cell-laccount)
  - [4-Bit Gate-Level Ripple Carry Adder (`RippleAdder.circ`)](#4-bit-gate-level-ripple-carry-adder-rippleaddercirc)
  - [4-Stage Modular Adder Cascade (`RippleAdder_2.circ`)](#4-stage-modular-adder-cascade-rippleadder_2circ)
  - [Full Adder via Multiplexer (`Adder_Mux_8_1.circ`)](#full-adder-via-multiplexer-adder_mux_8_1circ)
- [3. Combinational & Data-Routing Circuits](#3-combinational--data-routing-circuits)
  - [4-to-1 Multiplexer (`MUX.circ`)](#4-to-1-multiplexer-muxcirc)
  - [1-to-4 Demultiplexer (`DeMUX.circ`)](#1-to-4-demultiplexer-demuxcirc)
  - [3-to-8 Line Decoder (`De_coder.circ`)](#3-to-8-line-decoder-de_codercirc)
  - [9-to-4 Decimal-to-BCD Encoder (`Encoder.circ`)](#9-to-4-decimal-to-bcd-encoder-encodercirc)
  - [2-Bit Equality Comparator (`Comparator_equal.circ`)](#2-bit-equality-comparator-comparator_equalcirc)
  - [4-Bit Binary to Gray Code Converter (`BinaryToGray.circ`)](#4-bit-binary-to-gray-code-converter-binarytograycirc)
- [4. Sequential & Storage Circuits](#4-sequential--storage-circuits)
  - [Dual 4-Bit Register File (`Registers.circ`)](#dual-4-bit-register-file-registerscirc)
  - [4-in-1 Shift Register Suite (`ShiftReg.circ`)](#4-in-1-shift-register-suite-shiftregcirc)
  - [4-Bit Bidirectional Shift Register (`BiDirectReg.circ`)](#4-bit-bidirectional-shift-register-bidirectregcirc)

---

## 1. Arithmetic Logic Units (ALU)

### 8-Bit ALU (`ALU.circ`)

The 8-Bit ALU combines arithmetic and bitwise logic operations into a single execution block selected by a 3-bit operation code (`OP`).

#### Pin Specifications

| Port | Direction | Bus Width | Description |
|:---|:---:|:---:|:---|
| `A` | Input | 8-bit | Operand A |
| `B` | Input | 8-bit | Operand B |
| `OP` | Input | 3-bit | Operation select code (`000` to `111`) |
| `RESULT` | Output | 8-bit | Calculated output |

#### Operation Mapping

| OP (`OP[2:0]`) | Operation | Expression | Hardware Component |
|:---:|:---|:---|:---|
| `000` (0) | **ADD** | $A + B$ | Logisim 8-bit Adder |
| `001` (1) | **SUB** | $A - B$ | Logisim 8-bit Subtractor |
| `010` (2) | **MULT** | $A \times B$ | Logisim 8-bit Multiplier |
| `011` (3) | **DIV** | $A / B$ | Logisim 8-bit Divider |
| `100` (4) | **AND** | $A \land B$ | 8-bit Bitwise AND Gate |
| `101` (5) | **OR** | $A \lor B$ | 8-bit Bitwise OR Gate |
| `110` (6) | **XOR** | $A \oplus B$ | 8-bit Bitwise XOR Gate |
| `111` (7) | **NOT A** | $\sim A$ | 8-bit Bitwise Inverter |

---

### 4-Bit Arithmetic Unit (`ArithmeticUnit.circ`)

A dedicated 4-bit computation block performing four fundamental arithmetic functions selected via a 2-bit control signal.

#### Pin Specifications

| Port | Direction | Bus Width | Description |
|:---|:---:|:---:|:---|
| `A` | Input | 4-bit | 4-bit primary operand |
| `B` | Input | 4-bit | 4-bit secondary operand |
| `OP` | Input | 2-bit | Operation select code (`00` to `11`) |
| `Result` | Output | 4-bit | 4-bit computed arithmetic result |

#### Operation Mapping

| OP (`OP[1:0]`) | Operation | Expression | Implementation |
|:---:|:---|:---|:---|
| `00` | **ADD** | $A + B$ | 4-bit Adder |
| `01` | **SUB** | $A - B$ | 4-bit Subtractor |
| `10` | **MULT** | $A \times B$ | 4-bit Multiplier |
| `11` | **DIV** | $A / B$ | 4-bit Divider |

---

## 2. Arithmetic & Adder/Subtractor Circuits

### Half Adder (`HalfAdder.circ`)

Performs addition of two single-bit inputs without a carry-in.

- **Inputs**: `A` (1-bit), `B` (1-bit)
- **Outputs**: `S` (Sum, 1-bit), `C` (Carry, 1-bit)
- **Boolean Logic**:
  $$S = A \oplus B$$
  $$C = A \cdot B$$

#### Truth Table

| $A$ | $B$ | $S$ (Sum) | $C$ (Carry) |
|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

---

### Full Adder (`FullAdder.circ`)

Adds three 1-bit operands ($A$, $B$, $Cin$) using discrete logic gates in canonical Sum-of-Products (SOP) form.

- **Inputs**: `A` (1-bit), `B` (1-bit), `Cin` (1-bit)
- **Outputs**: `Sum` (1-bit), `Cout` (1-bit)
- **Boolean Logic**:
  $$\text{Sum} = \bar{A}\bar{B}Cin + \bar{A}B\overline{Cin} + A\bar{B}\overline{Cin} + ABCin$$
  $$\text{Cout} = AB + B\cdot Cin + A\cdot Cin$$

---

### Half Subtractor (`HalfSubtractor.circ`)

Computes difference and borrow for two 1-bit operands.

- **Inputs**: `A` (Minuend, 1-bit), `Bi` (Subtrahend, 1-bit)
- **Outputs**: `S` (Difference, 1-bit), `Bo` (Borrow Out, 1-bit)
- **Boolean Logic**:
  $$\text{Difference} = A \oplus Bi$$
  $$\text{Borrow Out} = \bar{A} \cdot Bi$$

#### Truth Table

| $A$ | $Bi$ | $S$ (Diff) | $Bo$ (Borrow) |
|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |

---

### Full Subtractor (`FullSubtractor.circ`)

Computes difference and borrow for three 1-bit inputs ($A$, $B$, $Bin$) using canonical gate-level SOP logic.

- **Inputs**: `A` (1-bit), `B` (1-bit), `Bin` (1-bit)
- **Outputs**: `Diff` (1-bit), `Bout` (1-bit)
- **Boolean Logic**:
  $$\text{Diff} = A \oplus B \oplus Bin$$
  $$\text{Bout} = \bar{A}B + \bar{A}Bin + B\cdot Bin$$

---

### Lookahead Carry Adder Cell (`LAC.circ`)

Implements a high-speed adder cell utilizing carry generation ($G$) and propagation ($P$) to minimize ripple latency.

- **Inputs**: `A` (1-bit), `B` (1-bit), `Cin` (1-bit)
- **Outputs**: `Sum` (1-bit), `Cout` (1-bit)
- **Equations**:
  - Carry Propagate: $P = A \oplus B$
  - Carry Generate: $G = A \cdot B$
  - Sum: $\text{Sum} = P \oplus Cin = A \oplus B \oplus Cin$
  - Carry Out: $\text{Cout} = G + (P \cdot Cin)$

---

### 4-Bit Gate-Level Ripple Carry Adder (`RippleAdder.circ`)

A 4-bit adder constructed by cascading four discrete gate-level Full Adder slices without built-in macros.

- **Inputs**:
  - $A[3:0]$ (4 single-bit input pins)
  - $B[3:0]$ (4 single-bit input pins)
  - $C_0$ (Carry In, 1-bit)
- **Outputs**:
  - $S[3:0]$ (4 single-bit Sum pins)
  - $C_1, C_2, C_3, C_4$ (Intermediate and final carry output pins)
- **Gate Count**: 28 AND gates, 12 NOT gates, 8 OR gates (65 total components, 285 wire routes).

---

### 4-Stage Modular Adder Cascade (`RippleAdder_2.circ`)

A multi-stage adder pipeline constructed with 4-bit arithmetic adder blocks.

- **Inputs**: $A$ (4-bit), $B$ (4-bit), $Cin$ (4-bit)
- **Outputs**: 4 4-bit intermediate/stage sum pins and 1 carry-out pin.

---

### Full Adder via Multiplexer (`Adder_Mux_8_1.circ`)

Synthesizes a 1-bit Full Adder using two 8:1 Multiplexers where operand bits $(A, B, Cin)$ drive the 3-bit multiplexer select inputs.

- **Select Input**: 3-bit bus $(A, B, Cin)$ connected to select lines of both multiplexers.
- **Data Channels**: Tied to logic constants (0 or 1):
  - **Sum Mux**: Logic 1 on channels 1, 2, 4, 7 ($\sum m(1, 2, 4, 7)$).
  - **Carry Mux**: Logic 1 on channels 3, 5, 6, 7 ($\sum m(3, 5, 6, 7)$).
- **Outputs**: Sum (Mux 1 output), Carry (Mux 2 output).

---

## 3. Combinational & Data-Routing Circuits

### 4-to-1 Multiplexer (`MUX.circ`)

A 4-to-1 multiplexer constructed strictly using NOT gates and NAND gates (NAND-NAND equivalent to AND-OR).

- **Data Inputs**: `I0`, `I1`, `I2`, `I3` (1-bit each)
- **Select Inputs**: `S1`, `S0`
- **Enable Input**: `G` (active-low enable)
- **Output**: 1-bit multiplexed output
- **Logic Expression**:
  $$Y = \overline{G} \cdot \left( I_0 \overline{S_1}\overline{S_0} + I_1 \overline{S_1}S_0 + I_2 S_1\overline{S_0} + I_3 S_1 S_0 \right)$$

---

### 1-to-4 Demultiplexer (`DeMUX.circ`)

Routes a single input data signal to one of four output channels based on a 2-bit select code.

- **Data Input**: 1-bit line
- **Select Inputs**: `s1`, `s0`
- **Outputs**: $Y_0, Y_1, Y_2, Y_3$ (4 1-bit lines)
- **Logic**:
  - $Y_0 = \text{Data} \cdot \overline{s_1} \cdot \overline{s_0}$
  - $Y_1 = \text{Data} \cdot \overline{s_1} \cdot s_0$
  - $Y_2 = \text{Data} \cdot s_1 \cdot \overline{s_0}$
  - $Y_3 = \text{Data} \cdot s_1 \cdot s_0$

---

### 3-to-8 Line Decoder (`De_coder.circ`)

Decodes a 3-bit binary input vector into 8 mutually exclusive active-high output lines.

- **Inputs**: `A2` (MSB), `A1`, `A0` (LSB)
- **Outputs**: $Y_0$ through $Y_7$
- **Components**: 3 NOT gates, 8 3-input AND gates generating minterms $m_0$ to $m_7$.

---

### 9-to-4 Decimal-to-BCD Encoder (`Encoder.circ`)

Converts 9 active-high decimal inputs into standard 4-bit Binary-Coded Decimal (BCD).

- **Inputs**: $D_1$ through $D_9$
- **Outputs**: $B_3, B_2, B_1, B_0$
- **OR-Gate Logic**:
  - $B_3 = D_8 + D_9$
  - $B_2 = D_4 + D_5 + D_6 + D_7$
  - $B_1 = D_2 + D_3 + D_6 + D_7$
  - $B_0 = D_1 + D_3 + D_5 + D_7 + D_9$

---

### 2-Bit Equality Comparator (`Comparator_equal.circ`)

Evaluates equality between two 2-bit binary words $A = [A_1, A_0]$ and $B = [B_1, B_0]$.

- **Inputs**: $A_1, A_0, B_1, B_0$
- **Output**: $A=B$ (1 when $A == B$, 0 otherwise)
- **Realization**: Evaluates the 4 match conditions with 4-input AND gates:
  $$(A_1=0, A_0=0, B_1=0, B_0=0) \lor (A_1=0, A_0=1, B_1=0, B_0=1) \lor (A_1=1, A_0=0, B_1=1, B_0=0) \lor (A_1=1, A_0=1, B_1=1, B_0=1)$$

---

### 4-Bit Binary to Gray Code Converter (`BinaryToGray.circ`)

Converts a 4-bit standard binary word into reflected Gray code where successive values differ by exactly one bit.

- **Inputs**: $B_3$ (MSB), $B_2$, $B_1$, $B_0$ (LSB)
- **Outputs**: $G_3$ (MSB), $G_2$, $G_1$, $G_0$ (LSB)
- **Conversion Equations**:
  $$G_3 = B_3$$
  $$G_2 = B_3 \oplus B_2$$
  $$G_1 = B_2 \oplus B_1$$
  $$G_0 = B_1 \oplus B_0$$

---

## 4. Sequential & Storage Circuits

### Dual 4-Bit Register File (`Registers.circ`)

Implements a dual-register storage block containing two 4-bit registers (**Register A** and **Register B**) constructed from discrete edge-triggered D flip-flops with gated clock inputs.

> [!NOTE]
> The top-level circuit canvas `Reg_main` is an empty container. Open the `RegisterFile` subcircuit in Logisim's navigation explorer to view and simulate the register logic.

#### Pin Specifications

| Port | Direction | Width | Function |
|:---|:---:|:---:|:---|
| `DATAIN` / `Data_In` | Input | 4-bit | Parallel data input bus |
| `SELECTT` | Input | 1-bit | Register destination select |
| `Load_A` | Input | 1-bit | Register A write enable |
| `Load_B` | Input | 1-bit | Register B write enable |
| `Clock` / `clk` | Input | 1-bit | Synchronous system clock |
| `REG_A_OUT` | Output | 4-bit | Register A parallel 4-bit output bus |
| `REG_B_OUT` | Output | 4-bit | Register B parallel 4-bit output bus |

---

### 4-in-1 Shift Register Suite (`ShiftReg.circ`)

Contains all four fundamental shift register architectures on a single simulation canvas:

1. **SIPO** (Serial-In Parallel-Out): 4-bit serial input shifted across 4 DFF stages with parallel outputs exposed.
2. **SISO** (Serial-In Serial-Out): 4-bit serial shift pipeline with single terminal bit output.
3. **PISO** (Parallel-In Serial-Out): 4-bit parallel load via 2:1 multiplexers into 4 DFFs, followed by serial shift out.
4. **PIPO** (Parallel-In Parallel-Out): 4-bit synchronous parallel load and read.

- **Component Total**: 16 D Flip-Flops, 4 Multiplexers, 4 Clock sources, 29 Pins.

---

### 4-Bit Bidirectional Shift Register (`BiDirectReg.circ`)

A universal 4-bit shift register capable of shifting stored bits either left or right on each clock edge.

- **Components**: 4 D Flip-Flops, 8 2-input AND gates, 4 OR gates, 1 Clock pin, 1 Mode pin.
- **Steering Logic**: Each flip-flop's $D$ input is driven by an OR gate combining:
  - $\text{Right-shift path}: \text{Data}_{i-1} \cdot \overline{\text{Mode}}$
  - $\text{Left-shift path}: \text{Data}_{i+1} \cdot \text{Mode}$
- **Clock**: Synchronous positive-edge trigger.

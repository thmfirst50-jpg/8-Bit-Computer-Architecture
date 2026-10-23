# 8-Bit Computer Architecture

A fully modular 8-bit central processing unit (CPU) engineered from fundamental logic gates inside Logisim Evolution, featuring a custom ALU, register file, microcode instruction sequencing, and human-readable numeric output.

[![Simulation](https://img.shields.io/badge/Tool-Logisim%20Evolution-blue.svg)](https://github.com/logisim-evolution/logisim-evolution)
[![Architecture](https://img.shields.io/badge/Architecture-8--Bit%20Microcoded%20CPU-orange.svg)](#system-architecture)
[![Display](https://img.shields.io/badge/Output-BCD%20%2B%207--Segment%20Display-green.svg)](#bcd-conversion--7-segment-display)

---

## 1. Project Overview

This project implements a complete, working von Neumann computer architecture simulated from discrete logic gates in **Logisim Evolution**. It bridges the gap between digital logic design and microprocessor architecture by realizing every core CPU component from primitive gates—without using pre-packaged high-level CPU blocks.

Key features include:
- A custom 8-bit Arithmetic Logic Unit (ALU) with arithmetic, logic, and condition flags.
- A $16 \times 8$-bit multi-port Register File for high-speed operand storage.
- Microcode-driven instruction decoder and centralized control unit.
- Program Counter (PC), Memory Address Register (MAR), and Instruction Register (IR).
- Binary-Coded Decimal (BCD) conversion circuitry driving hardware 7-segment display readouts.

---

## 2. System Architecture & Datapath

The CPU datapath coordinates data movement between the register file, ALU, memory, and output display across a common internal bus.

```
                  +-----------------------------------+
                  |         Instruction ROM           |
                  +-----------------+-----------------+
                                    |
                                    v
+---------------+         +---------+---------+         +-----------------+
| Program       |         | Instruction       |         | Microcode /     |
| Counter (PC)  |         | Register (IR)     | ------> | Control Unit    |
+-------+-------+         +-------------------+         +--------+--------+
        |                                                        |
        v                                                        | Control
+-------+-------------------+                                    | Signals
| Memory Address Reg (MAR)  |                                    |
+-------+-------------------+                                    v
        |                                               +--------+--------+
        v                                               | 16 x 8-Bit      |
+-------+-------------------+                           | Register File   |
| RAM Memory (256 x 8-Bit)  |                           +--------+--------+
+-------+-------------------+                                    |
        |                                                        | Operands
        +---------------------------> Internal Bus <-------------+
                                           |
                                           v
                               +-----------+-----------+
                               | 8-Bit Arithmetic      |
                               | Logic Unit (ALU)      |
                               +-----------+-----------+
                                           |
                                           v Flags (Z, C, N, V)
                               +-----------+-----------+
                               | BCD Converter &       |
                               | 7-Segment Displays    |
                               +-----------------------+
```

---

## 3. Core Subsystems

### 1. Arithmetic Logic Unit (ALU)
- **Arithmetic**: 8-bit Addition and Subtraction (via two's complement inversion and carry-in assertion).
- **Bitwise Logic**: AND, OR, XOR, NOT, Logical Shift Left (SHL), and Logical Shift Right (SHR).
- **Status Flags Register**:
  - **Zero ($Z$)**: Asserted when the ALU result equals $00000000_2$.
  - **Carry ($C$)**: Asserted on unsigned arithmetic overflow.
  - **Negative ($N$)**: Reflects the most significant bit ($MSB = 1$).
  - **Overflow ($V$)**: Asserted on signed two's complement arithmetic overflow.

### 2. $16 \times 8$-Bit Register File
- Sixteen general-purpose 8-bit registers ($R_0$ through $R_{15}$).
- Dual read ports allowing simultaneous readout of two source operands within a single clock phase.
- Synchronous write-enable gating controlled by instruction microcode.

### 3. Microcoded Control Unit
- Decodes the operation code (OpCode) latched in the Instruction Register.
- Generates synchronized control word signals across fetch, decode, execute, and memory-writeback cycles.
- Manages conditional branching based on the state of ALU condition flags.

### 4. BCD Conversion & 7-Segment Display
- Integrates a hardware implementation of the **Double-Dabble** (Shift-and-Add-3) algorithm.
- Converts 8-bit binary numbers ($0 - 255$) into three 4-bit Binary-Coded Decimal nibbles (Hundreds, Tens, Units).
- Drives three multiplexed 7-segment LED display decoders for real-time numeric output during program execution.

---

## 4. Technical Specifications

| Parameter | Specification |
| :--- | :--- |
| **Architecture** | 8-Bit von Neumann CPU |
| **Word Size** | 8-Bit Data, 8-Bit Address space (256 bytes) |
| **Simulation Tool** | Logisim Evolution |
| **Design Methodology** | Gate-level logic synthesis (NAND, NOR, AND, OR, XOR, MUX) |
| **Register File** | $16 \times 8$-Bit dual-read general-purpose registers |
| **ALU Operations** | ADD, SUB, AND, OR, XOR, NOT, SHL, SHR |
| **Flags** | Zero ($Z$), Carry ($C$), Negative ($N$), Overflow ($V$) |
| **Display Subsystem** | Hardware BCD Double-Dabble converter + 3× 7-segment displays |

---

## 5. Repository Contents

This repository documents the architectural specifications, subcircuit schematics, microcode state sequencing, and Logisim Evolution simulation files for the 8-bit computer architecture.

---

## Author

**Mohammad Safi** — Electrical Engineering, University of Houston.  
Specializing in embedded systems, digital design, and machine learning.

- **Portfolio**: [thmfirst50-jpg.github.io](https://thmfirst50-jpg.github.io/)
- **LinkedIn**: [linkedin.com/in/safi0](https://linkedin.com/in/safi0/)

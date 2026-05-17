# Bin Packing Problem Solver — MIPS Assembly

A MIPS assembly implementation of the **Bin Packing Problem** using two classical heuristics: **First Fit (FF)** and **Best Fit (BF)**.

> **Course:** ENCS4370 — Computer Architecture, Birzeit University  
> **Semester:** Spring 2024/2025  
> **Instructor:** Dr. Aziz Qaroush  
> **Authors:** Hala Sabobeh (1220322) · Nour Omarii (1221212)

---

## Problem Description

Given a collection of `n` items with sizes `S₁, S₂, … Sₙ` (each a floating-point number in `(0, 1]`), pack them into the **minimum number of unit-capacity bins**.

### Heuristics Implemented

**First Fit (FF):** For each item, scan bins in order and place the item in the first bin with enough remaining capacity. If none fits, open a new bin.

**Best Fit (BF):** For each item, find the *fullest* bin that still has enough space and place the item there. If none fits, open a new bin.

---

## Features

- Interactive menu loop (exits on `q` / `Q`)
- Reads item sizes from a user-specified input text file
- Input validation — rejects files that don't exist or contain values outside `(0, 1]`
- Writes results to a user-specified output text file
- Reports the total number of bins used and the items packed in each bin
- Custom `string_to_float` and `int_to_string` conversion routines (no high-level syscalls for parsing)
- Windows (`\r\n`) and Unix (`\n`) line-ending support

---

## File Structure

```
.
├── Project1_1220322_1221212.asm   # Final submission — main source file
├── binpacking.asm                  # Development / draft version
└── README.md
```

---

## How to Run

The project is designed for **MARS** (MIPS Assembler and Runtime Simulator).

1. Download [MARS](http://courses.missouristate.edu/KenVollmar/mars/) if you don't have it.
2. Open MARS and load `Project1_1220322_1221212.asm`.
3. Assemble (`F3`) then run (`F5`).
4. Follow the on-screen prompts.

---

## Usage

```
Bin Packing Problem Solver - MIPS Assembly

Menu:
1. First Fit (FF)
2. Best Fit (BF)
Q. Quit
Enter your choice:
```

After selecting an algorithm:
- You will be prompted to enter the **input file path** (e.g. `items.txt`).
- You will be prompted to enter the **output file path** (e.g. `results.txt`).

### Input File Format

One floating-point value per line, each strictly between `0` and `1`:

```
0.4
0.7
0.2
0.5
0.1
0.8
```

### Output File Format

```
Total bins used: 3
Bin 1 Items: 1 3 5
Bin 2 Items: 2 4
Bin 3 Items: 6
```

---

## Constraints & Limits

| Parameter | Value |
|---|---|
| Maximum items | 100 |
| Maximum bins | 100 |
| Maximum items per bin | 10 |
| Bin capacity | 1.0 |
| Valid item size range | (0.0, 1.0] |

---

## Implementation Notes

- Floating-point arithmetic uses the MIPS FPU (`add.s`, `sub.s`, `c.le.s`, etc.).
- A small `EPSILON = 0.000001` is defined for safe float comparisons.
- The stack is used for all callee-saved registers (`$s0–$s7`, `$ra`) following standard MIPS calling conventions.
- `bin_space` tracks how much space has been *consumed* in each bin (not remaining), so remaining capacity = `1.0 − bin_space[i]`.

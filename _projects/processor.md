---
layout: page
title: Out-of-order Processor
description: An out of order processor written in verilog for the RISC-V ISA
img: assets/img/oopdesign.png
importance: 3
category: work
related_publications: false
---

# RISC-V Out-of-Order Processor 

**Date completed**: 2024-12-14 
**Contributors**: Gregory Payton & Heran Yang 

## Overview
In this project, we implemented an **Out-of-Order Processor** using **Verilog**, demonstrating an understanding of modern proccessor design principles that improve performance. The processor leverages out-of-order execution to optimize throughput while maintaining program correctness. This project, developed collaboratively me and my partner, Heran Yang, showcases our ability to work together in a team to design, simulate, and validate a complex digital system. By starting early, using iterative and test driven developement, and working extremely hard, we completed this impressive project over the span of six weeks during our fall quarter.

## Table of Contents
- [Overview](#overview)
- [Background Information](#background-information)
- [Objective](#objective)
- [Technologies Used](#technologies-used)
- [System Architecture](#system-architecture)
- [Implementation](#implementation)
- [References](#references)

## Background Information
The "Iron Law" of processor performance states that:

> CPU Time = IC x CPI x CT 
> where:
> IC = Instruction Count, CPI = Cycles Per Instruction, CT = Cycle Time
 
Considering the CPU time of a simple processor (single-cycle and in-order), a few tricks can be used to decrease CPU time. 

We can use **pipe-lining** which splits the processor into different stages, allowing us to clock faster, and utilize our hardware more efficiently. This decreases our CT.

We can use instruction-level **parallelism** by duplicating our hardware N times to run N instructions at a time. A pipelined processor that can issue/processor multiple instructions simultaneously is called **Super Scalar**. This increases IPC.

This approach however is unable to make full use of it's added hardware because of data dependencies between instructions. To get the most out of our super scalar we need **out-order execution**. Out-of-order execution allows instructions to be executed in any order, which is helpful for filling up the pipelines as long as independent instructions can be found. The trade-off for this speed is added complexity and hardware to address the problems that arise when things are done out of order.

**RISC-V** is an open source Instruction Set Architecture (ISA) that we'll be using for our implementation.

## Objective
The main objective is to create an out-of-order processor capable of supporting the following RISC-V instructions: ADD, ADDI, LUI, ORI, XOR, SRAI, LB, LW, SB, SW.

Specifications:
- 32 bit instruction size
- Standard ALU instructions
- 2-Issue processor
- 1 fetch per CC
- Assume no stalling, except at the issue stage

This can be broken down into many sub-objectives regarding the execution flow of the processor. Execution flow generally has around 7 pipeline stages: 

1. Fetch (inorder)
2. Decode (inorder)
3. Rename (inorder)
4. Dispatch (inorder)
5. Issue (out-of-order)
6. Complete (out-of-order)
7. Retire (inorder)

These can be further broken down into the modules making up each stage. See below for more details. Modules are individually tested and then connected in a top-level module and tested again. The best path of development was completing the stages in order so that new  modules could immediately be tested with the rest of the design. This drastically cut down on the time we spent doing verification and testing.


## Technologies Used

- Verilog
- EDA Playground

We both have macs so we were unable to install a proper Verilog IDE like Vivado, but we made it work with EDA playground, an online IDE that works quite well.


## System Architecture

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/oopdesign.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Pictured above is the architecture level diagram of the processor. This provides a high level overview of how each sub-block interacts with one another. It mainly shows the datapath which is useful to visualize the life-cylce of an instruction. A lot more signals are needed for the design but for readability purposes, they are not shown in the diagram.

## Implementation

Below are details on the modules that makes up the design. 

### Fetch

Submodules

- PC
- PC Adder
- Instruction Read Only Memory (ROM) (4KB)

**Inputs: None
Outputs: 12 bit PC, 32 bit instruction**

The fetch module handles the storage and increment of the program counter. The program counter is used to index the Instruction ROM. 

### Decode

Submodules

- Controller
- Immediate generator
- ALU controller

**Inputs: 12 bit PC
Outputs: 12 bit PC, 7 bit control signals, 32 bit immediate, 3 bit ALU operation signal**

The Decode modules takes in the 32 bit instruction and generates the corresponding control signals and immediate for it. 

### Free Pool

- Contains a stack of unused physical registers
- Initialized with registers 63 (top) to 32
- Maximum depth of 32

The Free Pool pops of a register for renaming when the instruction has a destination register (pop signal is 1). It pushes up to two freed physical registers when instructions are retired from the ROB (push1 & push2 signals are 1). Popping happens on the positive edge of the clock and pushing happens on the negetaive edge.

### A/P Reg File

- Contains 32 entry Register Alias Table (RAT).
- RAT is initialized with 1 to 1 mapping, i.e. 0->0, 1->1, etc.
- 64 Entry ready table for physical registers
- 64 Entry, 32 bit memory for physical registers

There is no actual architectural register file. Instead this register holds mappings from architectural to physcial register tags. The associated tags are then used to index the acutal physical registers. More physical than architectural registers is essential for renaming which gets rid of false dependecies. 


### Reservation Station

- 64 entries, 157 bits per entry
- Stores instruction data and relevant signals
- Can issue up to 3 instructions at a time
- Forwarding from ALUs updates register data and ready status

During dispatch, the reservation station or unified issue queue reserves an entry for the instruction, assigning it an FU and an ROB number. At the start of each clock cycle, any instruction from the issue queue can be issued as long as it's source registers and assigned FU is ready. The RS also gets written back to on the negative edge of the clock, updating any register data and ready signals sent back from the execution stage.

### FU Table

- 3, 1 bit entries
- Updates at the negative edge of the clock
- FU updates take priority over RS updates.

The FU table stores the current execution status of each of the three functional units (ready or not ready). The FU outputs these signals to the RS so it knows which instructions it can issue. The RS and FUs update the FU table at the negedge of the clock. The RS updates not ready statuses when issuing, and the FUs update ready statuses when finishing an instruction. Thus, the FU updates will take priorty, meaning we'll OR the updates together. A not ready (0) and a ready (1) result in the FU being ready.

### ALU

- 3 ALUs
- Support XOR, OR, AND, ADD, SHIFT_LEFT, SHIFT_RIGHT
- FU status gets written back to FU Table
- FU 3 is for memory operations (load & store)
- FU 1 & 2 are for all other operations
- Result 3 goes to the LSQ
- Result 1 & 2 go to the Complete stage


### Reorder Buffer

- 64, 58 bit entries
- Stores use bit, rd, old rd, pc, result, complete bit

The ROB reserves an entry at the same time as the RS, with the RS using the corresponsding ROB entry number. Whenever an instruction is completed, it's results are written to the ROB and it's complete bit is set. Starting from the head of the ROB, instructions are retired in order as long as all other instructions before it are also complete. Upon retiring, the data is written back to the physical register files and forwarded to the reservation station since we capture register data as soon as an entry is reserved. We also push the old destination register back onto the free pool. We can retire up to two instructions at per cycle.

### Load Store Queue

- Buffers load and store operations with PC as ID
- Also stores associated instruction information 
- Reserve entries @(posedge clk)
- Addresses are received from FU #2
- Update Logic @(negedge clk)
    - If updating a load, update address and scan previous entries for forwarding from matching stores
    - If a store, update address and data
- Issuing Logic @(posedge clk)
    - If memory is not busy and hasn’t just finished
        - If a load check if it can bypass memory
        - If not, send it to memory
        - Else send store to memory
    - Otherwise we wait
        - While we wait check to see if an instruction finishes 
    - If an instruction finishes put it on BUS2

### Memory

- Simulates latency using a counter and taking snapshots of initial signals
- Busy and Done flags
    - While Busy we wait
    - When Done, LSQ handles result
- Is_byte flag, comes from instruction information stored in LSQ
- If is_byte is set
    - If store, only wire least significant byte of data
    - If a load, sign extend the retrieved byte
- Otherwise store and load at word granularity

## Conclusion

This was one of the coolest and most enjoyable projects I've done at UCLA so far. Shout out to Professor Sehat for making this class and honors seminar a great experience. If it weren't for him I wouldn't like hardware as much as I do now. Also big thanks to my teammate for sticking it out with me, despite not even begin enrolled in the class. 

## References

- [Verilog Tutorial](https://www.chipverify.com/tutorials/verilog)
- [EDA Playground](https://edaplayground.com/home)


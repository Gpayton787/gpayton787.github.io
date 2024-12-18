---
layout: page
title: Single-Cycle Processor Simulator
description: A C++ program that simulates a single-cycle processor for the RISC-V32 ISA
img: assets/img/singlecyclediagram.png
importance: 4
category: work
related_publications: false
---
## Table of Contents
- [Overview](#overview)
- [Background Information](#background-information)
- [Goals](#goals)
- [Implementation](#implementation)
- [Results](#results)

## Overview
This is the first project in the class ECE-M116 Computer Systems Architecture. In this project, we were tasked with designing and implementing a single-cycle processor for the RISCV-32 [ISA](#background-information). This process begins with a design of the data path & controller, followed by the implementation of the design in C++. 

## Background Information
This section serves to provide some necessary background knowledge to readers who are not familiar with the material. 
- A **processor**, also known as a CPU, is a hardware unit that executes instructions for your computer. Every program that runs on your computer is eventually boiled down into a set of assembly instructions that the hardware understands. This set of instructions is called the **Instruction Set Architecture (ISA)**, and it serves as an interface between your computer's software and hardware
- A processor's job is simple: fetch and execute instructions and store their results. How it achieves this, however, is a different story. Processors vary in their implementation and, thus, their performance. In this project, we build a **single cycle** processor, meaning each instruction takes 1 complete clock cycle to run to completion. This is perhaps the simplest CPU design, which is 100% correct, but it's just TOO slow. Modern processors are **multi-cycle**, meaning the CPU's workflow is split into multiple stages, allowing us to increase the clock speed and better use the CPU's hardware at each stage. However, techniques to increase the speed and efficiency of a processor result in added complexity and correctness issues that are difficult to address. Thus, we'll stick with the single-cycle processor for now. 


## Goals
The goals for this project are to design a datapath & controller that supports the following instructions.
```
ADD, LUI, ORI, XOR, SRAI

LB, LW, SB, SW

BEQ, JAL
```
The design should be based on the datapath & controller we learned about in class, pictured below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/defaultdatapath.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## Implementation
To begin, I compiled a table of the instructions and their opcodes (similar to an instruction ID) along with their corresponding control signals. These control signals will be manipulated throughout the datapath to ensure the correct execution of each instruction.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/singlecycletable.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


Each control signal serves a different purpose depending on the behavior of the instruction. For example, ALUSRC is a bit that is set to 0 if the soruce to the ALU should be a register or 1 if it should be an immediate. I then designed the data path. This was an incremental process, as I made many changes to the initial design as I progressed through the coding part.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/singlecycle.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    In the design above a few notable changes were made to the original data path. A byte signal was added to the controller for B-type instructions. Logic was added to support the JAL instruction. The ALUOP was extended to 3 bits to support more operation types.
</div>

Implementing the design in C++ was straightforward in principle but tedious in practice. To illustrate, let's take a look at some of the class definitions of the equivalent hardware components in the CPU.

I defined many constants as strings to make them readable and make debugging easier.
```C
[label CPU.h]
//GLOBAL STUFF
const int INSTMEM_SIZE = 4096;
const int DMEM_SIZE = 4096*4;
//OPCODES
const string R_TYPE_OP = "0110011";
const string I_TYPE_OP = "0010011";
const string L_OP = "0000011";
const string S_OP = "0100011";
const string BEQ_OP = "1100011";
const string JAL_OP = "1101111";
const string LUI_OP = "0110111";
//FUNCT3s
const string F3_BYTE = "000";
const string F3_WORD = "010";
const string F3_SHIFT = "101";
...
```

For each hardware component in the CPU a corresponding class or function was defined. For example here is the class definition of the CPU itself. It takes care of the program counter (PC) and fetches instructions. It also handles DRAM which is on chip-memory that is much slower than the register file.

```C
class CPU {

public:
    CPU();
    long readPC();
    void setPC(long next_pc);
    void incPC();
    void fetch(char* instMem); //Fetches a single instruction relative to PC
    Instruction get_inst();
    int read_mem(int index, bool MemRe, bool isByte);
    void write_mem(int index, int val, bool MemWrite, bool isByte);


private:
    char dmemory[DMEM_SIZE]; //data memory byte addressable in little endian fashion;
    long PC; //pc
    Instruction inst;
};
```
Here is the definition of the register file. It allows you to read and write to the register file. Each register is 32 bits long.

```C
//WORKS WITH INTS
class Reg_file {
    public:
        Reg_file() {
            std::fill(std::begin(arr), std::end(arr), 0);
        }

        int get_reg(int rs);
        void set_reg(int rd, int val, bool reg_write);

    private:
        int arr[32];
};
```
Here are some other definitions of components. The ALU is defined as a class and handles all computations. Other components are defined as functions. For example the MUX, immediate generator, and control units are all functions. This is mostly a matter of preference. If the inputs and outputs are simple and very combinational I opted to use a function. If there are many moving parts and return values I found it easier to define a class with each return value as a member variable.
```C
class ALU {
    public:
        void compute(int arg1, int arg2, bitset<3> alu_op);
        int get_result(){ return result; };
        bool get_zero_flag(){ return zero; };

    private:
        bool zero;
        int result;
};

//Returns a 6 bit value where msb to lsb is REGWRITE | ALUSRC | BRANCH | MEMRE | MEMWR | MEMREG
bitset<7> control(bitset<7> opcode, bitset<3> funct3);

bitset<3> alu_control(bitset<7> opcode, bitset<3> funct3);

bitset<64> imm_generator(Instruction Instr, bitset<7> op_code, bitset<3> funct3);

long two_bit_mux(bool sig, long arg0, long arg1);
```
Overall the implementation followed pretty easily from the design. The hardest part of the project was handling all the bit operations correctly and testing instructions to ensure their correctness. Actually about half of the project was just writing test cases in assembly code and then manually converting them to machine code in the input format required for the simulator.

## Results
After a few days and more time debugging than actually coding, I finished the project and received a 100/100, passing all tests on Gradescope. Overall, the project was very enjoyable and informative. Building a simulator from scratch ensures you really know what's going on at each step in the CPU. If you liked this project, make sure you stay tuned for the Out-of-Order processor, which is going to be blazingly fast compared to this one. However, while executing instructions out-of-order may increase speed and efficiency, it also raises new problems and complexity that will need to be addressed... next time.
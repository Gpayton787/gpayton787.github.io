---
layout: page
title: Interpreter
description: An interpreter written in python for a brand new programming language called Brewin
img: assets/img/interpreter_diagram2.png
importance: 2
category: work
related_publications: false
---

## Programming Language Interpreter
- [Overview](#overview)
- [Version 1](#version-1)

### Overview
In this project we build an interpreter for a new programming language called **Brewin**. The interpreter itself is written in Python and is capable of supporting any valid Brewin program. This is a quarter-long project and is broken down into four increasingly complex versions. Each new version of the interpreter adds support for more programming language paradigms.

## Background Knowledge

After writing a program in your chosen language, you need to somehow execute it. The two main ways to do so are with an interpreter or a compiler.
- An **interpreter** is a program that takes another program as input, executes it line-by-line, and returns the output. Languages that use an interpreter include Python, Javascript, and Ruby.
- A **compiler** is a program that translates your program source code into machine code in a process of many steps called compilation. After compilation, your CPU can execute the resulting executable file.
- Each method has **pros and cons**. Interpreting avoids a lengthy compilation process but may lead to more run-time errors. Compiled code runs much faster and cathches bugs at compile time, but compilation is slow.
- An **Abstract Syntax Tree (AST)** is a tree of nodes that holds a hierarchical respresentation of a program. The tree format allows the program to be executed by a simple traversal. In this project we are given a parser module that will take a valid program string as input and output the corresponding AST.

## Version 1

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreter_diagram.jpg" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Version 1 supports:
- Function defintions, Statements, Expressions, Variables, Constants

**Execution Flow Pseudocode**
```
def interpret_program(program_string):
    generate the AST from parser module
    create a variable map
    find and run the main function

def run_function:
    for each statement in function
            run statement

def run_statement:
    if defintion
        handle variable definition
    if assignment 
        assign variable
    if function call
        run function

def define_var:
    ...

def assign_var
    ...

def function_call:
    ...
```

## Version 2

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreter_diagram2.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Version 2 supports:

- Function defintions, Statements, Expressions, Variables, Constants, **Function Parameters, Boolean Variables, Nil Datatype, Binary Integer Arithmetic Operations, Integer Comparison, Boolean Comparison, String Operations, If-else statements, For loops, Static scoping, Recursion, Return Statements**


## References
- [V1 Project Spec](https://docs.google.com/document/d/1npomXM55cXg9Af7BUXEj3_bFpj1sy2Jty2Nwi6Kp64E/edit?tab=t.0#heading=h.63zoibjlqvny)
- [Fall 2024 Course Website](https://ucla-cs-131.github.io/fall-24-website/)

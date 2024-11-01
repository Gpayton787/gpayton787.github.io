---
layout: page
title: Interpreter
description: An interpreter written in python for a brand new programming language called Brewin
img: assets/img/interpreter_diagram.jpg
importance: 2
category: work
related_publications: false
---

## Programming Language Interpreter
- [Overview](#overview)
- [Version 1](#version-1)

### Overview
In this project we build an interpreter for a new programming language called **Brewin**. It's described as the "lovechild of C++ and Python, with a little EMACS lisp mixed in." The interpreter itself is written in Python. This is a quarter-long project and is broken down into three sub-projects consisting of Versions 1,2, and 3 of the interpreter. Each new version adds newer and more complex functionality. I'm taking this class in Fall 2024 of my junior year when it's taught by my good friend Professor Nachenberg (He probably doesn't know who I am).

## Background Knowledge

After writing a program in your chosen language, it comes time to run it. If only there were something you could give your program to that understands precisely how to understand and run it.
- An **interpreter** is a program that takes another program as input, executes it line-by-line, and returns the output. Languages that use an interpreter include Python, Javascript, and Ruby.
- A **compiler** is a program that translates your program source code into machine code in a process of many steps called compilation. After compilation, your CPU can execute the resulting executable file.
- Each method has **pros and cons**. Interpreting avoids a lengthy compilation process but may lead to more run-time errors. Compiled code runs much faster, and lots of bugs can be caught at compile time, but of course, compilation can be slow. 
- Every programming language has several features that must be supported by the compiler or interpreter, including things like **variables, functions, typing systems, recursion, scoping, and casting & converions**, just to name a few. Thus, an interpreter will be a large program that is robust and capable of executing any valid progam in that language.
- An **Abstract Syntax Tree (AST)** is a tree of nodes that holds a hierarchical respresentation of a program. The tree format allows the program to be executed by a simple traversal. In this project we are given a parser module that will take a valid program string as input and output the corresponding AST.

## Version 1

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreter_diagram.jpg" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Here is my attempt to visually represent the interpreter and it's working parts.

1. The interpreter is initialzed with two member variables: a map of variables and a pointer to an AST, initialized to null.
2. The interpreter is passed a valid program string, which is then passed to parser module, which returns the AST for the program
3. The AST is then processed by a set of functions. The first function finds the main function which is the entry point to the program and runs that function. Functions are run by extracting and running their statement nodes (brown nodes above). These brown nodes are redirected based on their statement type to functions that will process and handle them. 
4. Once the AST is raversed, the interpreter stops as the program has run to completion.

### Features
The goal for part 1 of this project is to implement an interpreter that supports simple programs with the following features of the Brewin language:
- Function Definitions
- Statements
    - Variable Definition
    - Assignment
    - Function Call
- Expressions
- Variables
- Constants

## References
- [V1 Project Spec](https://docs.google.com/document/d/1npomXM55cXg9Af7BUXEj3_bFpj1sy2Jty2Nwi6Kp64E/edit?tab=t.0#heading=h.63zoibjlqvny)
- [Fall 2024 Course Website](https://ucla-cs-131.github.io/fall-24-website/)

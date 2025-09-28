---
layout: page
title: Brewin Interpreter
description: An interpreter written in Python for a brand new programming language called Brewin
img: assets/img/interpreterv4.png
importance: 2
category: work
related_publications: false
---

## Programming Language Interpreter
- [Overview](#overview)
- [Brewin](#brewin-v1)
- [Brewin++](#brewin++)
- [Brewin#](#brewin#)

### Overview

**Contributors:** Gregory Payton
**Date:** September-December 2024

As part of the fall offering of CS131 at UCLA, professor Carey Nachenberg has devised a new programming lanuage called Brewin. Given the language specification, it is up to us, the students, to build an interpreter that can run any valid Brewin program. Over the quarter we built four versions of the interpreter to support three different versions of the Brewin programming language.


1. interpreter v1 supports a subset of Brewin
2. interpreter v2 supports the full Brewin language
3. interpreter v3 supports Brewin++
4. interpreter v4 supports Brewin#

Each languages support its own set of features and paradigms. This diagram made by a TA shows how each language is related:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/venn.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Background Knowledge

After writing a program in your chosen language, you need to somehow execute it. The two main ways to do so are with an interpreter or a compiler.

- An **interpreter** is a program that takes another program as input, executes it line-by-line, and returns the output. Languages that use an interpreter include Python, Javascript, and Ruby.
- A **compiler** is a program that translates your program source code into machine code in a process of many steps called compilation. After compilation, your CPU can run the resulting executable file.
- Each method has **pros and cons**. Interpreting avoids a lengthy compilation process but may lead to more run-time errors. Compiled code runs much faster and cathches bugs at compile time, but compilation is slow.
- An **Abstract Syntax Tree (AST)** is a tree of nodes that holds a hierarchical respresentation of a program. The tree format allows the program to be executed by a simple traversal. In this project we are given a parser module that will take a valid program string as input and output the corresponding AST.

## Brewin

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreter_diagram.jpg" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A visualization of the intepreterv1.py program. *These visualizations will only provide a high-level view of how the interpreter runs. They were helpful for me to create when developing the project and also sort of fun to look at, but they DO NOT necessarily encapsulate all the details of the program.
</div>

### Interpreter V1

The first version of the interpreter is very basic, supporting many of the fundamentals you'd expect from a programming language. It supports only a small subset of the Brewin language:

- Functions
- Statements
- Expressions
- Variables 
- Constants
- Printing
- User Input

The development of interpreter V1 went smoothly. I tried to implement a very organized structure that would be easy to add on to in later projects. I ended up using my original project 1 solution to build off of for project 2 instead of the official solution, so I'd say I suceeded in making it future-proof.

### Interpreter V2

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreter_diagram2.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A visualization of the intepreterv2.py program. Notice a new environment stack in green. A lot of different functions need to access the stack. All the green blocks represent functions modifying the stack.
</div>

The second iteration of the interpreter adds support for the entirety of Brewin. 

New features:

-  Function Parameters
-  Boolean Variables
-  Nil Datatype
-  Binary Integer Arithmetic Operations
-  Integer Comparison
-  Boolean Comparison
-  String Operations
-  If-else statements
-  For loops
-  Static scoping
-  Recursion
-  Return Statements

The hardest part of interpreter version two was implementing static scoping. At first I treated the environment stack as a literal stack. This made it difficult to access variables in different scopes when implementing shadowing. I opted to use a solution involving variable inheritance which worked, but was unnecessarily complicated. For example, for an *if statment* to access a variable *x* in it's parent scope, it inherits a local copy of *x*, and upon leaving the block, merges its copy of *x* with the parent copy. For simplicity I ended up changing my solution to that of the official solution. To implement scoping, this solution traverses the stack upwards whenever you needed access to a variable further up the stack. In this way, you get rid of any inheritance and merging, making scoping much easier to implement.

## Brewin++

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreterv3.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A visualization of the intepreterv3.py program. Notice that it's built off of interpreterv2.py. The changes are highlighted above.
</div>

New features:

- Static Typing
- Default Return Values from Functions
- Coercion
- Structures

At first static typing didn't sound that hard, but as you can see from the above visualization, A LOT of type checking needs to be done. Because of the amount of type checking code that needed to be added, this was the toughest out of the three projects. Implementing the rest of the features was straightforward. Structures were very fun to implement, especially the implementation of the recursive dot operator.

## Brewin#

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/interpreterv4.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    A visualization of the intepreterv4.py program. Notice that it's built off of interpreterv2.py. The changes are highlighted above
</div>

New features:

- Need Semantics
- Lazy Evaluation
- Exception Handling
- Short Circuiting

Interpreter V4 was my favorite of the four projects. Need semantics and lazy evaluation required making a new object which I called "Expression Object". This object held the expression node, a snapshot of the current scope, an evaluated flag, and a result. Expression Objects are passed around and only evaluated when necessary (In standalone function calls like print statements). When evaluated, their result is cached, and their flag is set. Any further evaluations of this object will use the cached result. Exception handling was fun and straightforward to implement. In project 2, we already created an Execution Status enum, which was returned by functions and blocks. To implement error raising, a new execution status, RAISE, was added to the enum. Short circuiting was very easy, only a few lines were added in the evaluate_expression function to check for this.


## References
- [Project Specs](https://ucla-cs-131.github.io/fall-24-website/projects/)
- [Fall 2024 Course Website](https://ucla-cs-131.github.io/fall-24-website/)

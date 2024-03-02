---
layout: post
title: "Book review: Engineering a Compiler"
date: 2024-02-11
---

[<img src="/images/compiler_sketch.png">](/images/compiler_sketch.png)

I recently read [Engineering a Compiler](https://www.amazon.com/Engineering-Compiler-Keith-D-Cooper-dp-0128154128/dp/0128154128) by Cooper and Torczon, and it was honestly a real page turner for me. I read this book because I was clueless about compilers and saw this as an opportunity to become a better engineer. Turns out this was the perfect introduction and deep dive into the subject. The book is exceptionally well written! After reading it I felt like Neo in the Matrix when he learned kung fu.

Here are some key concepts and algorithms that really resonated with me:

### Layers

A compiler is fundamentally composed of layers of software that, when combined, perform a remarkable task (taking human readable instructions and converting it to 1s and 0s!) But each layer is self contained, and to a certain extent, simple. This is quite beautiful.

### Finite automata

An automata is just a state machine. In a compiler we see it used for [scanning](https://en.wikipedia.org/wiki/Lexical_analysis#Scanner), where it is used to recognize words in the source text. For example, it will encounter a character and essentially ask, “should I combine this with the last few characters I've seen and tag it as a variable name?”

We also see automata used in [parsing](https://en.wikipedia.org/wiki/Parsing), where the compiler recognizes whole lines of code by piecing together constituent words. The state machine maintains an understanding of the line by considering the next word and every possible state it could entail.

### Static Single Assignment ([SSA](https://en.wikipedia.org/wiki/Static_single-assignment_form))

A cornerstone technique in program understanding, and very relevant today, showing up in modern compilers like [LLVM](https://en.wikipedia.org/wiki/LLVM). The idea is surprisingly simple: each time a variable gets assigned, we represent that with a unique reference, regardless as to how it's used later (e.g. if that variable gets reassigned, we just create another unique reference then.)

By representing each assignment uniquely, we can confidently trace how data flows through all the downstream code. Indeed, it's an essential technique behind [Data Flow Analysis](https://en.wikipedia.org/wiki/Data-flow_analysis), and unlocks the ability to do optimization. Data flow analysis can answer questions like, which variables are no longer used here, or what invariant is true about this variable (e.g. does it have some algebraic property we can use to our advantage?)

### Value numbering

This is a technique for finding redundant computation. What struck me was how simple it was. In the simplest form the algorithm goes through lines of binary operations (`x = a <op> b`), and labels each value with a number. When it sees values it has already labeled it can reuse that previous number as the label for the current assignment.

### Graph coloring for register allocation

An application of graph theory that I found fascinating. On a physical CPU we are constrained to a set of registers and the code we compile must use no more than what's available. In order to determine conformance we need to determine which variables are alive between any two lines of code. If we represent a variable's live range as a graph node, and connect nodes where the ranges [intersect](https://en.wikipedia.org/wiki/Register_allocation#Graph-coloring_allocation), then solving the graph coloring problem is the same as answering the question of whether we have enough registers. So cool.

---

Going forward I'm excited to apply and build on the knowledge from this book. Already I found myself being able to grok things in the [Chrome V8 blog](https://v8.dev/blog) I hadn't been able to before. I highly recommend this book if you want to learn about compilers!

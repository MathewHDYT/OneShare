---
layout: default
title: Ownership
parent: Rust
grand_parent: Notes
---

## Ownership
Contains some of the basic general concepts of ownership and how Rust solves memory management without a garbage collector.

### Basics
Set of rules that govern how Rust manages memory.

#### Approaches to manage memory
1. Garbage collection, regular looks for no-longer used memory as the program runs
2. Memory management, program must explicitly allocate / free memory
3. Ownership, memory is managed through set of rules that compiler checks

#### Stack / Heap
Parts of memory available to use at runtime, but structured in different ways.

##### Stack
Stores values in order it gets them / removes them in the opposite order --> last in, first out.

**Adding Data** --> Pushing onto the stack 

**Removing Data** --> Popping off the stack 

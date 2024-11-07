---
title: Code Generation Passes
weight: 10
menu:
  notes:
    name: Code Generation Passes
    identifier: notes-llvm-codegenpasses
    parent: notes-llvm
    weight: 10
---


LLVM's code generator splits the code generation problem into individual passes
- instruction selection
- register allocation
- scheduling
- code layout optimization
- assembly emission

There are also many builtin passes that are run by default.

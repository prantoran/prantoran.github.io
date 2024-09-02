---
title: Angr
weight: 210
menu:
  notes:
    name: Angr
    identifier: notes-cybersecurity-angr
    parent: notes-cybersecurity-decompilers
    weight: 10
---


<!-- Angr -->
{{< note title="Angr" >}}


Rules:
- EagerReturnsSimplifier
  - Adds additional return statements to the decompiled code to improve readabilit of the code, if the number of the "in edges" for the return node (i.e., in-degree of the return site) is less than a specified threshold

Core libraries:
- SequenceWalker
  - Used to traverse graphs

For each decompiled function, angr constructs a corresponding abstract syntax tree (AST).

When angr modifies the CFG (e.g., applies EagerReturnsSim- plifier), angr calls SequenceWalker to traverse the graph and modify nodes, e.g., insert additional return statements on the AST.

Ijk_Boring is used to handle the conditional branch instruction.

{{< /note >}}
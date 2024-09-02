---
title: Ghidra
weight: 210
menu:
  notes:
    name: Ghidra
    identifier: notes-cybersecurity-ghidra
    parent: notes-cybersecurity-decompilers
    weight: 10
---


<!-- Ghidra -->
{{< note title="Ghidra" >}}

Internally, Ghidra uses debug information, stored in the binary in the **DWARF** format, from binary to help recover the function prototype of the decompiled function.

For functions with the same name with different argu- ments (i.e., function overloading), compilers store multiple entities in DWARF sections. However, Ghidra may fail to match the correct entity for such a function. Consequently, Ghidra suspends the analysis of this function, which results in its decompiled function lacking arguments, i.e., void.

In Ghidra, constants are treated simi- larly to global variables, which means rules will be applied to infer their types (both their signedness and their sizes).

When Ghidra cannot correctly resolve indirect addresses, it uses the notion of partially re- solved address, as shown in this expression: “𝑣𝑎𝑟1._𝑥_𝑦_ = 𝑣𝑎𝑟2”. This expression means that only 𝑦 bytes starting with offset 𝑥 in 𝑣𝑎𝑟1 should become equal to 𝑣𝑎𝑟2.

{{< /note >}}
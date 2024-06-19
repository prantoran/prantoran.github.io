---
title: "UPX Packer Internals"
date: 2024-06-18T08:06:25+06:00
description: Notes on how UPX packer works
menu:
  sidebar:
    name: UPX Packer Internals
    identifier: upx
    weight: 30
author:
  name: Pinku Deb Nath
  image: /images/author/pinku.png
math: true
---
Basically
- It will take an executable, compress it, and pack the compressed code into another section of the executable.
- At runtime, it will uncompress the previously compressed code and execute it.

In addition, UPX also obfuscates the actual code.

UPX packed executable extracts the packed code, runs it, and terminates its process without serializing anything to disk. The program representation on disk never changes.

**__TEXT** contains program code usually.
**__LINKEDIT** contains information used by the linker.
**__PAGEZERO** is a chunk of memory allocated and protected to throw exceptions on access.


Compressed binary by UPX have very few functions compared to the original binary and all the functions are **local**. None of these are imported from external libraries (Check Imported Symbols).

UPX version has no strings defined in the program, while the non-UPX/original version has plenty.

Orignal binary has a large initla entrypoint, while the UPX'd version has none. 
- Original binary makes system calls fairly immediately (i.e. getenv(.), ioctl(.), and so on). The UPX’d version is self-contained (we’d expect this, as it doesn’t import anything).


UPX'd version has a do/while loop in the center of its entrypoint function.
- Traverses the program __TEXT segment from the top down, until we get to a non-null value.
- Then, we have two additional function calls that are visited using jump instrcution, and we exit.
- One of those 2 functions is the primary function dispatcher for the UPX code.
  - This function is more complex than the EntryPoint() and it delegates to many more functions as well.
  - Returns a function pointer which the programs calls. 
    - This is the original program entry point after the code has been decompressed and is ready for execution.

Ways to identify a binary that is packed (i.e. with UPX):
- No (or very few) strings. 
- No (or very few) library dependencies. 
- The defined functions in the executable is very small.



# Reference
[1] https://dzone.com/articles/packers-how-they-work-featuring-upx
[2] Hopper Disassembler: https://www.hopperapp.com
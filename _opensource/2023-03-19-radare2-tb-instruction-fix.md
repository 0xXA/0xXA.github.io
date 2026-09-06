---
title: "Fix tb instruction for ARM assembler ##asm"
project: "radareorg/radare2"
date: 2023-03-19
tags: [open-source-development, C]
---

Current code unconditionally drops last 2 bits without checking if those 2 bits are set or cleared,
if ignored these 2 bits are eventually lost and not encoded in machine instruction
and it's dangerous to assume destination supplied is valid to correct this I implemented a check.
Last 2 bits are discarded when final machine instruction is generated
and later in the decode phase this 14 bit immediate value (destination) is shifted left 2 bit positions,
and later sign extended to 64 bits that means we can actually encode a number with 16 bits but current code encodes only upto 14 bits. Thus, wasting 2 bits.
Also, Current code unconditionally parses last 5 bits from immediate 1, which is nothing but bit number to be tested in the register. Therefore, it must be within range 0-31 if 32 bit register is used, range must 0-63 if 64 bit register is used.
Also, in the case of 64 bit register only last 5 bits are encoded because it's later concatenated with MSB hence rendering a 6 bit number that can be used to denote bit positions between 0-63. To tackle this a check for this is implemented.
At last testcases are added to demonstrate these checks.

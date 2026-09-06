---
title: "droidimg: fix a warning"
project: "nforest/droidimg"
date: 2018-12-18
tags: [open-source-development, C]
---

fix_kaslr_4_4.c:172:31: warning: format specifies type
      'unsigned int' but the argument has type
      'uint64_t' (aka 'unsigned long') [-Wformat]
  ...printf("p->info = 0x%x\n", p->info);

Change-Id: Ieb26227a3d1f159972abdfce359e4b9f5e9416f3

---
title: "fix(afl-fuzz-init): ensure proper permissions for setting CPU governor"
project: "AFLplusplus/AFLplusplus"
date: 2025-03-10
tags: [open-source-development, C]
---

The previous command used tee without sudo, which could fail due to insufficient permissions.

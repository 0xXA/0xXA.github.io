---
title: "releng: handle string checks carefully"
project: "frida/frida"
date: 2020-07-16
tags: [open-source-development, shell]
---

fixes build errors related to these check when the variable containing a string is empty
./releng/setup-env.sh: line 122: [: -ne: unary operator expected

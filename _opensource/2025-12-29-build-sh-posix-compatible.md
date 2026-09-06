---
title: "fix(build): make build_protos.sh POSIX sh compatible"
project: "google/osv-scalibr"
date: 2025-12-29
tags: [open-source-development, shell]
---

build_protos.sh used bash arithmetic syntax (( ... )) while being invoked via /bin/sh, causing runtime failures on systems where /bin/sh is dash.

Use POSIX-compliant [ ... ] numeric tests instead.

This fixes runtime errors like:

$ ./build_protos.sh
./build_protos.sh: 24: REGEN_RESULT: not found
./build_protos.sh: 24: REGEN_CONFIG: not found
./build_protos.sh: 33: REGEN_RESULT: not found
./build_protos.sh: 40: REGEN_CONFIG: not found

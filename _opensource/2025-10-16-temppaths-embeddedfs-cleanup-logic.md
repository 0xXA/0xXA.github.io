---
title: "Add TempPaths field to EmbeddedFS and improve cleanup logic - #1465"
project: "google/osv-scalibr"
date: 2025-10-16
tags: [open-source-development, go]
---

* Introduced a new TempPaths field in EmbeddedFS to track temporary files and directories created during extraction.
* Enhanced deferred cleanup logic to iterate over all EmbeddedFS instances and remove associated TempPaths safely.
* Added descriptive comments explaining the purpose and lifecycle of EmbeddedFS fields and the deferred cleanup behavior.

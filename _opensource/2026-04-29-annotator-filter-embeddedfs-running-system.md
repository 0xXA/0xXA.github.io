---
title: "Annotator: filter embedded FS packages for plugins with the RunningSystem capability"
project: "google/osv-scalibr"
date: 2026-04-29
tags: [open-source-development, go]
---

Annotators that require a running system may execute commands or binaries on the target filesystem. However, packages originating from embedded filesystems are extracted artifacts and do not represent a live system. This can lead to incorrect behavior, especially when binaries belong to a different architecture and emulation is not supported.

To address this, filter out packages originating from embedded filesystems before passing the inventory to annotators with RunningSystem requirements. Annotators that do not require a running system continue to receive the full inventory.

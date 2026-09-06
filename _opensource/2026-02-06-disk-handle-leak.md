---
title: "fix: prevent disk handle leak by calling disk.Close() on failure"
project: "google/osv-scalibr"
date: 2026-02-06
tags: [vulnerability research, go]
---

Ensure the disk handle is properly closed when opening the raw disk image or reading the partition table fails.
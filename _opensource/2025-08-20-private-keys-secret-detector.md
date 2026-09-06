---
title: "Add secret detector for private keys (PEM/OpenSSH, DER: PKCS#1, PKCS#8, EC)"
project: "google/osv-scalibr"
date: 2025-08-20
tags: [open-source-development, go]
---

Introduces a secret detector for private keys, supporting both PEM/OpenSSH blocks and DER-encoded formats.

Features:
```
    Detects common PEM headers/footers and validates structure via x509.
    Detects DER encodings (PKCS#1, PKCS#8, EC) using x509 parsers.
    Provides lightweight validation for DSA, Ed25519, and OpenSSH PEM keys.
    Includes unit tests for validatePEMBlock and detectDER.
```
Validation method:
```
    PEM: match headers/footers with regex, decode with pem.Decode, and
    confirm structure using RSA, EC, or PKCS#8 parsers.
    DER: try parsing input as PKCS#8, PKCS#1, or EC private key encodings.
    Unit tests ensure detection of valid keys and rejection of invalid inputs.
```

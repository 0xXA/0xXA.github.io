---
title: "simplevalidate: add multi-endpoint support"
project: "google/osv-scalibr"
date: 2026-02-18
tags: [open-source-development, go]
---

This change enhances the simplevalidate package by introducing support for validating secrets against multiple HTTP endpoints.

- Added support for multiple endpoints via:
  - Endpoints []string
  - EndpointsFunc func(S) ([]string, error)

- Enforced exclusivity validation: Exactly one of the following must be provided:
    - Endpoint
    - EndpointFunc
    - Endpoints
    - EndpointsFunc

  Supplying none or multiple options results in ValidationFailed.

- Updated validation loop to:
  - Iterate across multiple endpoints
  - Stop early on ValidationValid
  - Track invalid vs failure outcomes across endpoints
  - Return ValidationInvalid if any endpoint definitively invalidates the secret
  - Return ValidationFailed only when all endpoints fail

- Improved error reporting: Aggregates per-endpoint failure reasons and returns a combined error:
    <endpoint1>: <reason>, <endpoint2>: <reason>

  This makes debugging network, status, and body parsing failures much clearer.

Expanded testcases to incluude:

- Multiple endpoint success and early exit behavior
- All endpoints invalid
- EndpointFunc returning multiple endpoints
- EndpointsFunc returning error
- Exclusivity validation (all invalid combinations)
- No endpoints provided
- Mixed configuration scenarios

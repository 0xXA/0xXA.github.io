---
title: "slacktoken: return error on ValidationFailed and update tests"
project: "google/osv-scalibr"
date: 2026-02-19
tags: [open-source-development, go]
---

Ensure statusFromResponseBody returns a non-nil error when Slack API validation fails (non-invalid_auth error). Updated tests to assert error behavior using go-cmp with cmpopts.EquateErrors and cmpopts.AnyError.

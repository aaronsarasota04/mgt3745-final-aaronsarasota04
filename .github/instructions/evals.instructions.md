---
applyTo: "evals/**"
---
Tests use node:test and node:assert/strict.
Every test name begins with the EARS row ID it checks, for example "F-01b: empty input shows the message".
Test boundary values from the row directly; do not mock them.
A test that needs a running service reads its URL from an environment variable and skips with a reason when it is unset.

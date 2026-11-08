# AGENTS.md

## Setup
npm install
npx wrangler dev   (only if worker/ is used)

## Test
npm test runs node:test over evals/. All tests must pass before a pull request.

## Boundaries
Never write to context/ without a pull request. Never read .dev.vars. Never commit builds/ contents you did not export.

## Review
Open a draft pull request from a branch, link the DDR, and request review.

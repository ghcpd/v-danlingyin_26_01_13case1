API Documentation Audit — README.md

Scope
- Files reviewed: `README.md` (project root) and implementation `api.ts`.
- Target API: `createOrder`.
- Goal: verify that the documented API contract matches the TypeScript implementation.

Summary of findings
- Total issues: 4 (3 High, 1 Medium).
  - Missing/incorrect parameter requirements and ranges for `quantity` (High).
  - `couponCode` documented as required but is optional in code (Medium).
  - Return value shape in docs does not match `OrderResult` returned by the function (High).
  - Missing documentation of thrown error for invalid `quantity` (High).

How citations were extracted
- Parameter table rows and the Returns code block were taken from `README.md` (lines quoted in `citations.md`).
- Type declarations, runtime validation, thrown error, and returned object were taken from `api.ts` (exact line numbers quoted in `citations.md`).

How to manually verify
1. Open `README.md` and confirm the parameters table and Returns block.
2. Open `api.ts` and inspect:
   - `CreateOrderInput` (fields and optional markers).
   - Validation inside `createOrder` (the `if` that throws for invalid quantities).
   - `OrderResult` interface and the returned object.
3. Compare each documented contract item against the corresponding code lines — the exact lines are listed in `citations.md`.

Notes
- No source code was modified during this audit. A backup of the original README is provided as `README_backup.md`.
- The repository appears to expose a local TypeScript function rather than an HTTP endpoint; no HTTP status codes were documented or expected.
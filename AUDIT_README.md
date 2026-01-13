# API Documentation Audit — Order API

## Scope
- Files reviewed: `README.md`, `api.ts` in repository root.
- Focused on verifying that the documented API contract matches the actual TypeScript implementation.

## Summary of issues found
- 4 issues found: 3 High severity, 1 Medium severity.
  - High: Incorrect `quantity` documentation (required/range + missing error), incorrect Return value structure, missing error documentation.
  - Medium: `couponCode` documented as required but implemented as optional.

## How citations were extracted
- Exact lines from `README.md` were used (table rows and code blocks) and exact lines from `api.ts` (interface fields, runtime checks, return statement) were quoted with line references.
- To verify, open the files and compare the listed lines:
  - `README.md` (parameters table and returns block)
  - `api.ts` (lines defining `CreateOrderInput`, `OrderResult`, the range-check and return statement)

## How to manually verify corrections
1. Open `api.ts` and inspect:
   - `CreateOrderInput` fields (lines 3-6)
   - The validation `if (input.quantity < 1 || input.quantity > 5)` (lines 21-23)
   - Return object (lines 25-28)
2. Open the updated `README.md` and confirm:
   - Parameters match the interface (required/optional, types)
   - Returns match `OrderResult`
   - Throws section documents the `Invalid quantity` error and range

## Next steps / Recommendations
- Accept the updated `README.md` (provided) and keep an audit checklist for future changes: when code changes an API, update docs and add unit tests that assert documentation correctness (if feasible).

---

For questions or follow-ups, review `report.json` and `citations.md` for exact details.

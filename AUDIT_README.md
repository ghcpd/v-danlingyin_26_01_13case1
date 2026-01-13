API Documentation Audit — createOrder

Scope
- Repository path: `api.ts` (single-file implementation) and `README.md` (public docs).
- Objective: Verify that the documented API contract matches the implementation exactly.
- Files produced by this audit:
  - `README_backup.md` — original README (untouched copy)
  - `README.md` — corrected documentation (now matches code)
  - `report.json` — machine-readable audit report (issues, severities, citations)
  - `citations.md` — human-readable, side-by-side evidence for each issue

Summary of findings (high level)
- Major mismatches (High): parameter requiredness & range (`quantity`), return value shape, missing error documentation.
- Medium: `couponCode` incorrectly documented as required.
- Low: documentation does not state that this is a synchronous library function (no HTTP status codes).

How citations were extracted
- Source-of-truth for implementation: `api.ts` (line-numbered citations included in `citations.md`).
- Source-of-truth for original documentation: `README_backup.md` (original README content preserved verbatim).
- Every issue in `report.json` includes: the exact quoted documentation text (or section), and the exact code line-number citation that demonstrates the implemented behavior.

How to manually verify (quick steps)
1. Open `api.ts` and inspect:
   - `CreateOrderInput` fields (required/optional and types)
   - `OrderResult` interface
   - `createOrder` runtime checks and return statement
2. Open `README_backup.md` to see the original documentation claims.
3. Confirm that `README.md` now matches the implementation and that `report.json` issues are represented in `citations.md`.

Recommended follow-ups
- If this function is exposed over HTTP elsewhere, add a separate HTTP contract doc (endpoint, method, status codes, response body).
- Add unit tests that assert the thrown error for out-of-range `quantity` and verify returned `totalPrice` semantics.

Contact
- Audit performed automatically by repository auditor. For questions or disagreements, run the verification steps above and adjust documentation to reflect intended (not just implemented) behavior.

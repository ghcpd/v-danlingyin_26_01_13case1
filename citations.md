# Citations — createOrder

Issue 1 — quantity: documented vs actual

Documented API contract (README.md):
- Line 12: `| quantity | number | no | Number of items (0–10) |`

Actual code implementation (api.ts):
- Line 5: `quantity: number`
- Lines 21-22:
  `if (input.quantity < 1 || input.quantity > 5) {
    throw new Error("Invalid quantity");
  }`

Explanation
- The documentation marks `quantity` as optional (`no`) and states the range is 0–10. The code requires `quantity` (no `?`) and enforces a range of 1–5, throwing an error for out-of-range values. This is a functional mismatch that can cause runtime errors.

---

Issue 2 — couponCode: documented vs actual

Documented API contract (README.md):
- Line 13: `| couponCode | string | yes | Discount coupon code |`

Actual code implementation (api.ts):
- Line 6: `couponCode?: string`

Explanation
- The documentation says `couponCode` is required, but the TypeScript input type makes it optional. This mismatch is misleading for integrators.

---

Issue 3 — Return value shape

Documented API contract (README.md):
- Lines 17-21 (returns block):
  ```ts
  {
    success: boolean;
    orderId?: string;
  }
  ```

Actual code implementation (api.ts):
- Lines 9-11 (OrderResult interface):
  `export interface OrderResult {
    orderId: string;
    totalPrice: number;
  }`
- Lines 25-27 (returned object):
  `return {
    orderId: "ORD-001",
    totalPrice: input.quantity * 100
  };`

Explanation
- The documentation describes a different return shape (a `success` boolean and optional `orderId`). The function actually returns `orderId` (required) and `totalPrice` (number). This is a breaking mismatch for consumers parsing responses.

---

Issue 4 — Missing error documentation

Documented API contract (README.md):
- The README contains no "Errors" section and does not document validation or thrown exceptions for invalid inputs.

Actual code implementation (api.ts):
- Lines 21-22:
  `if (input.quantity < 1 || input.quantity > 5) {
    throw new Error("Invalid quantity");
  }`

Explanation
- The implementation throws an Error for invalid `quantity` values. The absence of this information in the documentation means callers cannot reliably handle this exception.

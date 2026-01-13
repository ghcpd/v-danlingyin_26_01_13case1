# Citations

## Issue 1 — Parameters: `quantity` (High)

- Documented API contract (README.md):

  "| quantity | number | no | Number of items (0–10) |" (README.md lines 11-12)

- Actual implementation (api.ts):

  - `CreateOrderInput.quantity` is required: `quantity: number;` (api.ts line 5)
  - Runtime check and thrown error: `if (input.quantity < 1 || input.quantity > 5) { throw new Error("Invalid quantity"); }` (api.ts lines 21-23)

- Explanation: The docs claim `quantity` is optional and allow 0–10, but the code requires it and limits the value to 1–5, throwing an error outside this range.

---

## Issue 2 — Parameters: `couponCode` (Medium)

- Documented API contract (README.md):

  "| couponCode | string | yes | Discount coupon code |" (README.md line 13)

- Actual implementation (api.ts):

  - `couponCode` is optional: `couponCode?: string;` (api.ts line 6)

- Explanation: The documentation marks `couponCode` as required while the type shows it's optional.

---

## Issue 3 — Returns (High)

- Documented API contract (README.md):

  ```
  {
    success: boolean;
    orderId?: string;
  }
  ```
  (README.md lines 17-21)

- Actual implementation (api.ts):

  - `OrderResult` interface: `orderId: string; totalPrice: number;` (api.ts lines 9-11)
  - Returned value: `return { orderId: "ORD-001", totalPrice: input.quantity * 100 };` (api.ts lines 25-28)

- Explanation: The docs claim a `success` boolean and an optional `orderId`. The implementation returns a mandatory `orderId` and a `totalPrice` number — there is no `success` property.

---

## Issue 4 — Errors/Exceptions (High)

- Documented API contract (README.md): No 'Throws' or 'Errors' section present; only 'Creates a new order.' is stated (README.md lines 3-5).

- Actual implementation (api.ts): The function throws `Error("Invalid quantity")` when quantity is out of range (api.ts lines 21-23).

- Explanation: The docs omit that the function throws on invalid `quantity`, which is essential for correct error handling by callers.

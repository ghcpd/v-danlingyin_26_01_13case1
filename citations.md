# Citations — createOrder (side-by-side)

Issue 1 — quantity: requiredness & range

- Documented API contract (README_backup.md, Parameters table):
  | quantity | number | no | Number of items (0–10) |

- Actual implementation (api.ts):
  - interface declaration: line 5: `quantity: number;`
  - runtime validation + throw: lines 21-22:
    if (input.quantity < 1 || input.quantity > 5) {
      throw new Error("Invalid quantity");
    }

- Explanation: The documentation states `quantity` is optional and allowed values 0–10. The code requires `quantity` (non-optional) and enforces a 1–5 inclusive range, throwing an Error for out-of-range values. This is a breaking mismatch (High).

---

Issue 2 — couponCode required flag

- Documented API contract (README_backup.md, Parameters table):
  | couponCode | string | yes | Discount coupon code |

- Actual implementation (api.ts):
  - interface declaration: line 6: `couponCode?: string;`

- Explanation: Documentation marks `couponCode` as required; implementation makes it optional. Consumers following the docs may send unnecessary data; callers following the code will not be required to provide a coupon. (Medium)

---

Issue 3 — return value shape

- Documented API contract (README_backup.md, Returns):
  ```ts
  {
    success: boolean;
    orderId?: string;
  }
  ```

- Actual implementation (api.ts):
  - return type/interface: lines 9-11:
    export interface OrderResult {
      orderId: string;
      totalPrice: number;
    }
  - actual returned object: lines 25-27:
    return {
      orderId: "ORD-001",
      totalPrice: input.quantity * 100
    };

- Explanation: Documentation lists `success` (boolean) which is not returned, and omits `totalPrice` which the implementation always returns. This will break integrations that expect `success` or that do not handle `totalPrice`. (High)

---

Issue 4 — missing error documentation

- Documented API contract: (no Errors / Exceptions section; Parameters table incorrectly documents the range)

- Actual implementation (api.ts): lines 21-22 (validation + throw):
    if (input.quantity < 1 || input.quantity > 5) {
      throw new Error("Invalid quantity");
    }

- Explanation: The code enforces constraints and throws a specific Error message; the documentation does not mention this. Callers will not know to catch or avoid the thrown error. (High)

---

Issue 5 — API vs. HTTP ambiguity

- Documented API contract (README_backup.md): presents a parameter/returns table but does not state transport.

- Actual implementation (api.ts): synchronous function (lines 20-29).

- Explanation / recommendation: Clarify that this is a library (in-process) API. If there is an HTTP wrapper, document its endpoint, method, status codes, and response shape separately. (Low)

---

How the code citations were extracted

- Code citations reference exact line numbers in `api.ts` from the repository root. Line numbers used are literal within the current file version examined by the auditor.

How the documentation citations were extracted

- The original documentation is preserved in `README_backup.md` and quoted above verbatim to ensure auditability.

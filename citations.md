# API Documentation Audit Citations

## Issue 1: quantity parameter requirement

### Documented Contract
**File:** README.md, Section: Parameters  
**Quoted Text:**  
| quantity | number | no | Number of items (0–10) |

### Actual Implementation
**File:** api.ts, Lines 3-5  
**Code:**  
```ts
export interface CreateOrderInput {
  productId: string;
  quantity: number;
  couponCode?: string;
}
```

### Explanation
The documentation states quantity is optional ("no"), but the TypeScript interface defines it as required (no `?`).

## Issue 2: quantity parameter constraint

### Documented Contract
**File:** README.md, Section: Parameters  
**Quoted Text:** Number of items (0–10)

### Actual Implementation
**File:** api.ts, Lines 15-17  
**Code:**  
```ts
if (input.quantity < 1 || input.quantity > 5) {
  throw new Error("Invalid quantity");
}
```

### Explanation
The documentation allows 0–10, but the code enforces 1–5 and throws an error for values outside this range.

## Issue 3: couponCode parameter requirement

### Documented Contract
**File:** README.md, Section: Parameters  
**Quoted Text:**  
| couponCode | string | yes | Discount coupon code |

### Actual Implementation
**File:** api.ts, Line 5  
**Code:**  
```ts
couponCode?: string;
```

### Explanation
The documentation states couponCode is required ("yes"), but the interface defines it as optional with `?`.

## Issue 4: Return value structure

### Documented Contract
**File:** README.md, Section: Returns  
**Quoted Text:**  
```ts
{
  success: boolean;
  orderId?: string;
}
```

### Actual Implementation
**File:** api.ts, Lines 8-10 and 19-22  
**Code:**  
```ts
export interface OrderResult {
  orderId: string;
  totalPrice: number;
}

/**
 * Creates a new order.
 * Throws error if quantitys
 * - quantity < 1
 * - quantity > 5
 */
export function createOrder(input: CreateOrderInput): OrderResult {
  if (input.quantity < 1 || input.quantity > 5) {
    throw new Error("Invalid quantity");
  }

  return {
    orderId: "ORD-001",
    totalPrice: input.quantity * 100
  };
}
```

### Explanation
The documented return type is { success: boolean; orderId?: string; }, but the actual return is { orderId: string; totalPrice: number; }.

## Issue 5: Missing error documentation

### Documented Contract
**File:** README.md  
**Quoted Text:** No error cases documented.

### Actual Implementation
**File:** api.ts, Lines 15-17  
**Code:**  
```ts
if (input.quantity < 1 || input.quantity > 5) {
  throw new Error("Invalid quantity");
}
```

### Explanation
The documentation does not mention any errors, but the code throws an Error for invalid quantity values.
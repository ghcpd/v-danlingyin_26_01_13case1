# Order API

## createOrder

Synchronous helper that creates a new order from a provided input object.

**Implementation:** TypeScript function — not an HTTP endpoint.

### Signature

```ts
function createOrder(input: CreateOrderInput): OrderResult
```

### Types

```ts
interface CreateOrderInput {
  productId: string;       // required
  quantity: number;        // required, 1..5 inclusive (validated at runtime)
  couponCode?: string;     // optional
}

interface OrderResult {
  orderId: string;
  totalPrice: number;
}
```

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| productId | string | yes | Product identifier. Must be provided. |
| quantity | number | yes | Number of items. Required. Valid range: **1–5** (inclusive). Values outside this range cause the function to throw an error. Non-integer numbers are accepted by the implementation (they will affect totalPrice). |
| couponCode | string | no | Optional discount coupon code. |

### Returns

Returns an object of type `OrderResult`.

```ts
{
  orderId: string;      // always present
  totalPrice: number;   // always present
}
```

### Errors

- Throws Error("Invalid quantity") when `quantity < 1` or `quantity > 5`.

### Notes

- This is a synchronous library function (TypeScript). There are no HTTP status codes associated with the function itself.
- The documentation precisely reflects the current implementation; callers should validate inputs to avoid runtime exceptions.

### Example

```ts
// input
const result = createOrder({ productId: "SKU-123", quantity: 2 });
// result -> { orderId: "ORD-001", totalPrice: 200 }
```
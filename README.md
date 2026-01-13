# Order API

## createOrder

Creates a new order.

Signature

```ts
createOrder(input: CreateOrderInput): OrderResult
```

### Parameters

| Name | Type | Required | Description |
|-----|------|----------|-------------|
| productId | string | yes | Product identifier |
| quantity | number | yes | Number of items. Valid range: 1–5 (inclusive). |
| couponCode | string | no | Discount coupon code (optional). |

### Returns

```ts
export interface OrderResult {
  orderId: string;
  totalPrice: number;
}
```

### Errors

- Throws Error("Invalid quantity") when `quantity < 1` or `quantity > 5`.

Notes

- The module exports the `CreateOrderInput` and `OrderResult` TypeScript interfaces; callers should pass an input matching `CreateOrderInput`.
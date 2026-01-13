# Order API

## createOrder

Creates a new order.

### Parameters

| Name | Type | Required | Description |
|-----|------|----------|-------------|
| productId | string | yes | Product identifier |
| quantity | number | yes | Number of items (1–5). Must be between 1 and 5 (inclusive). An error is thrown if value is < 1 or > 5. |
| couponCode | string | no | Discount coupon code (optional) |

### Returns

```ts
{
  orderId: string;
  totalPrice: number; // price in cents (quantity * 100)
}
```

### Throws

- `Error("Invalid quantity")` — thrown when `quantity < 1` or `quantity > 5`.

### Notes

- `totalPrice` is computed as `quantity * 100`.
- `couponCode` is optional in the implementation.
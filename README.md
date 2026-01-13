# Order API

## createOrder

Creates a new order.

### Parameters

| Name | Type | Required | Description |
|-----|------|----------|-------------|
| productId | string | yes | Product identifier |
| quantity | number | yes | Number of items (1–5) |
| couponCode | string | no | Discount coupon code |

### Returns

```ts
{
  orderId: string;
  totalPrice: number;
}
```

### Errors

Throws an error if quantity is less than 1 or greater than 5.

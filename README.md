# Order API

## createOrder

Creates a new order. Throws an error if the quantity is outside the valid range.

### Parameters

| Name | Type | Required | Description |
|-----|------|----------|-------------|
| productId | string | yes | Product identifier |
| quantity | number | yes | Number of items (must be between 1 and 5 inclusive) |
| couponCode | string | no | Optional discount coupon code |

### Returns

```ts
{
  orderId: string;
  totalPrice: number;
}
```

### Errors

- Throws `Error` with message "Invalid quantity" if quantity is less than 1 or greater than 5
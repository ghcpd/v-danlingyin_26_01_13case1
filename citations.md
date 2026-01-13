# API Documentation Audit: Citation Evidence

## Overview
This document provides side-by-side comparison of documented API contracts versus actual code implementation for the `createOrder` function.

---

## Issue #1: Quantity Parameter Constraint

### Documented Behavior
**File**: README.md (Parameters table)
```
| quantity | number | no | Number of items (0–10) |
```
**Documented Contract**:
- Optional parameter (marked "no")
- Valid range: 0–10

### Actual Behavior
**File**: api.ts (Lines 17-20)
```typescript
if (input.quantity < 1 || input.quantity > 5) {
  throw new Error("Invalid quantity");
}
```
**Actual Contract**:
- Required parameter (no `?` in interface definition at line 4)
- Valid range: 1–5 (enforced at lines 18-19)
- Throws error if outside range

### Explanation
The documentation states quantity is optional with a range of 0–10, but the code enforces that quantity must be between 1 and 5 (inclusive). The parameter is also required in the `CreateOrderInput` interface (line 4 shows `quantity: number` without `?`), not optional.

---

## Issue #2: couponCode Parameter Required Status

### Documented Behavior
**File**: README.md (Parameters table)
```
| couponCode | string | yes | Discount coupon code |
```
**Documented Contract**:
- Required parameter (marked "yes")

### Actual Behavior
**File**: api.ts (Line 5)
```typescript
export interface CreateOrderInput {
  productId: string;
  quantity: number;
  couponCode?: string;  // <- Optional (indicated by ?)
}
```
**Actual Contract**:
- Optional parameter (indicated by `?` suffix)

### Explanation
Documentation marks couponCode as required, but the interface definition shows it as optional with the `?` operator. This means the function can be called without providing a couponCode.

---

## Issue #3: Return Type Structure

### Documented Behavior
**File**: README.md (Returns section)
```typescript
{
  success: boolean;
  orderId?: string;
}
```
**Documented Contract**:
- Returns object with `success` (required boolean) and `orderId` (optional string)

### Actual Behavior
**File**: api.ts (Lines 22-25)
```typescript
export interface OrderResult {
  orderId: string;
  totalPrice: number;
}
...
return {
  orderId: "ORD-001",
  totalPrice: input.quantity * 100
};
```
**Actual Contract**:
- Returns object with `orderId` (required string) and `totalPrice` (required number)
- No `success` field
- No optional fields

### Explanation
The documentation describes a completely different return object. The actual function returns `orderId` (always present, not optional) and `totalPrice` (always calculated), not a `success` boolean.

---

## Issue #4: Missing Error Documentation

### Documented Behavior
**File**: README.md
- No "Errors" section present
- No documentation of exception handling

### Actual Behavior
**File**: api.ts (Lines 14-20)
```typescript
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
```
**Actual Contract**:
- Throws `Error` with message "Invalid quantity" when quantity is invalid
- Error condition: quantity < 1 OR quantity > 5

### Explanation
The JSDoc comment in the code indicates that errors are thrown, but the README.md provides no documentation of this error behavior. API consumers need to know that the function throws an exception under specific conditions.

---

## Summary

| Issue | Type | Severity | Impact |
|-------|------|----------|--------|
| quantity range (0–10 vs 1–5) | Constraint mismatch | High | Code rejects valid documented values; accepts values at documented limits |
| quantity optional vs required | Parameter status | High | Code requires parameter; docs say optional |
| couponCode required vs optional | Parameter status | High | Code accepts missing parameter; docs require it |
| Return structure mismatch | Type definition | High | Documented fields don't exist; missing required totalPrice field |
| Missing error documentation | Behavioral omission | High | Documented API doesn't mention exception behavior |

All 4 issues are **High severity** because they directly cause:
- Runtime errors when following documentation
- Incorrect integration by API consumers
- Type mismatches in code relying on documented types

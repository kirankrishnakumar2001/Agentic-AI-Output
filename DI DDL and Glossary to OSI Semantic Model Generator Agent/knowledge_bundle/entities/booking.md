---
type: Business Entity
title: Booking
description: Represents a booked order line transaction.
tags:
  - Booking
  - Fact
---

# Booking

## Business definition
Stores measurable business transactions representing customer bookings. Each record corresponds to a booked order line.

## Technical mapping
- Table: `fact_bookings`
- Grain: one row per booked order line

## Attributes
- `booking_id`
- `order_number`
- `order_line_number`
- `date_key`
- `customer_key`
- `product_key`
- `partner_key`
- `geography_key`
- `sales_rep_key`
- `contract_key`
- `booking_type`
- `is_renewal`

## Keys
- Primary key: `booking_id`
- Foreign keys: `date_key`, `customer_key`, `product_key`, `partner_key`, `geography_key`, `sales_rep_key`, `contract_key`

## Measures
- [Quantity](../measures/quantity.md)
- [Unit List Price USD](../measures/unit-list-price-usd.md)
- [Discount Percentage](../measures/discount-percentage.md)
- [Booking Amount USD](../measures/booking-amount-usd.md)
- [ACV USD](../measures/acv-usd.md)
- [TCV USD](../measures/tcv-usd.md)

## Relationships
- [Booking occurs on Date](../relationships/booking-occurs-on-date.md)
- [Booking is for Customer](../relationships/booking-is-for-customer.md)
- [Booking includes Product](../relationships/booking-includes-product.md)
- [Booking involves Partner](../relationships/booking-involves-partner.md)
- [Booking is assigned to Geography](../relationships/booking-is-assigned-to-geography.md)
- [Booking is owned by Sales Representative](../relationships/booking-is-owned-by-sales-representative.md)
- [Booking is governed by Contract](../relationships/booking-is-governed-by-contract.md)

## Business notes
Degenerate dimensions are order_number and order_line_number.

## Confidence score
1.00

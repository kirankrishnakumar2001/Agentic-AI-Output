---
type: Business Domain
title: Bookings
description: Core transactional domain for booked order lines and commercial measures.
tags:
  - Bookings
  - Revenue
---

# Bookings

## Business definition
Captures measurable booking transactions at the grain of one booked order line.

## Business purpose
Supports revenue, pricing, renewal, and commercial performance analysis.

## Related entities
- [Booking](../entities/booking.md)

## Related measures
- [Quantity](../measures/quantity.md)
- [Unit List Price USD](../measures/unit-list-price-usd.md)
- [Discount Percentage](../measures/discount-percentage.md)
- [Booking Amount USD](../measures/booking-amount-usd.md)
- [ACV USD](../measures/acv-usd.md)
- [TCV USD](../measures/tcv-usd.md)

## Notes
Explicit domain based on fact table semantics.

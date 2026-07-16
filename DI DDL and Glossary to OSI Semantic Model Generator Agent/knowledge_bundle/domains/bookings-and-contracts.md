---
type: Business Domain
title: Bookings & Contracts
description: Core booking transactions and contract terms supporting revenue and renewal analysis.
tags:
  - Booking
  - Contract
  - Revenue
---

# Bookings & Contracts

## Business definition
Represents measurable booking transactions and their associated contract characteristics.

## Technical mapping
- Tables: `fact_bookings`, `dim_contract`

## Related entities
- [Booking](../entities/booking.md)
- [Contract](../entities/contract.md)

## Related measures
- [Quantity](../measures/quantity.md)
- [Unit List Price USD](../measures/unit-list-price-usd.md)
- [Discount Percent](../measures/discount-percent.md)
- [Booking Amount USD](../measures/booking-amount-usd.md)
- [ACV USD](../measures/acv-usd.md)
- [TCV USD](../measures/tcv-usd.md)

## Business notes
This is the strongest analytical domain because it contains the central fact table and all principal monetary measures.

## Confidence score
1.00

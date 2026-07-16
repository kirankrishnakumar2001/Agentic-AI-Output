---
type: Business Domain
title: Bookings
description: Central business domain for booked order line transactions and commercial measures.
tags:
  - Bookings
  - Revenue
---

# Bookings Domain

## Business Purpose
Represents measurable booking transactions used to analyze revenue, quantity, discounts, renewals, and contract value.

## Technical Mapping
- Table: `fact_bookings`

## Related Entities
- [Booking](../entities/booking.md)
- [Date](../entities/date.md)
- [Customer](../entities/customer.md)
- [Product](../entities/product.md)
- [Partner](../entities/partner.md)
- [Geography](../entities/geography.md)
- [Sales Representative](../entities/sales_representative.md)
- [Contract](../entities/contract.md)

## Related Measures
- [Quantity](../measures/quantity.md)
- [Unit List Price USD](../measures/unit_list_price_usd.md)
- [Discount Percent](../measures/discount_pct.md)
- [Booking Amount USD](../measures/booking_amount_usd.md)
- [ACV USD](../measures/acv_usd.md)
- [TCV USD](../measures/tcv_usd.md)

## Notes
Explicitly supported by the DDL and glossary. Confidence score: 1.00

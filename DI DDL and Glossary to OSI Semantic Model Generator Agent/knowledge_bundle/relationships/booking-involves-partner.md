---
type: Relationship
title: Booking involves Partner
description: Connects bookings to the partner dimension.
tags:
  - Relationship
---

# Booking involves Partner

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Partner](../entities/partner.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Partner

## Technical mapping
`fact_bookings.partner_key -> dim_partner.partner_key`

## Confidence score
1.00

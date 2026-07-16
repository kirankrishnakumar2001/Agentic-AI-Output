---
type: Relationship
title: Booking is owned by Sales Representative
description: Connects bookings to the sales representative dimension.
tags:
  - Relationship
---

# Booking is owned by Sales Representative

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Sales Representative](../entities/sales-representative.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Sales Representative

## Technical mapping
`fact_bookings.sales_rep_key -> dim_sales_rep.sales_rep_key`

## Confidence score
1.00

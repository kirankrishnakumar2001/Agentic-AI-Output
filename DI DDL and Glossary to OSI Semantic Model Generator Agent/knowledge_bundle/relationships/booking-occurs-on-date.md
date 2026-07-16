---
type: Relationship
title: Booking occurs on Date
description: Connects bookings to the date dimension.
tags:
  - Relationship
---

# Booking occurs on Date

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Date](../entities/date.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Date

## Technical mapping
`fact_bookings.date_key -> dim_date.date_key`

## Confidence score
1.00

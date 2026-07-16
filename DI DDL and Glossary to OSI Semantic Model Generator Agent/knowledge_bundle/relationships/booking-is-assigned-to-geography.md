---
type: Relationship
title: Booking is assigned to Geography
description: Connects bookings to the geography dimension.
tags:
  - Relationship
---

# Booking is assigned to Geography

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Geography](../entities/geography.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Geography

## Technical mapping
`fact_bookings.geography_key -> dim_geography.geography_key`

## Confidence score
0.95

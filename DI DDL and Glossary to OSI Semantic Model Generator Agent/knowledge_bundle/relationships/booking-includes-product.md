---
type: Relationship
title: Booking includes Product
description: Connects bookings to the product dimension.
tags:
  - Relationship
---

# Booking includes Product

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Product](../entities/product.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Product

## Technical mapping
`fact_bookings.product_key -> dim_product.product_key`

## Confidence score
1.00

---
type: Relationship
title: Booking is for Customer
description: Connects bookings to the customer dimension.
tags:
  - Relationship
---

# Booking is for Customer

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Customer](../entities/customer.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Customer

## Technical mapping
`fact_bookings.customer_key -> dim_customer.customer_key`

## Confidence score
1.00

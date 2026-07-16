---
type: Relationship
title: Booking is for Customer
description: Connects booking transactions to the customer.
tags:
  - Relationship
  - Customer
  - Booking
---

# Booking is for Customer

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Customer](../entities/customer.md)

## Relationship type
Foreign key

## Cardinality
Many bookings to one customer

## Technical mapping
`fact_bookings.customer_key` -> `dim_customer.customer_key`

## Confidence score
1.00

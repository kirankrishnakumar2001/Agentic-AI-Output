---
type: Relationship
title: Booking is governed by Contract
description: Connects bookings to the contract dimension.
tags:
  - Relationship
---

# Booking is governed by Contract

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Contract](../entities/contract.md)

## Relationship type
Fact-to-Dimension

## Cardinality
Many Bookings to One Contract

## Technical mapping
`fact_bookings.contract_key -> dim_contract.contract_key`

## Confidence score
1.00

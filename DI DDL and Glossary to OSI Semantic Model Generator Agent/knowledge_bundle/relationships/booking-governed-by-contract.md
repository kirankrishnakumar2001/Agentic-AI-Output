---
type: Relationship
title: Booking governed by Contract
description: Connects booking transactions to contract terms.
tags:
  - Relationship
  - Contract
  - Booking
---

# Booking governed by Contract

## Source entity
- [Booking](../entities/booking.md)

## Target entity
- [Contract](../entities/contract.md)

## Relationship type
Foreign key

## Cardinality
Many bookings to one contract

## Technical mapping
`fact_bookings.contract_key` -> `dim_contract.contract_key`

## Confidence score
1.00

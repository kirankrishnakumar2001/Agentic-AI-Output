---
type: Business Entity
title: Contract
description: Represents contract characteristics associated with bookings.
tags:
  - Contract
  - Dimension
---

# Contract

## Business definition
Stores information describing customer contracts associated with bookings.

## Technical mapping
- Table: `dim_contract`

## Attributes
- `contract_key`
- `contract_type`
- `term_months`
- `auto_renew_flag`
- `coverage_level`

## Keys
- Primary key: `contract_key`

## Measures
None

## Relationships
- Referenced by [Booking is governed by Contract](../relationships/booking-is-governed-by-contract.md)

## Business notes
Supports renewal and contract coverage analysis.

## Confidence score
1.00

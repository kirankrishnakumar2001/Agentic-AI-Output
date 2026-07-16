---
type: Business Entity
title: Partner
description: Represents channel partners involved in sales.
tags:
  - Partner
  - Dimension
---

# Partner

## Business definition
Stores information about channel partners involved in selling products.

## Technical mapping
- Table: `dim_partner`

## Attributes
- `partner_key`
- `partner_id`
- `partner_name`
- `partner_type`
- `partner_tier`
- `route_to_market`

## Keys
- Primary key: `partner_key`
- Business key: `partner_id`

## Measures
None

## Relationships
- Referenced by [Booking involves Partner](../relationships/booking-involves-partner.md)

## Business notes
Supports channel analysis and tiering.

## Confidence score
1.00

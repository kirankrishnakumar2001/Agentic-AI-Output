---
type: Business Entity
title: Sales Representative
description: Represents sales representatives responsible for bookings.
tags:
  - Sales
  - Dimension
---

# Sales Representative

## Business definition
Contains information about sales representatives responsible for customer bookings.

## Technical mapping
- Table: `dim_sales_rep`

## Attributes
- `sales_rep_key`
- `rep_id`
- `rep_name`
- `sales_role`
- `sales_team`
- `segment_covered`

## Keys
- Primary key: `sales_rep_key`
- Business key: `rep_id`

## Measures
None

## Relationships
- Referenced by [Booking is owned by Sales Representative](../relationships/booking-is-owned-by-sales-representative.md)

## Business notes
Supports team and role analysis.

## Confidence score
1.00

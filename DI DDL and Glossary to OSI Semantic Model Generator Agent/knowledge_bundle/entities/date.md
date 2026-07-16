---
type: Business Entity
title: Date
description: Represents calendar and fiscal reporting dates.
tags:
  - Date
  - Dimension
---

# Date

## Business definition
Stores calendar and fiscal date information used for analyzing bookings across time periods.

## Technical mapping
- Table: `dim_date`

## Attributes
- `date_key`
- `full_date`
- `month_name`
- `calendar_year`
- `fiscal_year`
- `fiscal_quarter`
- `fiscal_period_seq`

## Keys
- Primary key: `date_key`

## Measures
None

## Relationships
- Referenced by [Booking occurs on Date](../relationships/booking-occurs-on-date.md)

## Business notes
Glossary includes additional date terms not implemented in DDL.

## Confidence score
1.00

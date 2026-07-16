---
type: Business Entity
title: Date
description: Represents calendar and fiscal reporting dates.
tags:
  - Date
  - Dimension
---

# Date

## Business Definition
Stores calendar and fiscal date information used for analyzing bookings across different time periods.

## Technical Mapping
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
- Primary Key: `date_key`

## Measures
None

## Relationships
- [Booking occurs on Date](../relationships/booking_occurs_on_date.md)

## Business Notes
Glossary indicates richer date attributes than physical model. Confidence score: 0.90

---
type: Business Entity
title: Geography
description: Represents geographic hierarchy for bookings analysis.
tags:
  - Geography
---

# Geography

## Business definition
Provides hierarchical geographic information for analyzing bookings across global regions.

## Technical mapping
- Table: `dim_geography`
- Primary key: `geography_key`
- Glossary alias: `geo_key`

## Attributes
- `geography_key`
- `region`
- `theater`
- `country`

## Relationships
- [Booking associated with Geography](../relationships/booking-associated-with-geography.md)

## Measures
None

## Business notes
There is a naming mismatch between physical and glossary key names.

## Confidence score
0.92

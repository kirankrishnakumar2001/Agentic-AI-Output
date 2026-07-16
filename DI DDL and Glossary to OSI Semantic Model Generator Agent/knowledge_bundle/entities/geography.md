---
type: Business Entity
title: Geography
description: Represents regional and country reporting locations.
tags:
  - Geography
  - Dimension
---

# Geography

## Business definition
Provides hierarchical geographic information for analyzing bookings across global regions.

## Technical mapping
- Table: `dim_geography`
- Key mapping note: glossary `geo_key` corresponds to DDL `geography_key`

## Attributes
- `geography_key`
- `region`
- `theater`
- `country`

## Keys
- Primary key: `geography_key`

## Measures
None

## Relationships
- Referenced by [Booking is assigned to Geography](../relationships/booking-is-assigned-to-geography.md)

## Business notes
Contains hierarchy-like reporting levels within one dimension table.

## Confidence score
0.95

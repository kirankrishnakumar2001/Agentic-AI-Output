---
type: Business Domain
title: Geography
description: Geographic analysis domain for regional sales reporting.
tags:
  - Geography
---

# Geography Domain

## Business Purpose
Supports analysis of bookings by region, theater, and country.

## Technical Mapping
- Table: `dim_geography`

## Related Entities
- [Geography](../entities/geography.md)
- [Booking](../entities/booking.md)

## Related Measures
Booking measures are analyzable by geographic context.

## Notes
Supported by DDL and glossary with a key naming mismatch between `geo_key` and `geography_key`. Confidence score: 0.85

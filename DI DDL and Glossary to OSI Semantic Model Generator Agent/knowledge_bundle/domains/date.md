---
type: Business Domain
title: Date
description: Time analysis domain covering calendar and fiscal reporting structures.
tags:
  - Date
  - Time
---

# Date Domain

## Business Purpose
Enables analysis of bookings across calendar and fiscal periods.

## Technical Mapping
- Table: `dim_date`

## Related Entities
- [Date](../entities/date.md)
- [Booking](../entities/booking.md)

## Related Measures
Referenced by all booking measures for time-based slicing.

## Notes
Supported by DDL and glossary, but glossary includes additional attributes not physically present. Confidence score: 0.90

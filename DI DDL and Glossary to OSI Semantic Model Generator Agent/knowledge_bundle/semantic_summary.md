---
type: Semantic Summary
title: Cisco Bookings Semantic Summary
description: Summary of semantic overview, metrics, and quality observations for the Cisco bookings model.
tags:
  - Semantic Summary
  - Bookings
  - Metrics
---

# Cisco Bookings Semantic Summary

## Overview
- Business subject area: bookings analytics
- Core fact: booked order line transactions
- Primary analytical perspectives: date, customer, product, partner, geography, sales representative, and contract

## Business Questions
1. What is total booking amount by fiscal quarter, region, and product family?
2. How do renewal bookings and ACV differ by customer segment and contract type?
3. Which partners, sales teams, and technology domains generate the highest TCV in each region?

## Key Findings
- The schema is a clean star schema with one fact table and seven dimensions.
- Explicit foreign keys provide strong semantic relationship confidence.
- Glossary coverage is high, with minor gaps in the date and geography areas.
- Measures support revenue, quantity, pricing, and contract value analysis.

## Related Documents
- [Metrics](metrics.md)
- [Bookings Domain](domains/bookings.md)
- [Booking Entity](entities/booking.md)

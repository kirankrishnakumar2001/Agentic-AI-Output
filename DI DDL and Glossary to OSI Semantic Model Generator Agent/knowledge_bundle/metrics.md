---
type: Metrics Report
title: Cisco Bookings Semantic Metrics
description: Metrics and quality indicators for the Cisco bookings semantic model.
tags:
  - Metrics
  - Quality
  - Coverage
---

# Cisco Bookings Semantic Metrics

## Overall Metrics
- Total Tables: 8
- Total Columns: 48
- Total Relationships: 7
- Total Domains: 8
- Total Measures: 6
- Glossary Coverage: 96.0%

## Quality Indicators
- Orphan Tables: 0
- Many-to-Many Relationships: 0
- Deepest Relationship Chain: 2 hops
- Nullable Column Percentage: 72.9%

## Coverage Gaps
- Glossary contains date attributes not present in DDL: `day_of_week`, `month_number`, `calendar_quarter`
- Glossary uses `geo_key` while DDL uses `geography_key`

## Related Documents
- [Semantic Summary](semantic_summary.md)
- [Glossary Index](glossary/index.md)

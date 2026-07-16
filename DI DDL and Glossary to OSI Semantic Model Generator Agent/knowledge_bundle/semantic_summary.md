---
type: Semantic Summary
title: Cisco Bookings Semantic Summary
description: Summary of semantic overview, metrics, and quality observations for the Cisco bookings model.
tags:
  - Summary
  - Metrics
  - Quality
---

# Semantic Summary

## Overview
- Subject area: Cisco bookings analytics
- Core fact: [Booking](entities/booking.md)
- Core dimensions: [Date](entities/date.md), [Customer](entities/customer.md), [Product](entities/product.md), [Partner](entities/partner.md), [Geography](entities/geography.md), [Sales Representative](entities/sales-representative.md), [Contract](entities/contract.md)

## Business questions
1. What is total booking amount, ACV, and TCV by fiscal quarter, region, and product family?
2. How do renewal bookings and discount percentages vary by customer segment, contract type, and route to market?
3. Which sales teams, partners, and technology domains generate the highest booking revenue across fiscal years?

## Highlights
- Single fact table star schema
- Strong glossary alignment with minor date and geography gaps
- No broken foreign keys or orphan entities

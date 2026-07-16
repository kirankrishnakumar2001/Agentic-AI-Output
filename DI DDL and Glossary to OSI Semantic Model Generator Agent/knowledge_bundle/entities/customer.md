---
type: Business Entity
title: Customer
description: Represents customers who purchase products or services.
tags:
  - Customer
  - Dimension
---

# Customer

## Business definition
Contains descriptive information about customers who purchase products or services.

## Technical mapping
- Table: `dim_customer`

## Attributes
- `customer_key`
- `customer_id`
- `customer_name`
- `segment`
- `industry`
- `account_tier`
- `hq_country`
- `hq_region`

## Keys
- Primary key: `customer_key`
- Business key: `customer_id`

## Measures
None

## Relationships
- Referenced by [Booking is for Customer](../relationships/booking-is-for-customer.md)

## Business notes
Strong business segmentation semantics.

## Confidence score
1.00

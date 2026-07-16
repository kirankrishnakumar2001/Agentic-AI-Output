---
type: Business Entity
title: Product
description: Represents products and services sold through bookings.
tags:
  - Product
  - Dimension
---

# Product

## Business definition
Contains descriptive information about products and services sold through bookings.

## Technical mapping
- Table: `dim_product`

## Attributes
- `product_key`
- `product_id`
- `product_name`
- `product_family`
- `technology_domain`
- `offer_type`
- `business_entity`

## Keys
- Primary key: `product_key`
- Business key: `product_id`

## Measures
None

## Relationships
- Referenced by [Booking includes Product](../relationships/booking-includes-product.md)

## Business notes
Supports product family and technology analysis.

## Confidence score
1.00

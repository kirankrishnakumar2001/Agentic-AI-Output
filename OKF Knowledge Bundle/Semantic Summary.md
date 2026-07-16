# Semantic Summary

## Semantic Overview

### Business Subject Area
Cisco bookings analytics focused on booked order lines, revenue-related measures, customer, product, partner, geography, sales, contract, and time dimensions.

### Data Domain Description
The source model is a Kimball-style star schema centered on `fact_bookings`, with supporting dimensions for date, customer, product, partner, geography, sales representative, and contract. It supports dimensional analysis of bookings performance across commercial, organizational, geographic, and fiscal perspectives.

### Business Value Proposition
This semantic model enables a business-centric understanding of bookings performance by linking measurable booking events to core descriptive dimensions. It supports revenue analysis, contract valuation, pricing and discount analysis, partner/channel performance, sales accountability, and fiscal reporting.

### Business Capabilities Enabled
- Bookings and revenue performance analysis
- Customer segment and industry analysis
- Product portfolio and technology-domain analysis
- Channel and partner performance analysis
- Geographic and theater reporting
- Sales representative performance analysis
- Contract term and renewal analysis
- Fiscal period and time-series reporting

### Three Business Questions
1. What is total booking amount, ACV, and TCV by fiscal quarter, region, and product family?
2. Which customers, segments, and industries generate the highest booking revenue and contract value?
3. How do discount percentage, booking type, and partner route-to-market affect booking outcomes and renewal patterns?

---

## Semantic Metrics Report

### Overall Metrics
- **Total Tables:** 8
- **Total Columns:** 44
- **Total Relationships:** 7 explicit foreign-key relationships
- **Total Domains:** 4
- **Total Measures:** 6
- **Total Glossary Terms Mapped from DDL Objects:** 44
- **Total Glossary Attribute/Term Entries Identified:** 46
- **Mapped Terms:** 44
- **Glossary Coverage Percentage:** 95.65%

### Schema Statistics
- **Fact Tables:** 1 (`fact_bookings`)
- **Dimension Tables:** 7 (`dim_date`, `dim_customer`, `dim_product`, `dim_partner`, `dim_geography`, `dim_sales_rep`, `dim_contract`)
- **Lookup Tables:** 0 standalone lookup tables
- **Bridge Tables:** 0
- **Composite Primary Keys:** 0
- **Degenerate Dimensions:** `order_number`, `order_line_number`

### Per-Domain Metrics

#### Revenue Bookings Domain
- **Entity Count:** 1
- **Attribute Count:** 6 descriptive attributes/flags
- **Measure Count:** 6
- **Glossary Coverage Percentage:** 100%
- **Domain Status:** Healthy

#### Customer & Sales Domain
- **Entity Count:** 3
- **Attribute Count:** 19
- **Measure Count:** 0
- **Glossary Coverage Percentage:** 100%
- **Domain Status:** Healthy

#### Product & Partner Domain
- **Entity Count:** 2
- **Attribute Count:** 11
- **Measure Count:** 0
- **Glossary Coverage Percentage:** 100%
- **Domain Status:** Healthy

#### Time, Geography & Contract Domain
- **Entity Count:** 3
- **Attribute Count:** 14
- **Measure Count:** 0
- **Glossary Coverage Percentage:** 87.50%
- **Domain Status:** Healthy

### Glossary Coverage Statistics
- **Mapped Table/Column Terms:** 44
- **Unmapped Glossary Terms:** 2
- **Unmapped Glossary Terms Identified:** `day_of_week`, `month_number`, `calendar_quarter` (present in glossary but absent in DDL), offset by glossary use of `geo_key` where DDL uses `geography_key`
- **Ambiguous Mappings:** 1 (`geo_key` glossary term maps semantically to `geography_key` in DDL)
- **Duplicate Glossary Terms:** None detected
- **Invalid Glossary Mappings:** None critical; one naming discrepancy noted for geography key

---

## Semantic Quality & Reasoning Report

### Validation Warnings
- No unreadable DDL or glossary content after corrected file path access.
- No duplicate table definitions detected.
- No duplicate glossary term blocks detected.
- No invalid column definitions detected.
- No missing primary keys detected.
- No broken foreign keys detected.
- **Warning:** Glossary uses `geo_key` while DDL defines `geography_key` in `dim_geography` and `fact_bookings`.
- **Warning:** Glossary includes date attributes `day_of_week`, `month_number`, and `calendar_quarter` that are not present in the DDL.

### Design Observations
- **Deepest Relationship Chain:** 2 hops in the star pattern (dimension -> fact -> dimension)
- **Orphan Tables:** 0
- **Many-to-Many Relationships:** 0 explicit; none inferred because no bridge tables exist
- **Nullable Column Percentage:** Approximately 79.55% (35 of 44 columns nullable, based on explicit `NOT NULL` constraints only)
- **Model Pattern:** Clean star schema with a single central fact and conformed dimensions
- **Semantic Structure:** Strong analytical orientation with explicit dimensionality and additive revenue measures

### Glossary Health
- **Unmapped Glossary Terms Count:** 3 direct glossary-only attributes (`day_of_week`, `month_number`, `calendar_quarter`)
- **Ambiguous Mappings Count:** 1 (`geo_key` vs `geography_key`)
- **Domain Coverage:** All discovered domains have glossary support
- **Coverage Gaps:** Date dimension glossary is richer than implemented physical schema

### Recommendations

#### Strong Semantic Domains
- Revenue Bookings
- Customer & Sales
- Product & Partner

#### Weak or Thinner Areas
- Contract semantics are structurally present but comparatively light in descriptive richness.
- Geography is useful but minimal, with only region, theater, and country.

#### Sparse Modeling
- No standalone reference tables for booking type, offer type, partner type, tiering, or route-to-market values.
- No customer hierarchy, product hierarchy beyond family, or sales territory hierarchy beyond team/segment coverage.

#### Broken Lineage
- No broken referential lineage detected from DDL.
- Semantic lineage caution exists for the glossary-to-DDL discrepancy between `geo_key` and `geography_key`.

#### Suggested Join Paths
- Bookings to Time: `fact_bookings.date_key -> dim_date.date_key`
- Bookings to Customer: `fact_bookings.customer_key -> dim_customer.customer_key`
- Bookings to Product: `fact_bookings.product_key -> dim_product.product_key`
- Bookings to Partner: `fact_bookings.partner_key -> dim_partner.partner_key`
- Bookings to Geography: `fact_bookings.geography_key -> dim_geography.geography_key`
- Bookings to Sales Rep: `fact_bookings.sales_rep_key -> dim_sales_rep.sales_rep_key`
- Bookings to Contract: `fact_bookings.contract_key -> dim_contract.contract_key`
- Common analytical path: Bookings -> Customer + Product + Date for customer-product-period performance

#### Data Quality Watch-Outs
- `is_renewal` is an integer flag; semantic governance should constrain to valid values (0/1).
- `auto_renew_flag` is a single-character field; allowed values should be standardized.
- `discount_pct` should be validated for allowable percentage range.
- `unit_list_price_usd` is non-additive and should not be summed without business rules.
- Surrogate keys are primary keys but natural key uniqueness is not enforced in DDL for identifiers such as `customer_id`, `product_id`, `partner_id`, and `rep_id`.
- Fact foreign keys are nullable in DDL, which may permit incomplete dimensional context unless constrained upstream.

### Phase 12 Validation
- Missing glossary mappings: Only glossary-only date terms and geography key naming mismatch.
- Duplicate entities: None detected.
- Duplicate measures: None detected.
- Circular relationships: None detected.
- Missing keys: None detected at table primary key level.
- Invalid semantic domains: None detected.
- Orphan entities: None detected.

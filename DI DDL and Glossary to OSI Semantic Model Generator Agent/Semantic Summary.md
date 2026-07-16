# Semantic Summary

## Semantic Overview

### Business Subject Area
Cisco bookings, sales performance, customer revenue, product portfolio, partner channel, contract, and geographic sales analysis.

### Data Domain Description
The source model is a Kimball-style star schema centered on `fact_bookings`, with surrounding dimensions for date, customer, product, partner, geography, sales representative, and contract. The model supports analysis of booked order lines using additive and non-additive commercial measures. The glossary confirms that the business focus is bookings analytics across time, customer, product, channel, sales, contract, and geography perspectives.

### Business Value Proposition
This semantic model provides a business-centric representation of booking transactions so downstream consumers can consistently analyze revenue, contract value, pricing, discounting, renewals, customer segmentation, partner contribution, product performance, and regional sales trends.

### Business Capabilities Enabled
- Booking revenue analysis by time, customer, product, geography, partner, sales rep, and contract
- Renewal and new business tracking
- Contract value analysis using ACV and TCV
- Discount and list price monitoring
- Channel and route-to-market performance analysis
- Customer segment and industry performance reporting
- Product family and technology domain performance analysis
- Fiscal and calendar period reporting

### Three Business Questions
1. What is total booking amount by fiscal quarter, region, and product family?
2. How do renewal bookings and ACV differ by customer segment and contract type?
3. Which partners, sales teams, and technology domains generate the highest TCV in each region?

## Semantic Metrics Report

### Overall Metrics
- Total Tables: 8
- Total Columns: 48
- Total Relationships: 7 explicit foreign key relationships
- Total Domains: 8
- Total Measures: 6
- Total Glossary Terms Identified: 50
- Mapped Terms: 48
- Glossary Coverage Percentage: 96.0%

### Schema Statistics
- Fact Tables: 1 (`fact_bookings`)
- Dimension Tables: 7 (`dim_date`, `dim_customer`, `dim_product`, `dim_partner`, `dim_geography`, `dim_sales_rep`, `dim_contract`)
- Lookup Tables: 0
- Bridge Tables: 0
- Composite Keys: 0
- Degenerate Dimensions: 2 (`order_number`, `order_line_number`)
- Primary Keys Present: 8 of 8 tables
- Foreign Keys Present: 7

### Per-Domain Metrics
| Domain Name | Entity Count | Attribute Count | Measure Count | Glossary Coverage % | Domain Status |
|---|---:|---:|---:|---:|---|
| Bookings | 1 | 12 | 6 | 100% | Healthy |
| Date | 1 | 6 | 0 | 60% | Thin |
| Customer | 1 | 7 | 0 | 100% | Healthy |
| Product | 1 | 6 | 0 | 100% | Healthy |
| Partner | 1 | 6 | 0 | 100% | Healthy |
| Geography | 1 | 4 | 0 | 75% | Thin |
| Sales | 1 | 6 | 0 | 100% | Healthy |
| Contract | 1 | 5 | 0 | 100% | Healthy |

### Glossary Coverage Statistics
- Fully mapped table-level glossary sections: 8 of 8
- Columns with direct glossary match: 46
- Columns with inferred glossary match: 2
- Unmapped columns in DDL: 0
- Glossary attributes not present in DDL: 4
  - `day_of_week`
  - `month_number`
  - `calendar_quarter`
  - `geo_key` used in glossary where DDL uses `geography_key`
- Ambiguous mappings: 1
  - `geography_key` ↔ glossary `geo_key`
- Duplicate glossary terms detected: 0
- Invalid glossary mappings: none materially broken; one naming mismatch noted for geography key

## Semantic Quality & Reasoning Report

### Validation Warnings
- No missing tables detected.
- No duplicate table definitions detected.
- No duplicate glossary terms detected.
- No invalid column definitions detected in the DDL.
- No missing primary keys detected.
- No broken foreign keys detected.
- Warning: glossary references `geo_key` while DDL uses `geography_key` in both `dim_geography` and `fact_bookings`.
- Warning: glossary includes `day_of_week`, `month_number`, and `calendar_quarter` for `dim_date`, but these columns do not exist in the DDL.

### Design Observations
- Deepest relationship chain: 2 hops in the star pattern (`fact_bookings` → any dimension).
- Orphan tables: 0
- Many-to-many relationships: 0 explicit; none inferred because no bridge tables exist.
- Nullable column percentage: approximately 72.9% (35 of 48 columns are nullable based on DDL `NOT NULL` declarations).
- Schema style: well-formed star schema with a central transactional fact and descriptive dimensions.
- Fact grain clarity: explicit and strong; one row per booked order line.

### Glossary Health
- Unmapped glossary terms: 4 attribute-level terms not represented exactly in DDL
- Ambiguous mappings: 1
- Domain coverage: strong across all business areas represented in the schema
- Coverage gaps:
  - Date domain glossary is richer than the physical model
  - Geography key naming is inconsistent between glossary and DDL

### Recommendations
#### Strong Semantic Domains
- Bookings
- Customer
- Product
- Partner
- Sales
- Contract

#### Weak Semantic Domains
- Date, because several glossary attributes are absent from the schema
- Geography, because of glossary-to-schema key naming inconsistency

#### Sparse Modeling
- No severe sparsity in the core business model
- Dimension-only domains are naturally attribute-focused and contain no measures

#### Broken Lineage
- No broken lineage found in referential structure
- Minor semantic lineage issue between `geo_key` and `geography_key`

#### Suggested Join Paths
- Bookings → Date on `fact_bookings.date_key = dim_date.date_key`
- Bookings → Customer on `fact_bookings.customer_key = dim_customer.customer_key`
- Bookings → Product on `fact_bookings.product_key = dim_product.product_key`
- Bookings → Partner on `fact_bookings.partner_key = dim_partner.partner_key`
- Bookings → Geography on `fact_bookings.geography_key = dim_geography.geography_key`
- Bookings → Sales Rep on `fact_bookings.sales_rep_key = dim_sales_rep.sales_rep_key`
- Bookings → Contract on `fact_bookings.contract_key = dim_contract.contract_key`

#### Data Quality Watch-outs
- `is_renewal` is modeled as integer rather than constrained boolean semantics
- `discount_pct` lacks explicit range constraint validation in DDL
- Natural business identifiers such as `customer_id`, `product_id`, `partner_id`, and `rep_id` are `NOT NULL` but not declared unique
- Several descriptive dimension attributes are nullable and may reduce segmentation quality
- `unit_list_price_usd` is a rate and should not be aggregated without business rules

### Semantic Reasoning Notes
- `fact_bookings` is classified as a fact table due to additive measures and foreign keys to multiple dimensions.
- All `dim_*` tables are dimensions based on descriptive attributes and single-column surrogate primary keys.
- `order_number` and `order_line_number` are degenerate dimensions because they reside in the fact table and describe transactional context.
- Measures inferred from glossary and DDL comments:
  - Additive: `quantity`, `booking_amount_usd`, `acv_usd`, `tcv_usd`
  - Non-additive or semi-analytic: `unit_list_price_usd`, `discount_pct`
- Relationship confidence is high for all seven fact-to-dimension joins because they are explicit foreign keys.

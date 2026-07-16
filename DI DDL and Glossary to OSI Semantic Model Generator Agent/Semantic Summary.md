# Semantic Summary

## Semantic Overview

### Business subject area
Cisco bookings analytics for revenue, contract, customer, product, partner, sales, geography, and time analysis.

### Data domain description
The source model is a Kimball-style star schema centered on `fact_bookings` at the grain of one booked order line. It supports multidimensional analysis of booking transactions across seven conformed dimensions: date, customer, product, partner, geography, sales representative, and contract. The glossary confirms that the model is intended for booking performance, revenue analysis, channel analysis, customer segmentation, and fiscal reporting.

### Business value proposition
This semantic model turns technical warehouse objects into business-aligned entities and measures so analytics teams can consistently answer bookings, revenue, renewal, pricing, customer, partner, and product performance questions. It establishes shared terminology, traceable glossary mappings, and reusable semantic metadata for downstream ontology, knowledge graph, and semantic search use cases.

### Business capabilities enabled
- Bookings and revenue analysis by customer, product, geography, partner, sales rep, and time
- Fiscal and calendar reporting
- Renewal and contract performance analysis
- Channel and route-to-market analysis
- Product portfolio and technology-domain performance analysis
- Sales coverage and segment performance analysis
- Semantic search and governed metadata reuse

### Three business questions
1. What is total booking amount, ACV, and TCV by fiscal quarter, region, and product family?
2. How do renewal bookings and discount percentages vary by customer segment, contract type, and route to market?
3. Which sales teams, partners, and technology domains generate the highest booking revenue across fiscal years?

## Semantic Metrics Report

### Overall Metrics
| Metric | Value |
|---|---:|
| Total Tables | 8 |
| Total Columns | 49 |
| Total Relationships | 7 |
| Total Domains | 8 |
| Total Measures | 6 |
| Total Glossary Terms | 49 |
| Mapped Terms | 47 |
| Glossary Coverage Percentage | 95.92% |

### Schema statistics
- Fact tables: 1 (`fact_bookings`)
- Dimension tables: 7 (`dim_date`, `dim_customer`, `dim_product`, `dim_partner`, `dim_geography`, `dim_sales_rep`, `dim_contract`)
- Lookup tables: 0 explicit standalone lookup tables
- Bridge tables: 0
- Composite primary keys: 0
- Foreign keys discovered: 7
- Degenerate dimensions in fact: `order_number`, `order_line_number`

### Per-Domain Metrics
| Domain Name | Entity Count | Attribute Count | Measure Count | Glossary Coverage % | Domain Status |
|---|---:|---:|---:|---:|---|
| Bookings | 1 | 18 | 6 | 100.00% | Healthy |
| Date | 1 | 6 | 0 | 75.00% | Thin |
| Customer | 1 | 8 | 0 | 100.00% | Healthy |
| Product | 1 | 7 | 0 | 100.00% | Healthy |
| Partner | 1 | 6 | 0 | 100.00% | Healthy |
| Geography | 1 | 4 | 0 | 75.00% | Thin |
| Sales | 1 | 6 | 0 | 100.00% | Healthy |
| Contract | 1 | 5 | 0 | 100.00% | Healthy |

### Glossary Coverage Statistics
- Exact or near-exact mapped glossary terms: 47
- Unmapped glossary terms: 2
  - `day_of_week` in glossary but not present in DDL
  - `month_number` and `calendar_quarter` also appear in glossary but not present in DDL; these are treated as glossary-only terms, contributing to coverage gaps in Date domain
- Technical columns without direct glossary term match: 2
  - `geography_key` in DDL vs glossary term `geo_key`
  - `booking_amount_usd` in DDL vs glossary definition wording aligns strongly
- Ambiguous mappings: 1 low-risk naming variance (`geography_key` ↔ `geo_key`)
- Duplicate glossary terms: none detected

## Semantic Quality & Reasoning Report

### Validation warnings
- No duplicate table definitions found.
- No duplicate glossary terms found.
- No broken foreign keys found.
- No invalid column definitions found.
- No missing primary keys found.
- Naming inconsistency detected: DDL uses `geography_key` while glossary uses `geo_key` for Geography and Fact Bookings references.
- Glossary contains attributes not implemented in DDL: `day_of_week`, `month_number`, `calendar_quarter`.

### Design observations
- Deepest relationship chain: 2 hops in the star pattern, e.g. `fact_bookings -> dim_customer` or any fact-to-dimension path. There are no snowflaked dimension chains.
- Orphan tables: none. Every dimension is referenced by the fact table.
- Many-to-many relationships: none explicit. Model is pure star schema.
- Nullable column percentage: approximately 71.43% (35 of 49 columns do not specify `NOT NULL` or `PRIMARY KEY` constraints explicitly beyond keys).
- Fact grain clarity: strong. Grain is explicitly stated as one row per booked order line.
- Constraint richness: moderate. PKs and FK references exist, but few business validation constraints beyond data types.

### Glossary health
- Domain coverage is strong for Customer, Product, Partner, Sales, Contract, and Bookings.
- Coverage gaps exist in Date and Geography due to mismatch between glossary and DDL attribute names or missing DDL columns.
- Unmapped glossary-only terms: `day_of_week`, `month_number`, `calendar_quarter`.
- Ambiguous mapping count: 1 (`geo_key` vs `geography_key`).

### Recommendations
#### Strong semantic domains
- Bookings
- Customer
- Product
- Partner
- Sales
- Contract

#### Weak semantic domains
- Date, because glossary describes richer calendar semantics than the physical table implements.
- Geography, because of key-name mismatch between technical and glossary metadata.

#### Sparse modeling
- No explicit reference-data tables for booking types, offer types, partner tiers, or sales roles.
- No additional dimensional hierarchy tables; hierarchies are flattened into dimensions.

#### Broken lineage
- No broken lineage in FK structure.
- Minor semantic lineage ambiguity around `geography_key`/`geo_key` naming.

#### Suggested join paths
- Bookings to Date: `fact_bookings.date_key = dim_date.date_key`
- Bookings to Customer: `fact_bookings.customer_key = dim_customer.customer_key`
- Bookings to Product: `fact_bookings.product_key = dim_product.product_key`
- Bookings to Partner: `fact_bookings.partner_key = dim_partner.partner_key`
- Bookings to Geography: `fact_bookings.geography_key = dim_geography.geography_key`
- Bookings to Sales Representative: `fact_bookings.sales_rep_key = dim_sales_rep.sales_rep_key`
- Bookings to Contract: `fact_bookings.contract_key = dim_contract.contract_key`

#### Data quality watch-outs
- `is_renewal` is modeled as integer rather than constrained boolean/flag domain.
- `discount_pct` lacks explicit range validation.
- `order_number` and `order_line_number` are degenerate dimensions with no explicit uniqueness constraint as a composite business key.
- Several descriptive columns are nullable, which may reduce dimensional completeness in reporting.
- `booking_amount_usd`, `acv_usd`, and `tcv_usd` are additive by glossary intent, but no database-level checks ensure non-negative values.

### Semantic reasoning notes
- `fact_bookings` is classified as a fact table because it contains additive business measures and multiple foreign keys to descriptive dimensions.
- All `dim_*` tables are classified as dimensions because they primarily contain descriptive context and surrogate keys.
- `quantity`, `booking_amount_usd`, `acv_usd`, and `tcv_usd` are measures with strong confidence because glossary definitions explicitly describe measurable values.
- `unit_list_price_usd` and `discount_pct` are treated as analytic measures with non-additive or rate semantics.
- Business entity names are taken directly from glossary-preferred table concepts where available.

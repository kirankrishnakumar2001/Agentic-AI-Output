# Semantic Summary

## Semantic Overview

### Business subject area
Cisco bookings and revenue performance analytics for booked order lines across customers, products, partners, geography, sales, contracts, and time.

### Data domain description
The source model is a star schema centered on `fact_bookings` at the grain of one row per booked order line. It is supported by seven dimensions: Date, Customer, Product, Partner, Geography, Sales Representative, and Contract. The glossary provides business meaning for nearly all modeled structures and confirms that the dataset is intended for booking analysis, fiscal reporting, customer segmentation, channel performance, and product portfolio analysis.

### Business value proposition
This semantic model enables business-centric analysis of bookings, contract value, pricing, discounting, customer segmentation, channel effectiveness, geographic performance, and sales execution. It provides a reusable intermediate layer for ontology generation, semantic search, metadata management, and downstream AI applications.

### Business capabilities enabled
- Analyze booking revenue by fiscal period, quarter, and year
- Track Annual Contract Value (ACV) and Total Contract Value (TCV)
- Compare bookings across customer segments, industries, and tiers
- Evaluate product family and technology domain performance
- Assess partner channel contribution and route-to-market effectiveness
- Measure geographic and theater-level booking outcomes
- Evaluate sales representative and team performance
- Analyze booking mix across new, renewal, and upsell transactions
- Support governed semantic mapping between technical schema and business terminology

### Three business questions
1. What is total booking amount, ACV, and TCV by fiscal quarter, region, and product family?
2. Which customer segments and partner tiers generate the highest booking revenue and renewal volume?
3. How do discounts, contract term months, and booking type affect booked revenue across sales teams?

---

## Semantic Metrics Report

### Overall Metrics
| Metric | Value |
|---|---:|
| Total Tables | 8 |
| Total Columns | 58 |
| Total Relationships | 7 |
| Total Domains | 5 |
| Total Measures | 6 |
| Total Glossary Terms (table + attribute level discovered) | 66 |
| Mapped Physical Columns | 56 |
| Unmapped Physical Columns | 2 |
| Glossary Coverage Percentage | 96.6% |

### Schema statistics
- Fact tables: 1 (`fact_bookings`)
- Dimension tables: 7 (`dim_date`, `dim_customer`, `dim_product`, `dim_partner`, `dim_geography`, `dim_sales_rep`, `dim_contract`)
- Lookup tables: 0 explicitly modeled
- Bridge tables: 0
- Composite primary keys: 0
- Foreign keys: 7
- Degenerate dimensions in fact: `order_number`, `order_line_number`

### Per-Domain Metrics
| Domain Name | Entity Count | Attribute Count | Measure Count | Glossary Coverage | Domain Status |
|---|---:|---:|---:|---:|---|
| Time | 1 | 7 | 0 | 71.4% | Thin |
| Customer | 1 | 8 | 0 | 100.0% | Thin |
| Product | 1 | 7 | 0 | 100.0% | Thin |
| Channel & Sales | 3 | 17 | 0 | 100.0% | Healthy |
| Bookings & Contracts | 2 | 19 | 6 | 100.0% | Healthy |

### Glossary Coverage Statistics
- Fully mapped tables: `dim_date`, `dim_customer`, `dim_product`, `dim_partner`, `dim_sales_rep`, `dim_contract`, `fact_bookings`
- Partially mapped table: `dim_geography`
- Unmapped DDL columns relative to glossary: `dim_geography.geography_key`, `fact_bookings.geography_key`
- Glossary terms present but not in DDL: `day_of_week`, `month_number`, `calendar_quarter`
- Ambiguous mappings: low ambiguity; `geo_key` in glossary corresponds semantically to `geography_key` in DDL
- Duplicate glossary terms: none detected in supplied text

---

## Semantic Quality & Reasoning Report

### Phase 1 – Input Validation
#### DDL readability
- Valid and readable SQL DDL detected.
- All tables are syntactically understandable and consistently named.

#### Glossary readability
- Glossary text is readable and organized by table and attribute.
- Business definitions exist for all listed tables and almost all DDL columns.

#### Validation warnings
- No missing tables between glossary and DDL.
- No duplicate table definitions detected.
- No duplicate glossary terms detected in the supplied glossary text.
- No invalid column definitions detected.
- No missing primary keys detected.
- No broken foreign keys detected.
- Warning: glossary uses `geo_key` while DDL uses `geography_key`; treated as a high-confidence semantic match, but technically inconsistent naming.
- Warning: glossary contains `day_of_week`, `month_number`, and `calendar_quarter` for `dim_date`, but these columns are not present in DDL.

### Phase 4 – Semantic Domain Discovery
#### Domains discovered
1. **Time**  
   Description: Calendar and fiscal structures used for period-based analysis.
2. **Customer**  
   Description: Customer identity, segmentation, and account classification.
3. **Product**  
   Description: Product, portfolio, and technology categorization.
4. **Channel & Sales**  
   Description: Partner, geography, and sales execution context for bookings.
5. **Bookings & Contracts**  
   Description: Booking transactions, revenue measures, and contract context.

### Phase 5 – Business Entity Discovery
#### Business entities identified
- Date
- Customer
- Product
- Partner
- Geography
- Sales Representative
- Contract
- Booking

#### Measures identified
- Quantity
- Unit List Price USD
- Discount Percent
- Booking Amount USD
- ACV USD
- TCV USD

#### Dimensions identified
- Date, Customer, Product, Partner, Geography, Sales Representative, Contract

#### Reference data candidates
- `booking_type`
- `offer_type`
- `partner_type`
- `partner_tier`
- `route_to_market`
- `segment`
- `industry`
- `account_tier`
- `sales_role`
- `sales_team`
- `coverage_level`

### Phase 6 – Relationship Discovery
| Relationship Name | Parent Entity | Child Entity | Cardinality | Relationship Type | Confidence |
|---|---|---|---|---|---:|
| Booking occurs on Date | Date | Booking | 1-to-many | Foreign key | 1.00 |
| Booking is for Customer | Customer | Booking | 1-to-many | Foreign key | 1.00 |
| Booking includes Product | Product | Booking | 1-to-many | Foreign key | 1.00 |
| Booking sold via Partner | Partner | Booking | 1-to-many | Foreign key | 1.00 |
| Booking associated with Geography | Geography | Booking | 1-to-many | Foreign key with naming reconciliation | 0.92 |
| Booking owned by Sales Representative | Sales Representative | Booking | 1-to-many | Foreign key | 1.00 |
| Booking governed by Contract | Contract | Booking | 1-to-many | Foreign key | 1.00 |

### Design observations
- Deepest relationship chain: 1 hop from `fact_bookings` to each connected dimension in the physical model.
- Orphan tables: none.
- Many-to-many relationships: none explicitly modeled.
- Nullable column percentage: approximately 79.3% (46 nullable columns of 58 total; primary keys and some natural keys are non-null).
- Star schema quality: strong and analytically aligned.
- Fact grain clarity: explicit and high quality.

### Glossary health
- Unmapped glossary terms: 3 attribute terms (`day_of_week`, `month_number`, `calendar_quarter`)
- Ambiguous mappings: 1 naming mismatch (`geo_key` ↔ `geography_key`)
- Domain coverage: all discovered domains have glossary support
- Coverage gaps: Time domain has incomplete attribute coverage due to glossary-only date attributes absent in DDL

### Recommendations
#### Strong semantic domains
- Bookings & Contracts
- Channel & Sales
- Customer
- Product

#### Weak semantic domains
- Time, due to missing physical columns relative to glossary expectations

#### Sparse modeling
- No separate reference/lookup tables exist for code-like descriptive categories; this is acceptable in a denormalized dimensional model but should be documented.

#### Broken lineage
- No broken lineage detected in FK structure.
- Naming inconsistency between geography key fields should be standardized.

#### Suggested join paths
- Booking → Date for fiscal and calendar time analysis
- Booking → Customer for account, segment, and industry analysis
- Booking → Product for portfolio and technology performance analysis
- Booking → Partner → route_to_market for channel effectiveness analysis
- Booking → Geography for regional and theater reporting
- Booking → Sales Representative for rep/team performance analysis
- Booking → Contract for contract duration and renewal analysis

#### Data quality watch-outs
- `is_renewal` is modeled as integer flag; enforce 0/1 domain values.
- `discount_pct` should be validated for acceptable percentage ranges.
- `unit_list_price_usd` is non-additive and should not be summed across rows without business rule context.
- `booking_amount_usd`, `acv_usd`, and `tcv_usd` should be checked for consistency with contract terms and booking type.
- Geographic key naming should be aligned between glossary and DDL.

### Phase 12 – Semantic Validation Warnings
- No duplicate entities detected.
- No duplicate measures detected.
- No circular relationships detected.
- No missing keys detected.
- No invalid semantic domains detected.
- No orphan entities detected.
- Warning retained for glossary/DDL mismatch on geography key naming.
- Warning retained for glossary-only date attributes not present physically.

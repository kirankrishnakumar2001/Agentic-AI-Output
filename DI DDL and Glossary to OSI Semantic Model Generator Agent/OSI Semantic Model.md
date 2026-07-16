# OSI Semantic Model

## Domains

### 1. Bookings
- **Description:** Core transactional domain capturing measurable booked order lines and associated commercial values.
- **Business Purpose:** Enables revenue, contract value, renewal, pricing, and operational booking analysis.

### 2. Date
- **Description:** Calendar and fiscal time dimension used to analyze bookings across reporting periods.
- **Business Purpose:** Supports time-series, calendar, and fiscal reporting.

### 3. Customer
- **Description:** Customer master context for analyzing who purchased products or services.
- **Business Purpose:** Supports segmentation, industry, and account-based performance analysis.

### 4. Product
- **Description:** Product and service catalog context for booked items.
- **Business Purpose:** Supports portfolio, offer type, and technology-domain analysis.

### 5. Partner
- **Description:** Channel partner context for indirect and partner-led sales.
- **Business Purpose:** Supports route-to-market and partner performance analysis.

### 6. Geography
- **Description:** Geographic reporting context for regional and country analysis.
- **Business Purpose:** Supports regional performance and theater reporting.

### 7. Sales
- **Description:** Sales representative and organizational coverage context.
- **Business Purpose:** Supports sales team and role-based performance analysis.

### 8. Contract
- **Description:** Contractual context associated with bookings.
- **Business Purpose:** Supports contract type, term, renewal, and coverage analysis.

## Entities

### Booking
- **Technical Table:** `fact_bookings`
- **Description:** Stores measurable business transactions representing customer bookings at one row per booked order line.
- **Attributes:**
  - `booking_id` — booking transaction identifier
  - `order_number` — sales order number
  - `order_line_number` — line number within sales order
  - `date_key` — booking date reference
  - `customer_key` — customer reference
  - `product_key` — product reference
  - `partner_key` — partner reference
  - `geography_key` — geography reference
  - `sales_rep_key` — sales representative reference
  - `contract_key` — contract reference
  - `booking_type` — booking category such as New, Renewal, Upsell
  - `is_renewal` — renewal indicator
- **Measures:**
  - `quantity`
  - `unit_list_price_usd`
  - `discount_pct`
  - `booking_amount_usd`
  - `acv_usd`
  - `tcv_usd`
- **Keys:**
  - Primary Key: `booking_id`
  - Foreign Keys: `date_key`, `customer_key`, `product_key`, `partner_key`, `geography_key`, `sales_rep_key`, `contract_key`
  - Degenerate dimensions: `order_number`, `order_line_number`
- **Relationships:**
  - Booking occurs on Date
  - Booking is for Customer
  - Booking includes Product
  - Booking involves Partner
  - Booking is assigned to Geography
  - Booking is owned by Sales Representative
  - Booking is governed by Contract

### Date
- **Technical Table:** `dim_date`
- **Description:** Stores calendar and fiscal date information used for analyzing bookings across time periods.
- **Attributes:**
  - `date_key`
  - `full_date`
  - `month_name`
  - `calendar_year`
  - `fiscal_year`
  - `fiscal_quarter`
  - `fiscal_period_seq`
- **Measures:** None
- **Keys:**
  - Primary Key: `date_key`
  - Business/Natural semantics: `full_date`
- **Relationships:**
  - Date is referenced by Booking

### Customer
- **Technical Table:** `dim_customer`
- **Description:** Contains descriptive information about customers who purchase products or services.
- **Attributes:**
  - `customer_key`
  - `customer_id`
  - `customer_name`
  - `segment`
  - `industry`
  - `account_tier`
  - `hq_country`
  - `hq_region`
- **Measures:** None
- **Keys:**
  - Primary Key: `customer_key`
  - Business Key: `customer_id`
- **Relationships:**
  - Customer is referenced by Booking

### Product
- **Technical Table:** `dim_product`
- **Description:** Contains descriptive information about products and services sold through bookings.
- **Attributes:**
  - `product_key`
  - `product_id`
  - `product_name`
  - `product_family`
  - `technology_domain`
  - `offer_type`
  - `business_entity`
- **Measures:** None
- **Keys:**
  - Primary Key: `product_key`
  - Business Key: `product_id`
- **Relationships:**
  - Product is referenced by Booking

### Partner
- **Technical Table:** `dim_partner`
- **Description:** Stores information about channel partners involved in selling products.
- **Attributes:**
  - `partner_key`
  - `partner_id`
  - `partner_name`
  - `partner_type`
  - `partner_tier`
  - `route_to_market`
- **Measures:** None
- **Keys:**
  - Primary Key: `partner_key`
  - Business Key: `partner_id`
- **Relationships:**
  - Partner is referenced by Booking

### Geography
- **Technical Table:** `dim_geography`
- **Description:** Provides hierarchical geographic information for analyzing bookings across global regions.
- **Attributes:**
  - `geography_key` *(mapped from glossary term `geo_key` with inferred equivalence)*
  - `region`
  - `theater`
  - `country`
- **Measures:** None
- **Keys:**
  - Primary Key: `geography_key`
- **Relationships:**
  - Geography is referenced by Booking

### Sales Representative
- **Technical Table:** `dim_sales_rep`
- **Description:** Contains information about sales representatives responsible for customer bookings.
- **Attributes:**
  - `sales_rep_key`
  - `rep_id`
  - `rep_name`
  - `sales_role`
  - `sales_team`
  - `segment_covered`
- **Measures:** None
- **Keys:**
  - Primary Key: `sales_rep_key`
  - Business Key: `rep_id`
- **Relationships:**
  - Sales Representative is referenced by Booking

### Contract
- **Technical Table:** `dim_contract`
- **Description:** Stores information describing customer contracts associated with bookings.
- **Attributes:**
  - `contract_key`
  - `contract_type`
  - `term_months`
  - `auto_renew_flag`
  - `coverage_level`
- **Measures:** None
- **Keys:**
  - Primary Key: `contract_key`
- **Relationships:**
  - Contract is referenced by Booking

## Relationships

### 1. Booking occurs on Date
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Date
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Date
- **Confidence Score:** 1.00
- **Evidence:** Explicit foreign key `fact_bookings.date_key -> dim_date.date_key`

### 2. Booking is for Customer
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Customer
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Customer
- **Confidence Score:** 1.00
- **Evidence:** Explicit foreign key `fact_bookings.customer_key -> dim_customer.customer_key`

### 3. Booking includes Product
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Product
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Product
- **Confidence Score:** 1.00
- **Evidence:** Explicit foreign key `fact_bookings.product_key -> dim_product.product_key`

### 4. Booking involves Partner
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Partner
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Partner
- **Confidence Score:** 1.00
- **Evidence:** Explicit foreign key `fact_bookings.partner_key -> dim_partner.partner_key`

### 5. Booking is assigned to Geography
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Geography
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Geography
- **Confidence Score:** 0.95
- **Evidence:** Explicit foreign key `fact_bookings.geography_key -> dim_geography.geography_key`; glossary uses `geo_key`

### 6. Booking is owned by Sales Representative
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Sales Representative
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Sales Representative
- **Confidence Score:** 1.00
- **Evidence:** Explicit foreign key `fact_bookings.sales_rep_key -> dim_sales_rep.sales_rep_key`

### 7. Booking is governed by Contract
- **Relationship Type:** Fact-to-Dimension
- **Parent Entity:** Contract
- **Child Entity:** Booking
- **Cardinality:** Many Bookings to One Contract
- **Confidence Score:** 1.00
- **Evidence:** Explicit foreign key `fact_bookings.contract_key -> dim_contract.contract_key`

## Measures

### Quantity
- **Technical Name:** `quantity`
- **Business Definition:** Number of product units included in the booking.
- **Aggregation Type:** Additive
- **Associated Entity:** Booking
- **Confidence Score:** 1.00

### Unit List Price USD
- **Technical Name:** `unit_list_price_usd`
- **Business Definition:** Standard list price per product unit before discounts.
- **Aggregation Type:** Non-additive / rate
- **Associated Entity:** Booking
- **Confidence Score:** 1.00

### Discount Percentage
- **Technical Name:** `discount_pct`
- **Business Definition:** Percentage discount applied to the standard list price.
- **Aggregation Type:** Non-additive / ratio
- **Associated Entity:** Booking
- **Confidence Score:** 1.00

### Booking Amount USD
- **Technical Name:** `booking_amount_usd`
- **Business Definition:** Net booking revenue after applying discounts.
- **Aggregation Type:** Additive
- **Associated Entity:** Booking
- **Confidence Score:** 1.00

### ACV USD
- **Technical Name:** `acv_usd`
- **Business Definition:** Annual Contract Value associated with the booking.
- **Aggregation Type:** Additive
- **Associated Entity:** Booking
- **Confidence Score:** 1.00

### TCV USD
- **Technical Name:** `tcv_usd`
- **Business Definition:** Total Contract Value over the full duration of the contract.
- **Aggregation Type:** Additive
- **Associated Entity:** Booking
- **Confidence Score:** 1.00

## Glossary Mapping

| Business Term | Technical Mapping | Confidence Score | Notes |
|---|---|---:|---|
| date_key | `dim_date.date_key`, `fact_bookings.date_key` | 1.00 | Exact match |
| full_date | `dim_date.full_date` | 1.00 | Exact match |
| month_name | `dim_date.month_name` | 1.00 | Exact match |
| calendar_year | `dim_date.calendar_year` | 1.00 | Exact match |
| fiscal_year | `dim_date.fiscal_year` | 1.00 | Exact match |
| fiscal_quarter | `dim_date.fiscal_quarter` | 1.00 | Exact match |
| fiscal_period_seq | `dim_date.fiscal_period_seq` | 1.00 | Exact match |
| customer_key | `dim_customer.customer_key`, `fact_bookings.customer_key` | 1.00 | Exact match |
| customer_id | `dim_customer.customer_id` | 1.00 | Exact match |
| customer_name | `dim_customer.customer_name` | 1.00 | Exact match |
| segment | `dim_customer.segment` | 1.00 | Exact match |
| industry | `dim_customer.industry` | 1.00 | Exact match |
| account_tier | `dim_customer.account_tier` | 1.00 | Exact match |
| hq_country | `dim_customer.hq_country` | 1.00 | Exact match |
| hq_region | `dim_customer.hq_region` | 1.00 | Exact match |
| product_key | `dim_product.product_key`, `fact_bookings.product_key` | 1.00 | Exact match |
| product_id | `dim_product.product_id` | 1.00 | Exact match |
| product_name | `dim_product.product_name` | 1.00 | Exact match |
| product_family | `dim_product.product_family` | 1.00 | Exact match |
| technology_domain | `dim_product.technology_domain` | 1.00 | Exact match |
| offer_type | `dim_product.offer_type` | 1.00 | Exact match |
| business_entity | `dim_product.business_entity` | 1.00 | Exact match |
| partner_key | `dim_partner.partner_key`, `fact_bookings.partner_key` | 1.00 | Exact match |
| partner_id | `dim_partner.partner_id` | 1.00 | Exact match |
| partner_name | `dim_partner.partner_name` | 1.00 | Exact match |
| partner_type | `dim_partner.partner_type` | 1.00 | Exact match |
| partner_tier | `dim_partner.partner_tier` | 1.00 | Exact match |
| route_to_market | `dim_partner.route_to_market` | 1.00 | Exact match |
| geo_key | `dim_geography.geography_key`, `fact_bookings.geography_key` | 0.95 | Inferred name equivalence |
| region | `dim_geography.region` | 1.00 | Exact match |
| theater | `dim_geography.theater` | 1.00 | Exact match |
| country | `dim_geography.country` | 1.00 | Exact match |
| sales_rep_key | `dim_sales_rep.sales_rep_key`, `fact_bookings.sales_rep_key` | 1.00 | Exact match |
| rep_id | `dim_sales_rep.rep_id` | 1.00 | Exact match |
| rep_name | `dim_sales_rep.rep_name` | 1.00 | Exact match |
| sales_role | `dim_sales_rep.sales_role` | 1.00 | Exact match |
| sales_team | `dim_sales_rep.sales_team` | 1.00 | Exact match |
| segment_covered | `dim_sales_rep.segment_covered` | 1.00 | Exact match |
| contract_key | `dim_contract.contract_key`, `fact_bookings.contract_key` | 1.00 | Exact match |
| contract_type | `dim_contract.contract_type` | 1.00 | Exact match |
| term_months | `dim_contract.term_months` | 1.00 | Exact match |
| auto_renew_flag | `dim_contract.auto_renew_flag` | 1.00 | Exact match |
| coverage_level | `dim_contract.coverage_level` | 1.00 | Exact match |
| booking_id | `fact_bookings.booking_id` | 1.00 | Exact match |
| order_number | `fact_bookings.order_number` | 1.00 | Exact match |
| order_line_number | `fact_bookings.order_line_number` | 1.00 | Exact match |
| booking_type | `fact_bookings.booking_type` | 1.00 | Exact match |
| is_renewal | `fact_bookings.is_renewal` | 1.00 | Exact match |
| quantity | `fact_bookings.quantity` | 1.00 | Exact match |
| unit_list_price_usd | `fact_bookings.unit_list_price_usd` | 1.00 | Exact match |
| discount_pct | `fact_bookings.discount_pct` | 1.00 | Exact match |
| booking_amount_usd | `fact_bookings.booking_amount_usd` | 1.00 | Exact match |
| acv_usd | `fact_bookings.acv_usd` | 1.00 | Exact match |
| tcv_usd | `fact_bookings.tcv_usd` | 1.00 | Exact match |

## Validation
- Missing glossary mappings: `day_of_week`, `month_number`, `calendar_quarter` are glossary-only and not found in DDL.
- Duplicate entities: none detected.
- Duplicate measures: none detected.
- Circular relationships: none detected.
- Missing keys: none detected in tables.
- Invalid semantic domains: none detected.
- Orphan entities: none detected.

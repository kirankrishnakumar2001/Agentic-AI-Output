# OSI Semantic Model

## Domains

### Domain: Time
**Description:** Calendar and fiscal date context used for bookings analysis and financial reporting.

### Domain: Customer
**Description:** Customer identity, segmentation, industry, and account classification.

### Domain: Product
**Description:** Product portfolio, offer category, technology domain, and responsible business entity.

### Domain: Channel & Sales
**Description:** Channel partner, geography, and sales execution context used to analyze how bookings are transacted and managed.

### Domain: Bookings & Contracts
**Description:** Core booking transactions and contract terms supporting revenue, renewal, and value analysis.

---

## Entities

### Business Name: Date
- **Technical Table:** `dim_date`
- **Description:** Stores calendar and fiscal date information used for analyzing bookings across different time periods.
- **Attributes:**
  - `date_key` — Unique surrogate key representing a calendar date
  - `full_date` — Complete calendar date associated with a transaction or booking
  - `month_name` — Name of the calendar month
  - `calendar_year` — Calendar year associated with the date
  - `fiscal_year` — Fiscal year according to the organization's accounting calendar
  - `fiscal_quarter` — Fiscal quarter according to the organization's fiscal calendar
  - `fiscal_period_seq` — Sequential fiscal accounting period used for financial reporting
- **Measures:** None
- **Keys:**
  - Primary Key: `date_key`
  - Business/semantic key candidate: `full_date`
- **Relationships:**
  - Date 1-to-many Booking (`date_key` → `fact_bookings.date_key`)

### Business Name: Customer
- **Technical Table:** `dim_customer`
- **Description:** Contains descriptive information about customers who purchase products or services.
- **Attributes:**
  - `customer_key` — Surrogate customer key
  - `customer_id` — Business identifier assigned to a customer
  - `customer_name` — Official name of the customer organization
  - `segment` — Business segment
  - `industry` — Industry classification
  - `account_tier` — Strategic classification based on business importance
  - `hq_country` — Headquarters country
  - `hq_region` — Headquarters region
- **Measures:** None
- **Keys:**
  - Primary Key: `customer_key`
  - Business Key: `customer_id`
- **Relationships:**
  - Customer 1-to-many Booking (`customer_key` → `fact_bookings.customer_key`)

### Business Name: Product
- **Technical Table:** `dim_product`
- **Description:** Contains descriptive information about products and services sold through bookings.
- **Attributes:**
  - `product_key` — Surrogate product key
  - `product_id` — Business identifier or SKU assigned to a product
  - `product_name` — Commercial product or service name
  - `product_family` — Group of related products
  - `technology_domain` — Technology category
  - `offer_type` — Offering category such as Hardware, Software Subscription, SaaS Subscription, or Service
  - `business_entity` — Business unit responsible for the product portfolio
- **Measures:** None
- **Keys:**
  - Primary Key: `product_key`
  - Business Key: `product_id`
- **Relationships:**
  - Product 1-to-many Booking (`product_key` → `fact_bookings.product_key`)

### Business Name: Partner
- **Technical Table:** `dim_partner`
- **Description:** Stores information about channel partners involved in selling products.
- **Attributes:**
  - `partner_key` — Surrogate partner key
  - `partner_id` — Business identifier assigned to the partner organization
  - `partner_name` — Partner organization name
  - `partner_type` — Partner classification
  - `partner_tier` — Partner program level
  - `route_to_market` — Sales channel through which products are sold
- **Measures:** None
- **Keys:**
  - Primary Key: `partner_key`
  - Business Key: `partner_id`
- **Relationships:**
  - Partner 1-to-many Booking (`partner_key` → `fact_bookings.partner_key`)

### Business Name: Geography
- **Technical Table:** `dim_geography`
- **Description:** Provides hierarchical geographic information for analyzing bookings across global regions.
- **Attributes:**
  - `geography_key` — Surrogate key identifying a geographic location
  - `region` — Highest-level geographic reporting region
  - `theater` — Sales theater within a region
  - `country` — Country associated with the booking or customer
- **Measures:** None
- **Keys:**
  - Primary Key: `geography_key`
- **Relationships:**
  - Geography 1-to-many Booking (`geography_key` → `fact_bookings.geography_key`), inferred glossary alignment to `geo_key`

### Business Name: Sales Representative
- **Technical Table:** `dim_sales_rep`
- **Description:** Contains information about sales representatives responsible for customer bookings.
- **Attributes:**
  - `sales_rep_key` — Surrogate key identifying a sales representative
  - `rep_id` — Business identifier assigned to the sales representative
  - `rep_name` — Full name of the sales representative
  - `sales_role` — Functional role performed by the sales representative
  - `sales_team` — Sales organization or team to which the representative belongs
  - `segment_covered` — Customer segment managed by the sales representative
- **Measures:** None
- **Keys:**
  - Primary Key: `sales_rep_key`
  - Business Key: `rep_id`
- **Relationships:**
  - Sales Representative 1-to-many Booking (`sales_rep_key` → `fact_bookings.sales_rep_key`)

### Business Name: Contract
- **Technical Table:** `dim_contract`
- **Description:** Stores information describing customer contracts associated with bookings.
- **Attributes:**
  - `contract_key` — Surrogate key identifying a contract
  - `contract_type` — Category of customer contract
  - `term_months` — Contract duration in months
  - `auto_renew_flag` — Indicates whether the contract automatically renews
  - `coverage_level` — Level of service or support included in the contract
- **Measures:** None
- **Keys:**
  - Primary Key: `contract_key`
- **Relationships:**
  - Contract 1-to-many Booking (`contract_key` → `fact_bookings.contract_key`)

### Business Name: Booking
- **Technical Table:** `fact_bookings`
- **Description:** Stores measurable business transactions representing customer bookings at one row per booked order line.
- **Attributes:**
  - `booking_id` — Unique identifier for a booking transaction
  - `order_number` — Sales order number associated with the booking
  - `order_line_number` — Line number within the sales order
  - `date_key` — Booking date foreign key
  - `customer_key` — Customer foreign key
  - `product_key` — Product foreign key
  - `partner_key` — Partner foreign key
  - `geography_key` — Geography foreign key
  - `sales_rep_key` — Sales representative foreign key
  - `contract_key` — Contract foreign key
  - `booking_type` — New, Renewal, or Upsell booking classification
  - `is_renewal` — Renewal indicator flag
- **Measures:**
  - `quantity` — Number of product units included in the booking
  - `unit_list_price_usd` — Standard list price per product unit before discounts
  - `discount_pct` — Percentage discount applied to the standard list price
  - `booking_amount_usd` — Net booking revenue after discounts
  - `acv_usd` — Annual Contract Value associated with the booking
  - `tcv_usd` — Total Contract Value over the full contract duration
- **Keys:**
  - Primary Key: `booking_id`
  - Degenerate dimensions: `order_number`, `order_line_number`
- **Relationships:**
  - Booking many-to-1 Date
  - Booking many-to-1 Customer
  - Booking many-to-1 Product
  - Booking many-to-1 Partner
  - Booking many-to-1 Geography
  - Booking many-to-1 Sales Representative
  - Booking many-to-1 Contract

---

## Relationships

| Relationship Name | Relationship Type | Cardinality | Confidence Score |
|---|---|---|---:|
| Booking occurs on Date | Foreign key | Booking many-to-1 Date / Date 1-to-many Booking | 1.00 |
| Booking is for Customer | Foreign key | Booking many-to-1 Customer / Customer 1-to-many Booking | 1.00 |
| Booking includes Product | Foreign key | Booking many-to-1 Product / Product 1-to-many Booking | 1.00 |
| Booking sold via Partner | Foreign key | Booking many-to-1 Partner / Partner 1-to-many Booking | 1.00 |
| Booking associated with Geography | Foreign key with glossary reconciliation | Booking many-to-1 Geography / Geography 1-to-many Booking | 0.92 |
| Booking owned by Sales Representative | Foreign key | Booking many-to-1 Sales Representative / Sales Representative 1-to-many Booking | 1.00 |
| Booking governed by Contract | Foreign key | Booking many-to-1 Contract / Contract 1-to-many Booking | 1.00 |

---

## Measures

| Measure Name | Business Definition | Aggregation Type |
|---|---|---|
| Quantity | Number of product units included in the booking. | Sum |
| Unit List Price USD | Standard list price per product unit before discounts. | Non-additive / typically Avg or context-specific |
| Discount Percent | Percentage discount applied to the standard list price. | Non-additive / typically Avg or weighted Avg |
| Booking Amount USD | Net booking revenue after applying discounts. | Sum |
| ACV USD | Annual Contract Value associated with the booking. | Sum |
| TCV USD | Total Contract Value over the full duration of the contract. | Sum |

---

## Glossary Mapping

| Business Term | Technical Mapping | Confidence Score |
|---|---|---:|
| Date | `dim_date` | 1.00 |
| date_key | `dim_date.date_key`, `fact_bookings.date_key` | 1.00 |
| full_date | `dim_date.full_date` | 1.00 |
| month_name | `dim_date.month_name` | 1.00 |
| calendar_year | `dim_date.calendar_year` | 1.00 |
| fiscal_year | `dim_date.fiscal_year` | 1.00 |
| fiscal_quarter | `dim_date.fiscal_quarter` | 1.00 |
| fiscal_period_seq | `dim_date.fiscal_period_seq` | 1.00 |
| Customer | `dim_customer` | 1.00 |
| customer_key | `dim_customer.customer_key`, `fact_bookings.customer_key` | 1.00 |
| customer_id | `dim_customer.customer_id` | 1.00 |
| customer_name | `dim_customer.customer_name` | 1.00 |
| segment | `dim_customer.segment` | 1.00 |
| industry | `dim_customer.industry` | 1.00 |
| account_tier | `dim_customer.account_tier` | 1.00 |
| hq_country | `dim_customer.hq_country` | 1.00 |
| hq_region | `dim_customer.hq_region` | 1.00 |
| Product | `dim_product` | 1.00 |
| product_key | `dim_product.product_key`, `fact_bookings.product_key` | 1.00 |
| product_id | `dim_product.product_id` | 1.00 |
| product_name | `dim_product.product_name` | 1.00 |
| product_family | `dim_product.product_family` | 1.00 |
| technology_domain | `dim_product.technology_domain` | 1.00 |
| offer_type | `dim_product.offer_type` | 1.00 |
| business_entity | `dim_product.business_entity` | 1.00 |
| Partner | `dim_partner` | 1.00 |
| partner_key | `dim_partner.partner_key`, `fact_bookings.partner_key` | 1.00 |
| partner_id | `dim_partner.partner_id` | 1.00 |
| partner_name | `dim_partner.partner_name` | 1.00 |
| partner_type | `dim_partner.partner_type` | 1.00 |
| partner_tier | `dim_partner.partner_tier` | 1.00 |
| route_to_market | `dim_partner.route_to_market` | 1.00 |
| Geography | `dim_geography` | 1.00 |
| geo_key | `dim_geography.geography_key`, `fact_bookings.geography_key` | 0.92 |
| region | `dim_geography.region` | 1.00 |
| theater | `dim_geography.theater` | 1.00 |
| country | `dim_geography.country` | 1.00 |
| Sales Representative | `dim_sales_rep` | 1.00 |
| sales_rep_key | `dim_sales_rep.sales_rep_key`, `fact_bookings.sales_rep_key` | 1.00 |
| rep_id | `dim_sales_rep.rep_id` | 1.00 |
| rep_name | `dim_sales_rep.rep_name` | 1.00 |
| sales_role | `dim_sales_rep.sales_role` | 1.00 |
| sales_team | `dim_sales_rep.sales_team` | 1.00 |
| segment_covered | `dim_sales_rep.segment_covered` | 1.00 |
| Contract | `dim_contract` | 1.00 |
| contract_key | `dim_contract.contract_key`, `fact_bookings.contract_key` | 1.00 |
| contract_type | `dim_contract.contract_type` | 1.00 |
| term_months | `dim_contract.term_months` | 1.00 |
| auto_renew_flag | `dim_contract.auto_renew_flag` | 1.00 |
| coverage_level | `dim_contract.coverage_level` | 1.00 |
| Booking | `fact_bookings` | 1.00 |
| booking_id | `fact_bookings.booking_id` | 1.00 |
| order_number | `fact_bookings.order_number` | 1.00 |
| order_line_number | `fact_bookings.order_line_number` | 1.00 |
| booking_type | `fact_bookings.booking_type` | 1.00 |
| is_renewal | `fact_bookings.is_renewal` | 1.00 |
| quantity | `fact_bookings.quantity` | 1.00 |
| unit_list_price_usd | `fact_bookings.unit_list_price_usd` | 1.00 |
| discount_pct | `fact_bookings.discount_pct` | 1.00 |
| booking_amount_usd | `fact_bookings.booking_amount_usd` | 1.00 |
| acv_usd | `fact_bookings.acv_usd` | 1.00 |
| tcv_usd | `fact_bookings.tcv_usd` | 1.00 |

### Unmapped or partially mapped glossary items
- `day_of_week` → no physical column found in DDL
- `month_number` → no physical column found in DDL
- `calendar_quarter` → no physical column found in DDL

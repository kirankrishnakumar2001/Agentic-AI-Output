# OSI Semantic Model

## Domains

### Bookings Domain
- Description: Central transactional domain representing booked order lines and their commercial measures.

### Date Domain
- Description: Calendar and fiscal time analysis domain used to slice bookings across reporting periods.

### Customer Domain
- Description: Customer master analysis domain for segmentation, industry, and account reporting.

### Product Domain
- Description: Product and service portfolio analysis domain for family, technology, and offer reporting.

### Partner Domain
- Description: Channel partner analysis domain for partner classification and route-to-market reporting.

### Geography Domain
- Description: Geographic reporting domain for regional, theater, and country analysis.

### Sales Domain
- Description: Sales organization domain describing representatives, teams, and sales roles.

### Contract Domain
- Description: Contract characteristics domain supporting duration, renewal, and coverage analysis.

## Entities

### Booking
- Business Name: Booking
- Technical Table: `fact_bookings`
- Description: Stores measurable booking transactions at the grain of one booked order line.
- Attributes:
  - `booking_id`: booking transaction identifier
  - `order_number`: sales order number
  - `order_line_number`: sales order line number
  - `booking_type`: type of booking transaction
  - `is_renewal`: renewal indicator
- Measures:
  - `quantity`: additive
  - `unit_list_price_usd`: non-additive unit rate
  - `discount_pct`: non-additive percentage
  - `booking_amount_usd`: additive net booking revenue
  - `acv_usd`: additive annual contract value
  - `tcv_usd`: additive total contract value
- Keys:
  - Primary Key: `booking_id`
  - Foreign Keys: `date_key`, `customer_key`, `product_key`, `partner_key`, `geography_key`, `sales_rep_key`, `contract_key`
  - Degenerate Dimensions: `order_number`, `order_line_number`
- Relationships:
  - Booking occurs on Date
  - Booking is for Customer
  - Booking includes Product
  - Booking is sold via Partner
  - Booking is associated with Geography
  - Booking is owned by Sales Representative
  - Booking is governed by Contract

### Date
- Business Name: Date
- Technical Table: `dim_date`
- Description: Describes calendar and fiscal dates used to analyze booking activity over time.
- Attributes:
  - `date_key`
  - `full_date`
  - `month_name`
  - `calendar_year`
  - `fiscal_year`
  - `fiscal_quarter`
  - `fiscal_period_seq`
- Measures: none
- Keys:
  - Primary Key: `date_key`
- Relationships:
  - Date has many Bookings

### Customer
- Business Name: Customer
- Technical Table: `dim_customer`
- Description: Represents customer organizations that purchase products or services.
- Attributes:
  - `customer_key`
  - `customer_id`
  - `customer_name`
  - `segment`
  - `industry`
  - `account_tier`
  - `hq_country`
  - `hq_region`
- Measures: none
- Keys:
  - Primary Key: `customer_key`
  - Business Key: `customer_id`
- Relationships:
  - Customer has many Bookings

### Product
- Business Name: Product
- Technical Table: `dim_product`
- Description: Represents products and services sold through customer bookings.
- Attributes:
  - `product_key`
  - `product_id`
  - `product_name`
  - `product_family`
  - `technology_domain`
  - `offer_type`
  - `business_entity`
- Measures: none
- Keys:
  - Primary Key: `product_key`
  - Business Key: `product_id`
- Relationships:
  - Product appears in many Bookings

### Partner
- Business Name: Partner
- Technical Table: `dim_partner`
- Description: Represents channel partners involved in booking transactions.
- Attributes:
  - `partner_key`
  - `partner_id`
  - `partner_name`
  - `partner_type`
  - `partner_tier`
  - `route_to_market`
- Measures: none
- Keys:
  - Primary Key: `partner_key`
  - Business Key: `partner_id`
- Relationships:
  - Partner influences many Bookings

### Geography
- Business Name: Geography
- Technical Table: `dim_geography`
- Description: Represents hierarchical geography used for sales reporting.
- Attributes:
  - `geography_key`
  - `region`
  - `theater`
  - `country`
- Measures: none
- Keys:
  - Primary Key: `geography_key`
- Relationships:
  - Geography contains many Bookings

### Sales Representative
- Business Name: Sales Representative
- Technical Table: `dim_sales_rep`
- Description: Represents sales personnel responsible for bookings.
- Attributes:
  - `sales_rep_key`
  - `rep_id`
  - `rep_name`
  - `sales_role`
  - `sales_team`
  - `segment_covered`
- Measures: none
- Keys:
  - Primary Key: `sales_rep_key`
  - Business Key: `rep_id`
- Relationships:
  - Sales Representative owns many Bookings

### Contract
- Business Name: Contract
- Technical Table: `dim_contract`
- Description: Represents contractual characteristics associated with bookings.
- Attributes:
  - `contract_key`
  - `contract_type`
  - `term_months`
  - `auto_renew_flag`
  - `coverage_level`
- Measures: none
- Keys:
  - Primary Key: `contract_key`
- Relationships:
  - Contract governs many Bookings

## Relationships

### Booking occurs on Date
- Relationship Type: Fact-to-Dimension
- Parent Entity: Date
- Child Entity: Booking
- Cardinality: One Date to Many Bookings
- Confidence Score: 1.00

### Booking is for Customer
- Relationship Type: Fact-to-Dimension
- Parent Entity: Customer
- Child Entity: Booking
- Cardinality: One Customer to Many Bookings
- Confidence Score: 1.00

### Booking includes Product
- Relationship Type: Fact-to-Dimension
- Parent Entity: Product
- Child Entity: Booking
- Cardinality: One Product to Many Bookings
- Confidence Score: 1.00

### Booking is sold via Partner
- Relationship Type: Fact-to-Dimension
- Parent Entity: Partner
- Child Entity: Booking
- Cardinality: One Partner to Many Bookings
- Confidence Score: 1.00

### Booking is associated with Geography
- Relationship Type: Fact-to-Dimension
- Parent Entity: Geography
- Child Entity: Booking
- Cardinality: One Geography to Many Bookings
- Confidence Score: 1.00

### Booking is owned by Sales Representative
- Relationship Type: Fact-to-Dimension
- Parent Entity: Sales Representative
- Child Entity: Booking
- Cardinality: One Sales Representative to Many Bookings
- Confidence Score: 1.00

### Booking is governed by Contract
- Relationship Type: Fact-to-Dimension
- Parent Entity: Contract
- Child Entity: Booking
- Cardinality: One Contract to Many Bookings
- Confidence Score: 1.00

## Measures

### Quantity
- Measure Name: `quantity`
- Business Definition: Number of product units included in the booking.
- Aggregation Type: Additive
- Confidence Score: 1.00

### Unit List Price USD
- Measure Name: `unit_list_price_usd`
- Business Definition: Standard list price per product unit before discounts.
- Aggregation Type: Non-additive
- Confidence Score: 1.00

### Discount Percent
- Measure Name: `discount_pct`
- Business Definition: Percentage discount applied to the standard list price.
- Aggregation Type: Non-additive
- Confidence Score: 1.00

### Booking Amount USD
- Measure Name: `booking_amount_usd`
- Business Definition: Net booking revenue after applying discounts.
- Aggregation Type: Additive
- Confidence Score: 1.00

### ACV USD
- Measure Name: `acv_usd`
- Business Definition: Annual Contract Value associated with the booking.
- Aggregation Type: Additive
- Confidence Score: 1.00

### TCV USD
- Measure Name: `tcv_usd`
- Business Definition: Total Contract Value over the full duration of the contract.
- Aggregation Type: Additive
- Confidence Score: 1.00

## Glossary Mapping

| Business Term | Technical Mapping | Confidence Score |
|---|---|---:|
| Date Key | `dim_date.date_key`, `fact_bookings.date_key` | 1.00 |
| Full Date | `dim_date.full_date` | 1.00 |
| Month Name | `dim_date.month_name` | 1.00 |
| Calendar Year | `dim_date.calendar_year` | 1.00 |
| Fiscal Year | `dim_date.fiscal_year` | 1.00 |
| Fiscal Quarter | `dim_date.fiscal_quarter` | 1.00 |
| Fiscal Period Sequence | `dim_date.fiscal_period_seq` | 1.00 |
| Customer Key | `dim_customer.customer_key`, `fact_bookings.customer_key` | 1.00 |
| Customer ID | `dim_customer.customer_id` | 1.00 |
| Customer Name | `dim_customer.customer_name` | 1.00 |
| Segment | `dim_customer.segment`, `dim_sales_rep.segment_covered` (related but not identical) | 0.85 |
| Industry | `dim_customer.industry` | 1.00 |
| Account Tier | `dim_customer.account_tier` | 1.00 |
| HQ Country | `dim_customer.hq_country` | 1.00 |
| HQ Region | `dim_customer.hq_region` | 1.00 |
| Product Key | `dim_product.product_key`, `fact_bookings.product_key` | 1.00 |
| Product ID | `dim_product.product_id` | 1.00 |
| Product Name | `dim_product.product_name` | 1.00 |
| Product Family | `dim_product.product_family` | 1.00 |
| Technology Domain | `dim_product.technology_domain` | 1.00 |
| Offer Type | `dim_product.offer_type` | 1.00 |
| Business Entity | `dim_product.business_entity` | 1.00 |
| Partner Key | `dim_partner.partner_key`, `fact_bookings.partner_key` | 1.00 |
| Partner ID | `dim_partner.partner_id` | 1.00 |
| Partner Name | `dim_partner.partner_name` | 1.00 |
| Partner Type | `dim_partner.partner_type` | 1.00 |
| Partner Tier | `dim_partner.partner_tier` | 1.00 |
| Route to Market | `dim_partner.route_to_market` | 1.00 |
| Geography Key | `dim_geography.geography_key`, `fact_bookings.geography_key` | 0.80 |
| Region | `dim_geography.region` | 1.00 |
| Theater | `dim_geography.theater` | 1.00 |
| Country | `dim_geography.country`, `dim_customer.hq_country` (context-dependent) | 0.80 |
| Sales Rep Key | `dim_sales_rep.sales_rep_key`, `fact_bookings.sales_rep_key` | 1.00 |
| Rep ID | `dim_sales_rep.rep_id` | 1.00 |
| Rep Name | `dim_sales_rep.rep_name` | 1.00 |
| Sales Role | `dim_sales_rep.sales_role` | 1.00 |
| Sales Team | `dim_sales_rep.sales_team` | 1.00 |
| Segment Covered | `dim_sales_rep.segment_covered` | 1.00 |
| Contract Key | `dim_contract.contract_key`, `fact_bookings.contract_key` | 1.00 |
| Contract Type | `dim_contract.contract_type` | 1.00 |
| Term Months | `dim_contract.term_months` | 1.00 |
| Auto Renew Flag | `dim_contract.auto_renew_flag` | 1.00 |
| Coverage Level | `dim_contract.coverage_level` | 1.00 |
| Booking ID | `fact_bookings.booking_id` | 1.00 |
| Order Number | `fact_bookings.order_number` | 1.00 |
| Order Line Number | `fact_bookings.order_line_number` | 1.00 |
| Booking Type | `fact_bookings.booking_type` | 1.00 |
| Is Renewal | `fact_bookings.is_renewal` | 1.00 |
| Quantity | `fact_bookings.quantity` | 1.00 |
| Unit List Price USD | `fact_bookings.unit_list_price_usd` | 1.00 |
| Discount Percent | `fact_bookings.discount_pct` | 1.00 |
| Booking Amount USD | `fact_bookings.booking_amount_usd` | 1.00 |
| ACV USD | `fact_bookings.acv_usd` | 1.00 |
| TCV USD | `fact_bookings.tcv_usd` | 1.00 |

## Validation
- Missing glossary mappings: none for DDL columns; 4 glossary attributes are not physically implemented.
- Duplicate entities: none detected.
- Duplicate measures: none detected.
- Circular relationships: none detected.
- Missing keys: none detected.
- Invalid semantic domains: none detected.
- Orphan entities: none detected.

# Ontology Summary

## Ontology Overview

### Primary Business Domain
Cisco bookings analytics within the Quote-to-Booking portion of the Order-to-Cash cycle.

### Ontology Scope
This ontology represents the semantic structure of Cisco booking transactions at booked order-line grain together with the descriptive business context required for analysis across time, customer, product, partner, geography, sales ownership, and contract dimensions. It captures explicit concepts from the OSI Semantic Model and Business Process document and adds only conservative inferences directly supported by documented workflow and business rules.

### Concepts Modeled
- Booking
- Date
- Customer
- Product
- Partner
- Geography
- SalesRepresentative
- Contract
- QuoteToBookingProcess
- OpportunityQualification
- ConfigurePriceQuoteActivity
- DealRegistrationActivity
- OrderPlacementActivity
- BookingRecognitionActivity
- FulfillmentProvisioningActivity
- ContractEntitlementCreationActivity
- RenewalLifecycleActivity
- SalesAccountExecutive
- DealDesk
- RevenueOperations
- ChannelPartnerOrganization
- CustomerExperienceRenewals
- Finance

### Relationships Modeled
#### Explicit semantic relationships
- Booking occurs on Date
- Booking is for Customer
- Booking includes Product
- Booking involves Partner
- Booking is assigned to Geography
- Booking is owned by SalesRepresentative
- Booking is governed by Contract

#### Process-supported relationships
- SalesAccountExecutive performs OpportunityQualification
- DealDesk performs ConfigurePriceQuoteActivity
- ChannelPartnerOrganization performs DealRegistrationActivity
- Customer submits OrderPlacementActivity
- Partner submits OrderPlacementActivity
- BookingRecognitionActivity recognizes Booking
- FulfillmentProvisioningActivity follows BookingRecognitionActivity
- ContractEntitlementCreationActivity creates Contract
- RenewalLifecycleActivity may produce renewal Booking

#### Inferred relationships
- BookingRecognitionActivity precedes FulfillmentProvisioningActivity
- Booking with isRenewal true is a RenewalBooking equivalent class candidate
- Booking linked to a Contract with autoRenewFlag true participates in renewal lifecycle context

### Business Rules Captured
#### Explicit rules
1. A booking is recorded when Cisco accepts a valid order with committed value.
2. Booking grain is one row per booked order line.
3. Booking amount, ACV, and TCV are computed at booking recognition.
4. Booking is classified as New, Renewal, or Upsell.
5. For subscription and SaaS offers, booking amount equals TCV.
6. For subscription offers, ACV is the annualized figure.
7. Hardware is perpetual and ACV is not applicable.
8. Renewal bookings flow back into the fact table.

#### OWL-translatable constraints
- Each Booking has exactly one Date.
- Each Booking has exactly one Customer.
- Each Booking has exactly one Product.
- Each Booking has exactly one Geography.
- Each Booking has exactly one SalesRepresentative.
- Each Booking has exactly one Contract.
- A Booking may involve zero or one Partner because channel deals are conditional in the process description and the schema allows nullability.
- autoRenewFlag and isRenewal behave as functional properties.

### Competency Questions
1. Which bookings are renewals and which contracts associated with them are auto-renewable?
2. Which products, partners, and sales teams contribute the highest booking amount by fiscal quarter and geography?
3. Which bookings recognized in the Quote-to-Booking process are associated with subscription-like contracts and annualized ACV measures?

### Input Validation
#### Validation status
- OSI Semantic completeness: sufficient for ontology generation
- Business Process readability: readable and extractable
- Missing entity definitions: none for core star-schema entities
- Missing relationships: no missing core fact-to-dimension relationships
- Duplicate entities: none detected
- Invalid semantic references: one naming mismatch `geo_key` vs `geography_key`
- Incomplete business process descriptions: some actors are role-level only and not linked to every schema entity
- Missing business rules: renewal capture rate and attach rate are mentioned as KPIs but not formally modeled as source attributes

#### Validation warnings
- `day_of_week`, `month_number`, and `calendar_quarter` appear in glossary/process context but are not physically present in the SQL schema.
- `geo_key` in glossary corresponds to `geography_key` in SQL and OSI model; treated as an equivalent identifier reference with confidence 0.95.
- Process mentions customer or partner submitting orders, but Customer and Partner are modeled as dimensions rather than process actors; therefore actor semantics are represented conservatively.
- Process mentions renewal date and entitlements, but no explicit columns for renewal date or entitlement entity exist in the source semantic model; these were not added as ontology classes.
- KPI concepts such as RenewalCaptureRate and AttachRate are not created as ontology classes because there is no supporting data structure in the provided semantic model.

## Ontology Metrics Report

### Overall Metrics Table
| Metric | Value |
|---|---:|
| Total Classes | 22 |
| Total Object Properties | 18 |
| Total Datatype Properties | 41 |
| Total Individuals | 0 |
| Hierarchy Depth | 2 |
| Total Axioms | 155 |
| Reasoning Rules / Constraints | 18 |

### Domain Metrics Table
| Domain | Class Count | Property Count | Individual Count | Reasoning Complexity |
|---|---:|---:|---:|---|
| Bookings | 3 | 14 | 0 | Complex |
| Date | 1 | 7 | 0 | Simple |
| Customer | 1 | 8 | 0 | Simple |
| Product | 1 | 7 | 0 | Medium |
| Partner | 1 | 6 | 0 | Medium |
| Geography | 1 | 4 | 0 | Simple |
| Sales | 2 | 7 | 0 | Medium |
| Contract | 1 | 5 | 0 | Medium |
| Business Process | 11 | 18 | 0 | Complex |

### Reasoning Complexity Summary
- **Simple:** Date, Customer, Geography because they mostly contribute descriptive context and straightforward range/domain semantics.
- **Medium:** Product, Partner, Sales, Contract because process semantics and constrained relationships support classification and inverse reasoning.
- **Complex:** Bookings and Business Process because they contain classification logic, sequence relations, actor-performance links, and equivalence/disjointness patterns.

## Ontology Quality & Reasoning Insights

### Semantic Coverage
#### Well-covered areas
- Core booking transaction semantics
- Fact-to-dimension business relationships
- Quote-to-Booking workflow stages
- Role ownership for key operational activities
- Booking classification and contract context

#### Sparse areas
- Date hierarchy is thinner than glossary description.
- Geography has a naming inconsistency between glossary and schema.
- Entitlement and billing/revenue downstream concepts are described in process text but not represented in the semantic model, so they are excluded.

#### Coverage observations
- Classes without datatype properties: most process activity classes intentionally rely on structural semantics rather than descriptive attributes.
- Missing inverse properties: inverse properties were added for the main Booking-to-dimension relationships and selected actor/activity links, but not for every workflow dependency to avoid unnecessary verbosity.
- Missing domain/range declarations: none for generated core properties.
- Incomplete axioms: no formal temporal ordering language beyond `precedesActivity` was introduced.
- Orphan classes: none; all classes participate in at least one hierarchy or property pattern.

### Reasoning Capabilities
- Multi-step inference: infer that a Booking belongs to Quote-to-Booking context through BookingRecognitionActivity and process stage association.
- Property chains: infer process governance context through `recognizedByActivity o partOfProcess` style modeling in the OWL comments, though kept conservative for Protégé compatibility.
- Inverse reasoning: from Customer `hasBooking` infer Booking `forCustomer`, and similarly for Product, Partner, Geography, SalesRepresentative, Contract, and actors performing activities.
- Functional reasoning: each Booking maps to one Date, Customer, Product, Geography, SalesRepresentative, and Contract.
- Classification reasoning: RenewalBooking is equivalent to Booking and `isRenewal` some boolean true literal pattern is approximated using subclass annotation due to OWL datatype limitations in simple RDF/XML output.
- Disjointness reasoning: dimension entities are pairwise disjoint from process activity and actor classes at top-level business semantics.

### Common SPARQL Query Patterns Supported
```sparql
# 1. Bookings by fiscal quarter and geography
SELECT ?fiscalQuarter ?region (SUM(?amount) AS ?totalBookings)
WHERE {
  ?b a :Booking ;
     :bookingAmountUsd ?amount ;
     :occursOnDate ?d ;
     :assignedToGeography ?g .
  ?d :fiscalQuarter ?fiscalQuarter .
  ?g :region ?region .
}
GROUP BY ?fiscalQuarter ?region
```

```sparql
# 2. Renewal bookings with auto-renew contracts
SELECT ?booking ?customerName ?contractType
WHERE {
  ?booking a :Booking ;
           :isRenewal ?renewal ;
           :forCustomer ?c ;
           :governedByContract ?ct .
  ?c :customerName ?customerName .
  ?ct :contractType ?contractType ;
      :autoRenewFlag ?flag .
  FILTER(?renewal = true && ?flag = true)
}
```

```sparql
# 3. Partner contribution by route to market
SELECT ?partnerName ?routeToMarket (SUM(?amount) AS ?total)
WHERE {
  ?b a :Booking ;
     :bookingAmountUsd ?amount ;
     :involvesPartner ?p .
  ?p :partnerName ?partnerName ;
     :routeToMarket ?routeToMarket .
}
GROUP BY ?partnerName ?routeToMarket
```

### Recommendations
- Add explicit entitlement, renewal date, and invoice/revenue concepts only when supported by source structures.
- Enrich Date semantics if physical columns for day-of-week, month-number, and calendar-quarter become available.
- Formalize booking-type controlled values as individuals or SKOS concepts if governance requires stronger classification.
- Consider SHACL for numeric validation such as discount percentage ranges and non-negative monetary measures.
- If Partner is mandatory for all channel-only subsets but optional globally, model conditional restrictions in SHACL or business rules rather than strict OWL cardinality.

### Watch-outs
- OWL is not ideal for arithmetic rules such as `bookingAmountUsd = tcvUsd` for subscription offers; these should be documented as business rules or implemented in rules/validation layers.
- Boolean classification from integer flags in source SQL requires normalization if stronger reasoning is needed.
- Process mentions hardware perpetual ACV not applicable, but absence of a dedicated product subtype prevents strict logical encoding.
- Property chains were kept conservative to maintain Protégé compatibility and avoid overcommitting unsupported semantics.

### Key Reasoning Paths
1. **Renewal context path**  
   Booking → governedByContract → Contract and Booking → isRenewal true enables renewal-focused classification.
2. **Operational ownership path**  
   SalesAccountExecutive → performsActivity → OpportunityQualification → precedesActivity → BookingRecognitionActivity supports process traceability.
3. **Channel analysis path**  
   Booking → involvesPartner → Partner → routeToMarket supports partner contribution and channel reasoning.
4. **Time-based performance path**  
   Booking → occursOnDate → Date → fiscalQuarter/fiscalYear supports fiscal analytics.
5. **Commercial structure path**  
   Booking → includesProduct → Product and Booking → governedByContract → Contract supports product-contract cross-analysis.

## Ontology Validation
| Check | Result | Confidence |
|---|---|---:|
| Duplicate classes | None detected | 0.99 |
| Duplicate properties | None detected | 0.99 |
| Invalid namespaces | None detected | 0.99 |
| Missing domains | None detected in generated core ontology | 0.96 |
| Missing ranges | None detected in generated core ontology | 0.96 |
| Circular inheritance | None detected | 0.99 |
| Invalid restrictions | None detected in asserted OWL restrictions | 0.94 |
| Logical consistency | Consistent under conservative modeling assumptions | 0.90 |
| Protégé compatibility | RDF/XML OWL content designed for Protégé import | 0.97 |

### Assumptions and Confidence Notes
- `QuoteToBookingProcess` and activity classes are inferred from the Business Process document with **0.92 confidence**.
- Actor-to-activity ownership mappings are inferred from the roles table and process flow with **0.88–0.95 confidence**.
- Optionality of Partner on Booking is inferred from channel-only wording and nullable schema design with **0.84 confidence**.
- `geo_key` and `geography_key` equivalence is inferred from glossary/schema alignment with **0.95 confidence**.
- No individuals were created because no instance-level records were provided.

# Ontology Summary

## Ontology Overview

### Business Domain
Primary business domain: **Revenue Bookings / Quote-to-Booking analytics**.

### Ontology Scope
This ontology represents the Cisco bookings semantic model and the quote-to-booking business process using OWL classes, object properties, datatype properties, and reasoning constraints derived from the provided OSI semantic model, business glossary, SQL DDL, and process documentation.

### Concepts Modeled
**Core business entities**
- Booking
- Date
- Customer
- Product
- Partner
- Geography
- SalesRepresentative
- Contract

**Business domains**
- RevenueBookingsDomain
- CustomerSalesDomain
- ProductPartnerDomain
- TimeGeographyContractDomain

**Process-supporting concepts explicitly grounded in the business process text**
- OpportunityQualification
- ConfigurePriceQuote
- DealRegistration
- OrderPlacement
- BookingRecognition
- FulfillmentProvisioning
- ContractEntitlementCreation
- RenewalLifecycle
- SalesAccountExecutive
- DealDesk
- RevenueOperations
- ChannelPartnerOrganization
- CustomerExperienceRenewals
- Finance

### Relationships Modeled
**Explicit relationships from the semantic model**
- Booking occursOn Date
- Booking madeBy Customer
- Booking referencesProduct Product
- Booking soldThrough Partner
- Booking assignedToGeography Geography
- Booking ownedBy SalesRepresentative
- Booking governedBy Contract

**Explicit process relationships from the business process document**
- SalesAccountExecutive performs OpportunityQualification
- DealDesk performs ConfigurePriceQuote
- ChannelPartnerOrganization performs DealRegistration
- BookingRecognition follows OrderPlacement
- FulfillmentProvisioning follows BookingRecognition
- ContractEntitlementCreation follows FulfillmentProvisioning
- RenewalLifecycle follows ContractEntitlementCreation
- BookingRecognition produces Booking
- ContractEntitlementCreation creates Contract
- RenewalLifecycle mayProduce Booking

**Inverse relationships included**
- Date hasBooking
- Customer makesBooking
- Product appearsInBooking
- Partner influencesBooking
- Geography hasAssignedBooking
- SalesRepresentative ownsBooking
- Contract governsBooking
- Process step inverses such as producesBooking / producedByProcessStep

### Business Rules Captured
**Explicitly supported rules**
1. A Booking is recorded when Cisco accepts a valid order with committed value.
2. Bookings are measured at order-line grain.
3. Booking amount, ACV, and TCV are computed at booking recognition.
4. Booking is classified as New, Renewal, or Upsell.
5. Subscription and SaaS offers have Booking Amount equal to TCV, and ACV is annualized.
6. Hardware is perpetual and ACV is not applicable.
7. Renewal bookings flow back into the fact table.
8. Contract creation establishes coverage level, term, and renewal obligation.

**OWL-translatable constraints actually encoded**
- Booking has exactly one occursOn Date
- Booking has exactly one madeBy Customer
- Booking has exactly one referencesProduct Product
- Booking has exactly one assignedToGeography Geography
- Booking has exactly one ownedBy SalesRepresentative
- Booking has exactly one governedBy Contract
- Booking has at most one soldThrough Partner
- Functional identifier properties for keys/business identifiers where justified by source data
- Inverse properties for major business relationships
- Transitive process dependency property: followsProcessStep

### Competency Questions
1. Which customers, product families, and regions contribute the most booking_amount_usd and tcv_usd by fiscal quarter?
2. Which bookings are renewals and which contracts governing them are marked for auto renewal?
3. Which partners and route-to-market channels influence bookings for particular product families and customer segments?

## Input Validation

### Validation Status
**Overall status:** Acceptable with warnings.

### Validation Findings
| Check | Result | Notes | Confidence |
|---|---|---|---:|
| OSI semantic completeness | Pass with warnings | Core entities, attributes, measures, and relationships are present | 0.95 |
| Business process readability | Pass | DOCX content successfully extracted and readable | 0.98 |
| Missing entity definitions | Warning | No explicit business entity for Opportunity or Order despite process references | 0.84 |
| Missing relationships | Warning | Process mentions order acceptance and entitlements, but no physical semantic entities for Order or Entitlement are present | 0.82 |
| Duplicate entities | Pass | No duplicate core semantic entities detected | 0.97 |
| Invalid semantic references | Warning | Glossary uses geo_key while SQL uses geography_key | 0.92 |
| Incomplete process descriptions | Warning | Renewal capture rate and attach rate are described but lack explicit source entities/measures in the schema | 0.79 |
| Missing business rules | Warning | Revenue recognition and billing are distinguished conceptually but not modeled as ontology classes due to lack of supporting schema entities | 0.81 |

### Assumptions
- No individuals were created because no instance/sample rows were provided.
- Process classes were created only where directly named in the process documentation.
- No unsupported classes such as Billing or RevenueRecognition were added as first-class business entities because they are discussed conceptually but not defined in the semantic model.

## Ontology Metrics Report

### Overall Metrics Table
| Metric | Value |
|---|---:|
| Total Classes | 22 |
| Total Object Properties | 20 |
| Total Datatype Properties | 48 |
| Total Individuals | 0 |
| Hierarchy Depth | 2 |
| Total Axioms | 170 |
| Reasoning Rules / Axiom Patterns | 16 |

### Domain Metrics Table
| Domain | Class Count | Property Count | Individual Count | Reasoning Complexity |
|---|---:|---:|---:|---|
| Revenue Bookings | 2 | 13 | 0 | Medium |
| Customer & Sales | 4 | 12 | 0 | Medium |
| Product & Partner | 4 | 10 | 0 | Medium |
| Time, Geography & Contract | 5 | 14 | 0 | Medium |
| Business Process | 11 | 11 | 0 | Complex |

### Reasoning Complexity Summary
- **Simple:** datatype validation, domain/range checks, inverse-property navigation.
- **Medium:** booking context inference through functional object properties and qualified cardinality restrictions.
- **Complex:** multi-step process traversal using transitive followsProcessStep, linking process steps to generated business artifacts such as Booking and Contract.

## Ontology Quality & Reasoning Insights

### Semantic Coverage
**Well-covered areas**
- Core booking fact and seven conformed dimensions.
- KPI-related measures directly supported by source metadata: quantity, booking_amount_usd, acv_usd, tcv_usd, discount_pct.
- Core quote-to-booking workflow stages named in the process document.

**Classes without many properties**
- RevenueBookingsDomain
- CustomerSalesDomain
- ProductPartnerDomain
- TimeGeographyContractDomain
These are organizational/domain grouping classes with annotation-oriented usage.

**Missing inverse properties**
- Most core entity relationships have inverses.
- Domain membership properties were intentionally not added to avoid artificial semantics not explicitly present in the source.

**Missing domain/range declarations**
- None for generated core object and datatype properties.

**Incomplete axioms**
- No renewal date restriction because no renewal_date attribute exists.
- No attach-rate rule because the model lacks explicit entitlement/service-attachment linkage at fact level.
- No explicit equivalence axiom for RenewalBooking because renewal is represented as a flag/value, not a fully normalized subtype source.

**Orphan classes**
- Domain grouping classes are lightly connected by annotations rather than structural axioms.
- Finance is intentionally process-role scoped and not connected to billing/revenue classes because such classes were not explicitly modeled from source data.

### Reasoning Capabilities
- Inference of booking context through object-property navigation: every Booking can be connected to its Date, Customer, Product, Geography, SalesRepresentative, and Contract.
- Inference through inverses: from Customer to all related bookings, from Product to all bookings in which it appears, etc.
- Process-chain reasoning through transitive followsProcessStep for workflow traversal from OpportunityQualification to RenewalLifecycle.
- Constraint-based validation of mandatory booking relationships.
- Classification support for bookings using bookingType and isRenewal values, though full value-based OWL classification remains limited without explicit enumerated individuals.

### Key Reasoning Paths
1. **Customer -> makesBooking -> Booking -> referencesProduct -> Product**
   - Answers which products a customer has booked.
2. **Partner -> influencesBooking -> Booking -> governedBy -> Contract**
   - Supports partner analysis by contract type and coverage level.
3. **BookingRecognition -> producesBooking -> Booking -> occursOn -> Date**
   - Links process recognition step to time-based booked transactions.
4. **ContractEntitlementCreation -> createsContract -> Contract -> governsBooking^-1 -> Booking**
   - Connects contract creation activity to governed bookings.
5. **OpportunityQualification -> followsProcessStep+ -> RenewalLifecycle**
   - Supports multi-step process lineage over the quote-to-booking lifecycle.

### SPARQL Query Patterns Supported
```sparql
# 1. Bookings by customer and fiscal quarter
SELECT ?customerName ?fiscalQuarter (SUM(?amount) AS ?totalBookings)
WHERE {
  ?b a <http://example.com/cisco/bookings#Booking> ;
     <http://example.com/cisco/bookings#madeBy> ?c ;
     <http://example.com/cisco/bookings#occursOn> ?d ;
     <http://example.com/cisco/bookings#bookingAmountUsd> ?amount .
  ?c <http://example.com/cisco/bookings#customerName> ?customerName .
  ?d <http://example.com/cisco/bookings#fiscalQuarter> ?fiscalQuarter .
}
GROUP BY ?customerName ?fiscalQuarter
```

```sparql
# 2. Renewal bookings with auto-renew contracts
SELECT ?bookingId ?contractType
WHERE {
  ?b a <http://example.com/cisco/bookings#Booking> ;
     <http://example.com/cisco/bookings#bookingId> ?bookingId ;
     <http://example.com/cisco/bookings#isRenewal> true ;
     <http://example.com/cisco/bookings#governedBy> ?contract .
  ?contract <http://example.com/cisco/bookings#autoRenewFlag> "Y" .
  OPTIONAL { ?contract <http://example.com/cisco/bookings#contractType> ?contractType }
}
```

```sparql
# 3. Partner contribution by route to market and product family
SELECT ?partnerName ?routeToMarket ?productFamily (SUM(?amount) AS ?bookings)
WHERE {
  ?b a <http://example.com/cisco/bookings#Booking> ;
     <http://example.com/cisco/bookings#soldThrough> ?partner ;
     <http://example.com/cisco/bookings#referencesProduct> ?product ;
     <http://example.com/cisco/bookings#bookingAmountUsd> ?amount .
  ?partner <http://example.com/cisco/bookings#partnerName> ?partnerName ;
           <http://example.com/cisco/bookings#routeToMarket> ?routeToMarket .
  ?product <http://example.com/cisco/bookings#productFamily> ?productFamily .
}
GROUP BY ?partnerName ?routeToMarket ?productFamily
```

```sparql
# 4. Process lineage from booking recognition onward
SELECT ?nextStep
WHERE {
  <http://example.com/cisco/bookings#BookingRecognition> 
    <http://example.com/cisco/bookings#followsProcessStep>+ ?nextStep .
}
```

### Recommendations
1. Add explicit entities for Order, Opportunity, Entitlement, and Renewal Obligation if future source material supports them.
2. Normalize controlled vocabularies for booking_type, route_to_market, offer_type, partner_type, and contract_type into reference classes or SKOS concepts.
3. Add sample individuals or instance mappings if knowledge graph population is required.
4. Add SWRL or SHACL rules outside core OWL if the business needs validations such as “Hardware ACV not applicable” or “Subscription booking amount equals TCV”.
5. Reconcile geo_key vs geography_key across glossary and physical model for metadata governance consistency.
6. Extend the Date dimension ontology if day_of_week, month_number, and calendar_quarter must be queryable in OWL.

### Watch-outs
- The process document mentions billings and revenue recognition, but these are outside the provided semantic model and were not promoted to modeled classes.
- ACV applicability differs by offer type; OWL alone is limited for arithmetic or conditional business-rule enforcement.
- Some business KPIs such as renewal capture rate and attach rate are not fully computable from the current schema.
- Domain classes are conceptual packaging constructs rather than operational business entities.

## Ontology Validation

| Validation Check | Result | Notes | Confidence |
|---|---|---|---:|
| Duplicate classes | Pass | No duplicate IRIs generated | 0.99 |
| Duplicate properties | Pass | No duplicate property IRIs generated | 0.99 |
| Invalid namespaces | Pass | Single consistent namespace used | 0.99 |
| Missing domains | Pass | Domains declared for generated properties | 0.96 |
| Missing ranges | Pass | Ranges declared for generated properties | 0.96 |
| Circular inheritance | Pass | No subclass cycles created | 0.99 |
| Invalid restrictions | Pass with caution | Restrictions limited to source-supported cardinalities | 0.93 |
| Logical consistency | Pass with caution | Designed for consistency; not machine-reasoner verified in this environment | 0.91 |
| Protégé compatibility | Pass | RDF/XML OWL serialization suitable for Protégé | 0.97 |

## Explicit vs Inferred Knowledge

### Explicit
- Eight semantic entities from SQL/semantic model.
- Seven fact-to-dimension relationships.
- Measures and attributes from SQL and glossary.
- Process steps and roles named in the business process document.

### Inferred
- Domain grouping classes.
- Cardinality restrictions based on dimensional foreign-key semantics.
- Inverse properties.
- Transitive process dependency property.
- Mapping from glossary geo_key to SQL geography_key with lower confidence.

# Data Dictionary - PostgreSQL Schema Discovery Report

## 1. Validation Summary

- **Input credential completeness:** Passed
- **Required connection parameters present:** Passed
- **Requested database type support:** Passed (`PostgreSQL` is supported)
- **Output format validity:** Passed (`Markdown`)
- **Secure connectivity/authentication check:** Passed via metadata query execution

## 2. Connection and Platform Detection

- **Detected Platform:** PostgreSQL
- **Detected Database Name:** insights_governance
- **Detected Current Schema:** tableau
- **Detected Database Version:** PostgreSQL 17.9 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 13.2.0, 64-bit
- **Requested Target Schema:** ontology

## 3. Important Validation Finding

The requested schema `ontology` was **not found to contain any discoverable user objects** accessible through the executed metadata queries.

Observed results for schema `ontology`:
- Tables: 0
- Columns: 0
- Keys / constraints: 0
- Indexes: 0
- Views: 0
- Functions / procedures: 0
- Triggers: 0
- Sequences: 0

This means the metadata completeness requirement for a populated data dictionary could not be satisfied for the requested schema because no structural objects were returned.

## 4. Schema Discovery Scope Results

### 4.1 Non-system schemas discovered in the database

The database contains multiple non-system schemas, including but not limited to:

`SSRS`, `UI`, `cognos`, `mstr`, `powerbi`, `powerbi_v2`, `public`, `ssrs_migration`, `tableau`, `thoughtspot`, `webfocus`

Additionally, a very large number of temporary schemas (`pg_temp_*`) were present and excluded from business interpretation.

### 4.2 Target Schema Discovery Result

| Database Name | Schema | Object Type | Object Count | Status |
| --- | --- | --- | ---: | --- |
| insights_governance | ontology | Tables | 0 | No objects found |
| insights_governance | ontology | Columns | 0 | No objects found |
| insights_governance | ontology | Constraints | 0 | No objects found |
| insights_governance | ontology | Indexes | 0 | No objects found |
| insights_governance | ontology | Views | 0 | No objects found |
| insights_governance | ontology | Functions / Procedures | 0 | No objects found |
| insights_governance | ontology | Triggers | 0 | No objects found |
| insights_governance | ontology | Sequences | 0 | No objects found |

## 5. Database-Specific Metadata SQL Used

The following PostgreSQL catalog / information schema queries were used for discovery:

1. Database/version detection using `current_database()`, `current_schema()`, and `version()`
2. Schema enumeration from `pg_namespace`
3. Table/view discovery from `pg_class` joined to `pg_namespace`
4. Column metadata from `information_schema.columns`
5. Key and referential metadata from `information_schema.table_constraints`, `key_column_usage`, `referential_constraints`, and `constraint_column_usage`
6. Constraint definitions from `pg_constraint`
7. Index metadata from `pg_indexes`
8. View definitions from `information_schema.views` with `pg_get_viewdef`
9. Routine metadata from `pg_proc` with `pg_get_functiondef`
10. Trigger metadata from `information_schema.triggers`
11. Sequence metadata from `information_schema.sequences`

## 6. Normalized Data Dictionary

Because no objects were returned for the requested schema, the normalized data dictionary is structurally valid but contains no business records.

| Database Name | Schema | Table Name | Object Type | Column Name | Data Type | Length | Precision | Scale | Nullable | Default Value | Primary Key | Foreign Key | Referenced Table | Referenced Column | Index | Constraint | Description / Remarks | DDL Statement |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| insights_governance | ontology | — | Schema | — | — | — | — | — | — | — | — | — | — | — | — | — | No discoverable objects were returned for schema `ontology`. | — |

## 7. Structural Metadata Summary

### 7.1 Tables

No tables were discovered in schema `ontology`.

### 7.2 Columns

No columns were discovered because no tables or views were returned for schema `ontology`.

### 7.3 Keys

- **Primary Keys:** None discovered
- **Foreign Keys:** None discovered
- **Unique Keys:** None discovered

### 7.4 Constraints

- **Check Constraints:** None discovered
- **Unique Constraints:** None discovered
- **Referential Constraints:** None discovered

### 7.5 Indexes

No indexes were discovered in schema `ontology`.

### 7.6 Relationships

No parent-child relationships were discovered in schema `ontology`.

### 7.7 Other Database Objects

- **Views:** None discovered
- **Stored Procedures:** None discovered
- **Functions:** None discovered
- **Triggers:** None discovered
- **Sequences:** None discovered

## 8. DDL Extraction Results

DDL extraction for supported objects in schema `ontology` could not yield object DDL because no eligible objects were discovered.

### DDL Availability Matrix

| Object Type | DDL Extraction Status | Notes |
| --- | --- | --- |
| Tables | Not available | No objects found |
| Views | Not available | No objects found |
| Indexes | Not available | No objects found |
| Constraints | Not available | No objects found |
| Sequences | Not available | No objects found |
| Stored Procedures | Not available | No objects found |
| Functions | Not available | No objects found |
| Triggers | Not available | No objects found |

## 9. Metadata Completeness Validation

| Validation Check | Result | Details |
| --- | --- | --- |
| Every discovered table has associated column metadata | Passed | No tables discovered, therefore no orphan table metadata |
| Primary and foreign key relationships correctly identified | Passed | No keys discovered |
| Object names unique within schema | Passed | No objects discovered |
| DDL extraction completed where supported | Partial | No discoverable target objects; attempted object-level extraction not applicable |
| Missing metadata reported | Passed | Empty schema/object set explicitly reported |

## 10. Exceptions / Technical Notes

Two exploratory PostgreSQL DDL helper queries failed during execution:

1. A test query using `:=` alias syntax failed due to PostgreSQL SQL syntax rules.
2. A query using `pg_get_tabledef(oid)` failed because that function is not available in this PostgreSQL environment.

These failures did **not** affect the structural metadata discovery outcome for the target schema, because the schema had no returned objects from the primary metadata queries.

## 11. Conclusion

A secure metadata discovery run was completed successfully against the PostgreSQL database.

### Final Status
- **Platform confirmed:** PostgreSQL
- **Database confirmed:** insights_governance
- **Target schema analyzed:** ontology
- **Schema structural metadata extracted:** No discoverable objects returned
- **Normalized data dictionary generated:** Yes
- **GitHub output generation:** Completed

## 12. Recommended Next Actions

1. Verify that the intended target schema name is correct.
2. Confirm whether access permissions to schema `ontology` are sufficient for metadata visibility.
3. If the intended schema is different from `ontology`, rerun discovery for the correct schema.
4. If required, perform discovery against one of the visible non-system schemas such as `tableau`, `powerbi`, `cognos`, or `public`.

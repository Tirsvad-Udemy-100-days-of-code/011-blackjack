# Entity Relationship Diagram (ERD) (ERD)

Cite in `CrossReference` only if the instance exists (`new-artifact.sh` checks this for you):

- **DCD** — **Required source**: classes/attributes each entity persists; data types must match

## Required sections (after Metadata / Version History)

1. **Purpose and Scope**.
2. **Diagram** — PlantUML entity diagram with PK/FK marked on every entity and
   cardinality (1:1, 1:N) on every relationship; N:M relationships resolved
   through explicit junction entities.
3. **Entity Table** — storage is a technical solution and first appears here and in the DCD,
   never in the Domain Model. Per entity: `Attribute | Type | PK/FK | Nullable |
   Source (DCD class.attribute)`. Types consistent with the DCD; consistent
   naming, no implementation-specific abbreviations.
4. **Relationship Table** — `Entity | Cardinality | Entity | FK | Rule`.
5. **Normalization Notes** — 3NF confirmed; any denormalization documented
   with its performance justification.

## Viewpoint

This view serves the developers and the database owner. It frames which data is stored, with keys, relationships and cardinality. The Purpose of an instance names the
concerns (`CN-NNN` of the Stakeholder Analysis) the Entity Relationship Diagram answers and the
stakeholders it serves; the Traceability Matrix lists it for each concern.

## Terminology

Entity and column names follow the IT terms from the dictionary (`DICT`), not the PO terms the Domain Model uses.

# @TITLE@

## Metadata
| Key | Value |
| --- | --- |
| ID | @ID@ |
| CrossReference | @CROSSREF@ |
| Language | @LANGUAGE@ |
| Domain | @DOMAIN@ |

## Version History
| Date | Status | Author | Reviewer | Change | Commit |
| --- | --- | --- | --- | --- | --- |
| @DATE@ | Proposed | @AUTHOR@ | <reviewer S-ID> | Initial version | pending |

---

## Purpose and Scope

<system boundary in words>

## Diagram

```plantuml
@startuml
left to right direction
actor "Name" as A1
rectangle "System Name" {
  usecase "Verb-phrase goal" as UC1
}
A1 --> UC1
@enduml
```

## Actor Table

| Actor | Stereotype | Stakeholder ID (SA) | Goals (use cases) |
| --- | --- | --- | --- |

## Use Case Table

| Use Case | Actor(s) | Goal | Source story | Priority (MoSCoW) | Reason |
| --- | --- | --- | --- | --- | --- |

## Role Matrix

Only when there is more than one primary actor. A suggestion for the Product
Owner to approve, not a decision; it says which use cases a role may perform,
not which data it may see.

| Use Case | <role 1> | <role 2> |
| --- | --- | --- |
| <use case> | performs | - |

## Relationships

| From | Relationship (`<<include>>` / `<<extend>>`) | To | Justification |
| --- | --- | --- | --- |

---

@LINKS@

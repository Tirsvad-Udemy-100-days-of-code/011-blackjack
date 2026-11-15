# Domain Model (DM)

Cite in `CrossReference` only if the instance exists (`new-artifact.sh` checks this for you):

- **UC** — Source of every concept (noun phrases) — cite each `UC-*` used
- **UCD** — Scope check: actors/goals the model must cover
- **SSD** — Forward: system operations that act on these concepts

## Required sections (after Metadata / Version History)

1. **Purpose and Scope** — which use cases the model covers.
2. **Diagram** — PlantUML class diagram (or linked source) showing
   **concepts, attributes and associations only — no operations**.
   Business language throughout ("Sale", not "SaleTable"/"SaleClass").
3. **Concept Table** — `Concept | Definition | Attributes | Source (use
   case noun phrase / glossary)`. Every concept traces to a noun in a use
   case or glossary. Every concept has at least one attribute. Attributes are
   simple domain data, not foreign-key-like references (model those as
   associations).
4. **Association Table** — `From | Association name (with reading
   direction) | To | Multiplicity (both ends)`. All multiplicities present.
5. **Generalizations** — only true "is-a" relationships, never inheritance
   for code reuse.

## Start from the project Domain Model

The project-level Domain Model, `docs/domain-model.md` in the root of `docs/`, is
the consolidated model. A Domain Model written for one use case (`docs/use-cases/uc-NNN/dm.md`) starts by reading it:

1. Reuse every concept, attribute and association that is already there, with
   its name and meaning; do not draw a second concept for the same thing.
2. Put only what is new or changed in the use case's model.
3. When the use case's model is done, add its new and changed concepts, attributes
   and associations to the project-level model, with a new `## Version History`
   row there that names the use case. If nothing changes, say so in the use
   case's task: "project DM unchanged: <reason>".

## Keep the model conceptual

The Domain Model has **conceptual classes only**: things in the Product Owner's
world, named in the Product Owner's words. Two rules, both Mandatory in
`QC-DM-001`:

1. **No technical solution names.** A technical solution is how the software
   does something, not what the business is made of. Examples that never appear
   in a Domain Model: "config from file", "environment variable", "settings
   file", "database", "table", "API", "endpoint", "JSON", "service",
   "controller", "repository", "DTO", "cache", "queue", "session", "log file".
   Two tests: would the Product Owner recognise the concept if there were no
   software, and would the concept stay if the technology were replaced? If a use
   case sentence names a technical solution ("the tool reads the setting from an
   environment variable"), take the business concept behind it ("Setting") and
   leave the mechanism out. The mechanism first appears in the Sequence Diagram
   (SD), the Design Class Diagram (DCD) and the Entity Relationship Diagram (ERD).
   The Operation Contract (OC) states the state change in Domain Model terms and
   does not name the mechanism either.
2. **No empty concept.** A concept with no attribute and no association says
   nothing. Give every concept at least one attribute and at least one
   association. If you cannot, it is not a concept: make it an attribute of
   another concept, the name of an association, or drop it.

Before you finish, read the Concept Table once with both rules: delete or
rename each row that fails.

## Viewpoint

This view serves the Product Owner and the domain experts. It frames what the business is made of: its concepts, their attributes and how they relate, in the Product Owner's terms. The Purpose of an instance names the
concerns (`CN-NNN` of the Stakeholder Analysis) the Domain Model answers and the
stakeholders it serves; the Traceability Matrix lists it for each concern.

## Terminology

Concept names are the PO terms recorded in the dictionary (`DICT`); add a row there for each new concept.

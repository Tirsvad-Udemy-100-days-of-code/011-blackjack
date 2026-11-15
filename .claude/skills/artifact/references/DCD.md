# Design Class Diagram (DCD) (DCD)

Cite in `CrossReference` only if the instance exists (`new-artifact.sh` checks this for you):

- **DM** — Concepts each design class refines — names must stay consistent
- **SD** — Messages that become method signatures
- **ERD** — Forward: persistence of the classes' attributes

## Required sections (after Metadata / Version History)

1. **Purpose and Scope**.
2. **Diagram** — PlantUML class diagram: visibility markers (`+` `-` `#`)
   on every member; association vs aggregation vs composition vs dependency
   used per true ownership/lifecycle; multiplicity and navigability on every
   association.
   **Layered architecture** (Clean, Hexagonal, Onion or similar): keep the dependency rules
   and the class and package structure in the diagram. Draw one package per layer, named
   as in the Sequence Diagram's layers, and put every class in exactly one package.
   Dependencies point inward only (delivery and infrastructure depend on application and
   domain, never the reverse). Inner layers own the interfaces (ports); outer layers
   implement them (draw the realization arrow from the outer class to the inner
   interface). Show the package dependencies as arrows between the packages.
   Colour each layer with its fixed pastel colour, the same in every SD and DCD of every
   project, so a layer is recognised by its colour:

   | Layer | Colour | Also named |
   | --- | --- | --- |
   | Delivery | `#FADADD` (pastel pink) | Presentation, UI, API, Interface Adapters |
   | Application | `#FFF3B0` (pastel yellow) | Use Cases, Services |
   | Domain | `#CDEFD3` (pastel green) | Entities, Core, Model |
   | Infrastructure | `#D4E4F7` (pastel blue) | Frameworks and Drivers, Persistence, External |

   In the Design Class Diagram: `package "Domain" #CDEFD3 { ... }` around the classes of the layer.
3. **Class Table** — technical classes (configuration reader, repository, service) are named
   here and have no Domain Model concept to refine; say so in the `Refines` cell.
   `Class | Refines (Domain Model concept) |
   Responsibility (one sentence) | Attributes | Operations`. SOLID applied;
   no god classes; names consistent with the Domain Model.
4. **Method Traceability** — `Method signature | Operation Contract / SD
   message`; every method traces to one.
5. **Pattern Annotations** — `Pattern | Classes | Rationale`, explicit.
6. **Dependency Check** — note confirming no circular class/package
   dependencies (or an explicit justification). With a layered architecture, also confirm
   that every dependency points inward and that no domain class depends on an outer layer.

## Viewpoint

This view serves the developers and the architect. It frames which classes, attributes, methods and associations the software has. The Purpose of an instance names the
concerns (`CN-NNN` of the Stakeholder Analysis) the Design Class Diagram answers and the
stakeholders it serves; the Traceability Matrix lists it for each concern.

## Terminology

Class and attribute names are the IT terms from the dictionary (`DICT`), not the PO terms the Domain Model uses.

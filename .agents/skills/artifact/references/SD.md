# Sequence Diagram (Design) (SD)

Cite in `CrossReference` only if the instance exists (`new-artifact.sh` checks this for you):

- **OC** — **Required source**: the contract whose postconditions each diagram realizes
- **DCD** — Forward: classes/methods these messages become

## Required sections (after Metadata / Version History)

One block per realized Operation Contract:

1. **Realizes** — the contract (`[OC-…]`) and its operation name. This is where technical
   solutions (a file, an environment variable, a service) first appear, as design objects;
   they are never added back to the Domain Model.
2. **Diagram** — PlantUML sequence diagram: sync (solid filled arrow),
   async (open arrow), returns (dashed); activations matching the call
   nesting; `create` / `destroy` shown for transient objects;
   `loop` / `alt` / `opt` fragments for conditional/repeated behavior.
   **Layered architecture** (Clean, Hexagonal, Onion or similar): show the layers in the
   diagram. Group the lifelines by layer (PlantUML `box "<Layer>"` per layer, ordered
   outermost to innermost, e.g. Delivery, Application, Domain, Infrastructure) so the
   crossing of each layer boundary is visible. A call that crosses a boundary goes
   through the inner layer's interface (port); the implementation lives in the outer
   layer. Do not draw an inner-layer lifeline calling an outer-layer concrete object.
   Colour each layer with its fixed pastel colour, the same in every SD and DCD of every
   project, so a layer is recognised by its colour:

   | Layer | Colour | Also named |
   | --- | --- | --- |
   | Delivery | `#FADADD` (pastel pink) | Presentation, UI, API, Interface Adapters |
   | Application | `#FFF3B0` (pastel yellow) | Use Cases, Services |
   | Domain | `#CDEFD3` (pastel green) | Entities, Core, Model |
   | Infrastructure | `#D4E4F7` (pastel blue) | Frameworks and Drivers, Persistence, External |

   In the Sequence Diagram: `box "Domain" #CDEFD3` around the lifelines of the layer.
3. **Pattern Annotations** — table `Pattern (GRASP/GoF) | Applied to |
   Rationale`. Patterns are labelled, never implicit.
4. **Postcondition Coverage** — `Postcondition | Satisfied by message`;
   every postcondition of the contract must be covered.
5. **Responsibility Check** — a short note showing no god-object receives
   all messages (low coupling, high cohesion).

## Viewpoint

This view serves the designers and developers. It frames how the objects of one use case collaborate to carry out a system operation. The Purpose of an instance names the
concerns (`CN-NNN` of the Stakeholder Analysis) the Sequence Diagram answers and the
stakeholders it serves; the Traceability Matrix lists it for each concern.

## Terminology

Use the IT terms from the dictionary (`DICT`), not the PO terms the Domain Model uses.

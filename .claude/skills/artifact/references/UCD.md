# Use Case Diagram (UCD)

Cite in `CrossReference` only if the instance exists (`new-artifact.sh` checks this for you):

- **SA** — Each actor traces to a stakeholder need — cite S-IDs
- **BC** — Scope / objectives the boundary reflects
- **US** — Forward: stories whose role must match an actor here
- **UC** — Forward: the use cases detailing each goal shown

## Required sections (after Metadata / Version History)

1. **Purpose and Scope** — the system boundary in words.
2. **Diagram** — actors with correct stereotypes (`<<Actor>>`,
   `<<System>>`), a labelled system boundary, `<<include>>` / `<<extend>>`
   used per UML 2.5.1 (not as generic "uses"). Embed PlantUML or link the
   diagram source; no UI or implementation detail.
   **Fewest crossings**: before settling the layout, try the alternatives (the order of the
   actors and of the use cases, actors on one side or on both sides, `left to right direction`
   or top to bottom, which use cases sit next to which actor) and keep the one whose
   association, `<<include>>` and `<<extend>>` arrows cross least. Render each candidate on the
   PlantUML server (`bash framework/scripts/render-diagrams.sh --out <dir> <file>`; the server is the
   `PlantUML server` setting of the project registry), look at the SVG and count the crossings; do not settle for the first
   layout that renders. State the number of
   crossings of the chosen layout under the diagram.
   **Crossings that remain**: give each actor whose arrows cross another arrow a strong colour
   of its own, and draw that actor and all of its arrows in it (PlantUML
   `actor "Clerk" as A #D62728` and `A -[#D62728]-> UC1`), so each line can be followed across
   the crossing. Use these strong colours in this order, one per crossing actor: `#D62728`
   (red), `#1F77B4` (blue), `#E67E22` (orange), `#8E44AD` (purple), `#00897B` (teal). Actors
   whose arrows cross nothing keep the default colour. Add the colour to the Actor Table's
   `Actor` cell only if it helps the reader.
3. **Actor Table** — `Actor | Stereotype | Stakeholder ID (SA) | Goals
   (use cases)`. No orphan actors: every actor appears in at least one use
   case.
4. **Use Case Table** — `Use Case | Actor(s) | Goal`, names as **verb
   phrases describing actor goals** ("Place Order"), not system operations
   ("Validate Input").
5. **Relationships** — every `<<include>>` / `<<extend>>` with a one-line
   justification.

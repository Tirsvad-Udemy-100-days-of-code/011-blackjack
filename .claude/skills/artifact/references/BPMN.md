# BPMN Process Model (BPMN)

Cite in `CrossReference` only if the instance exists (`new-artifact.sh` checks this for you):

- **BC** — The stated business goal/objective the process serves
- **SA** — Participants / lanes — cite S-IDs
- **UCD** — Forward link: actors and goals derived from this process

## Required sections (after Metadata / Version History)

1. **Purpose and Business Goal** — the Business Case objective realized
   (cite `[BC-001]`).
2. **Participants (Pools / Lanes)** — every participant, mapped to an `SA`
   stakeholder ID where one exists.
3. **Process Diagram** — valid BPMN 2.0. Message flows cross pool
   boundaries; sequence flows do not. Keep the diagram source next to the
   document (BPMN has no native markdown form) so it stays diffable.
4. **Element Table** — `Element | Type | Lane | Description` for every
   event, activity and gateway. Every gateway states its type (XOR/AND/OR)
   and its matching join.
5. **Path Coverage** — every path runs from a start event to a defined end
   event; no dead ends.

## Viewpoint

This view serves the process owners and the business stakeholders. It frames how the business process runs, who does each step and where work is handed over. The Purpose of an instance names the
concerns (`CN-NNN` of the Stakeholder Analysis) the BPMN Process Model answers and the
stakeholders it serves; the Traceability Matrix lists it for each concern.

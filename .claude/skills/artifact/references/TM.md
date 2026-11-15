# Traceability Matrix (TM)

One matrix per project: `docs/sqa/traceability-matrix.md`. It makes the
Business Case's cross-artifact traceability target measurable.

## Required sections (after Metadata / Version History)

- **Purpose**.
- **Traceability Table** — one row per artifact instance:
  `Artifact Instance | Type | Language | Domain | Upstream (Backward Link) |
  Downstream (Forward Link) | Last Reviewed (RC-ID)`. `Language` and `Domain`
  copy the artifact's Metadata rows so a reviewer can pick the right reviewer
  from the matrix; they are `-` for a technical type (OC, SD, DCD, ERD, ADR,
  TM, RC, QC, source code). `check-languages.sh --list` prints the same values.
  Add or update a row whenever an instance is created or reviewed.
- **Concern Coverage** — one row per concern of the Stakeholder Analysis:
  `Concern (CN-ID) | Views that answer it | Decisions (ADR) | Quality check
  (TP) | Status`. A concern with no view is `Gap` and is also listed in the
  Architecture Description, when there is one. Add the row when the concern is
  added.
- **Open Inconsistencies** — one row per inconsistency found between views:
  `# | Views | Inconsistency | Found in (RC-ID) | Status | Resolved in`.
  `Status` is `Open` or `Closed`; a closed row stays for the record. A
  No-Go `RC-*` that names an inconsistency between two views adds a row here.
- **Coverage Notes** — which types have no instance yet, and how to read
  `-`: in Upstream it means foundational; in Downstream it means nothing
  is built on it yet; in Last Reviewed it means no `RC-*` exists yet.

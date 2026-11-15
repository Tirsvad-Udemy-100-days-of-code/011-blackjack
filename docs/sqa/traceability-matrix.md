# Traceability Matrix: Blackjack

## Metadata
| Key | Value |
| --- | --- |
| ID | TM-001 |
| CrossReference | [BC-001], [PP-001] |

## Version History
| Date | Status | Author | Reviewer | Change | Commit |
| --- | --- | --- | --- | --- | --- |
| 2026-11-15 | Accepted | Jens Tirsvad Nielsen | AI as stand-in for S01 | Initial version; created with the review records of the baseline (autonomous, not independent) | [11c811b] |

---

## Purpose

Tracks backward and forward links between the artifact instances of Blackjack so that the traceability of the documents is measurable. A row is added or updated whenever an instance is created or reviewed.

## Traceability Table

| Artifact Instance | Type | Language | Domain | Upstream (Backward Link) | Downstream (Forward Link) | Last Reviewed (RC-ID) |
| --- | --- | --- | --- | --- | --- | --- |
| [BC-001] | Business Case | en | education | - | [SA-001], [PP-001], [MIL-001], [MIL-002], [MIL-003] | [RC-001] |
| [SA-001] | Stakeholder Analysis | en | education | [BC-001] | [PP-001] | [RC-002] |
| [PP-001] | Project Plan | en | education | [BC-001], [SA-001] | [MIL-001], [MIL-002], [MIL-003] | [RC-003] |
| [MIL-001] | Milestone | en | education | [BC-001], [PP-001] | [MIL-002] | [RC-004] |
| [MIL-002] | Milestone | en | education | [BC-001], [PP-001], [MIL-001] | [MIL-003] | [RC-005] |
| [MIL-003] | Milestone | en | education | [BC-001], [PP-001], [MIL-002] | - | [RC-006] |
| Wiki | Wiki | en, da, ms | education | [BC-001] | - | [RC-007] |

## Concern Coverage

| Concern (CN-ID) | Views that answer it | Decisions (ADR) | Quality check (TP) | Status |
| --- | --- | --- | --- | --- |
| CN-001 | [MIL-002] (criteria 1 to 4) | - | - | Covered |
| CN-002 | [MIL-001] (criterion 1), [MIL-003] (criterion 3) | - | - | Covered |
| CN-003 | [MIL-002] (criteria 1 and 5) | - | - | Covered |
| CN-004 | [MIL-002] (criteria 1 and 6) | - | - | Covered |
| CN-005 | [MIL-001] (criterion 4), [MIL-003] (criterion 4) | - | - | Covered |
| CN-006 | [MIL-001] (criteria 4 and 5), [MIL-003] (criterion 3) | - | - | Covered |
| CN-007 | [MIL-001] (criterion 1) | - | - | Covered |
| CN-008 | [MIL-003] (criteria 1 and 6) | - | - | Covered |

## Open Inconsistencies

| # | Views | Inconsistency | Found in (RC-ID) | Status | Resolved in |
| --- | --- | --- | --- | --- | --- |
| - | - | none found | - | Closed | - |

## Coverage Notes

No view, design or test-plan artifact exists yet (the scope excludes them), so the concerns are covered by the Go/No-Go criteria of the milestones. In Upstream, `-` means foundational; in Downstream, `-` means nothing is built on it yet.

---

[BC-001]: ../business-case.md
[SA-001]: ../stakeholder-analysis.md
[PP-001]: ../project-plan.md
[MIL-001]: ../milestones/mil-001-project-setup.md
[MIL-002]: ../milestones/mil-002-game-implementation.md
[MIL-003]: ../milestones/mil-003-framework-baseline.md
[RC-001]: ./reviews/rc-001-business-case.md
[RC-002]: ./reviews/rc-002-stakeholder-analysis.md
[RC-003]: ./reviews/rc-003-project-plan.md
[RC-004]: ./reviews/rc-004-milestone-001.md
[RC-005]: ./reviews/rc-005-milestone-002.md
[RC-006]: ./reviews/rc-006-milestone-003.md
[RC-007]: ./reviews/rc-007-wiki.md
[11c811b]: https://git.tirsystem.com/Tirsvad-Udemy-100-days-of-code/011-blackjack/commit/11c811bf713454e183db31fb0ebb0965614bd3bf

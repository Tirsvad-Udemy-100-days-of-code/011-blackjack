# Gateway 3 - Framework Baseline

## Metadata
| Key | Value |
| --- | --- |
| ID | MIL-003 |
| CrossReference | [BC-001], [PP-001] |
| Language | en |
| Domain | education |

## Version History
| Date | Status | Author | Reviewer | Change | Commit |
| --- | --- | --- | --- | --- | --- |
| 2026-11-15 | Accepted | Jens Tirsvad Nielsen | AI as stand-in for S01 | Initial version; accepted with the Go verdict of the review record (autonomous, not independent) | [11c811b] |

---

## Purpose

This gate decides whether Blackjack has the baseline of the framework: the adopted framework release, a wiki in English, Danish and Malay, the README in the framework template, complete licence metadata and reviewed documents, merged to the default branch with a green build.

## Deliverable

A merged pull request that holds: the updated Business Case, Stakeholder Analysis and Project Plan and this milestone with their review records; the traceability matrix; the wiki in English, Danish and Malay; the README in the framework template; `pyproject.toml` with complete licence metadata; and the framework release adopted by the project. The program and its tests do not change.

## Go / No-Go Criteria

| # | Criterion (objectively checkable) | Go | No-Go |
| --- | --- | --- | --- |
| 1 | BC-001, PP-001 and MIL-003 each have an `Accepted` Version History row and a review record with the verdict `Go` | All three | Any missing or `Go-with-conditions` |
| 2 | `python -m unittest discover -s tests` runs | Exit code 0 | Any failure |
| 3 | `docs/wiki/` has `_Sidebar.md` and the pages Home, Getting-Started and Project-Documents in en, da and ms | 9 pages and the sidebar | A page or language missing |
| 4 | `framework/scripts/check-readme.sh` and `framework/scripts/check-languages.sh` run | Exit code 0 | Any finding |
| 5 | `pyproject.toml` sets `license` and `license-files` | Both set | Either missing |
| 6 | All CI (continuous integration) checks of the pull request | Green | Red or pending |

## Dependencies

| Depends on | Reason |
| --- | --- |
| MIL-002 | Documents and baselines the finished game |

## Traceability

| Business Case objective / KPI / user story | Reference |
| --- | --- |
| Objectives 1 to 3 | Criterion 2 (the behaviour is unchanged and its tests pass) |
| Objectives 4 and 5 | Criteria 3, 4 and 5 (documentation, README and licence) |
| Objective 7 and success criterion 6 | Criteria 1 and 6 |

## Ownership

| Role | Stakeholder ID (SA) |
| --- | --- |
| Owner | S01 |
| Approving reviewer | S01 (AI as stand-in, autonomous mode, not independent) |

## Target Date

2026-11-22: one week after the start, as the Business Case constraint requires.

## Tasks

| # | Task | Summary | Needs its own Use Case/User Story? | Reference |
| --- | --- | --- | --- | --- |
| 1 | Adopt the framework release | Track the `framework/` submodule and `.gitmodules`, install the skills, hooks and `.editorconfig`, and track `.env.example` so the project follows the same rules as the other projects of the portfolio. | No | |
| 2 | Update and review the planning documents | Add this milestone to the Business Case and Project Plan, create the traceability matrix and review each changed document against its QC checklist, so the plan-first gate is met. | No | |
| 3 | Publish the wiki in English, Danish and Malay | Write the pages Home, Getting-Started and Project-Documents and the sidebar in `docs/wiki/`, so a visitor can learn what the exercise is and how to run it in their own language. | No | |
| 4 | Restore the README to the framework template | Bring the original text of the README into the template's sections and the document appendix, with the project links, so the README is complete and checkable. | No | |
| 5 | Add licence metadata to pyproject.toml | Set `license-files`, the authors and the project URLs next to the licence expression so package tools can read the licence. | No | |

---

[BC-001]: ../business-case.md
[PP-001]: ../project-plan.md
[11c811b]: https://git.tirsystem.com/Tirsvad-Udemy-100-days-of-code/011-blackjack/commit/11c811bf713454e183db31fb0ebb0965614bd3bf

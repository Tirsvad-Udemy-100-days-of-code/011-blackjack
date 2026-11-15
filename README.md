# 🃏 Blackjack

A console Blackjack game against a computer dealer, played with emoji cards.
It is the day 11 capstone of Udemy's *100 Days of Code: The Complete Python Pro
Bootcamp*, built with a plan-first process (milestones and issues in `docs/`).

## 📚 Table of Contents

- [Overview](#-overview)
- [Requirements](#-requirements)
- [Setup](#-setup)
- [Run](#-run)
- [Tests](#-tests)
- [License](#-license)
- [Links](#-links)
- [Appendix: Project documents](#-appendix-project-documents)

## 🧭 Overview

The game is the day 11 capstone of the course. You and the computer dealer each get two cards, shown as emoji. You choose to draw another card (`y`) or to pass (`n`); then the dealer draws while its score is below 17, and the program tells you who won. Afterwards you can play again.

House rules:

- The deck is unlimited; cards are not removed when drawn. No jokers.
- Jack, Queen and King count 10. An ace counts 11 or 1.
- A two-card hand of ace + 10 is a blackjack.
- The computer is the dealer and draws while its score is below 17.

## 📋 Requirements

- Python 3.13 or newer
- No runtime dependencies
- Optional: [Doxygen](https://www.doxygen.nl/) to build the source documentation
- A terminal with UTF-8 and emoji support (for example Windows Terminal)

## 🛠️ Setup

Create and activate a local virtual environment, upgrade pip, then install the
project in editable mode with the development tools.

Windows (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

Linux and macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

Leave the environment with `deactivate`.

## ▶️ Run

Start the game from the activated virtual environment:

```bash
python -m blackjack
```

## 🧪 Tests

```bash
python -m unittest discover -s tests
```

`pytest` also runs the same tests (`python -m pytest`).

Lint and type check:

```bash
ruff check .
ruff format --check .
mypy src
```

Build the source documentation from the Doxygen comments in `src/` (written to `docs/doxygen/`):

```bash
doxygen Doxyfile
```

## 📄 License

The code is licensed under the GNU Affero General Public License v3.0 or later (AGPL-3.0-or-later). See [LICENSE][license].

## 🔗 Links

- [Repository][repository]
- [Documentation][documentation]
- [Issue tracker][issues]

## 📎 Appendix: Project documents

The documents a newcomer needs, in reading order.

| Document | What it tells you |
| --- | --- |
| [Business Case][BC-001] | Why the project exists, its scope and success criteria |
| [Stakeholder Analysis][SA-001] | Who is involved; the stakeholder IDs used for owners and reviewers |
| [Project Plan][PP-001] | The gateways and their schedule |
| [Milestones][milestones] | One document per gateway: Go/No-Go criteria and tasks |
| [Artifact registry][registry] | Where each artifact type lives, the next version, the PO language and domain |
| [Traceability matrix][TM-001] | How the artifacts link and when each was last reviewed |

---

[repository]: https://git.tirsystem.com/Tirsvad-Udemy-100-days-of-code/011-blackjack
[documentation]: https://git.tirsystem.com/Tirsvad-Udemy-100-days-of-code/011-blackjack/wiki
[issues]: https://git.tirsystem.com/Tirsvad-Udemy-100-days-of-code/011-blackjack/issues
[license]: ./LICENSE
[BC-001]: ./docs/business-case.md
[SA-001]: ./docs/stakeholder-analysis.md
[PP-001]: ./docs/project-plan.md
[milestones]: ./docs/milestones/
[registry]: ./docs/artifact-registry.md
[TM-001]: ./docs/sqa/traceability-matrix.md

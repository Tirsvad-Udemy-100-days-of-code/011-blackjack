**English** · [Dansk](Getting-Started-da) · [Bahasa Melayu](Getting-Started-ms)

# Getting Started

## Requirements

Python 3.13 or newer. There are no runtime dependencies. Use a terminal with UTF-8 and emoji support (for example Windows Terminal).

## Setup

```bash
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .\.venv\Scripts\Activate.ps1
python -m pip install -e ".[dev]"
```

## Run

```bash
python -m blackjack
```

## Tests

```bash
python -m unittest discover -s tests
```

## Licence

The code is licensed under the GNU Affero General Public License v3.0 or later (AGPL-3.0-or-later). See the `LICENSE` file.

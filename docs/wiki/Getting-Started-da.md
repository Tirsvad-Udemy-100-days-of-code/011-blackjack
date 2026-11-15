[English](Getting-Started) · **Dansk** · [Bahasa Melayu](Getting-Started-ms)

# Kom godt i gang

## Krav

Python 3.13 eller nyere. Der er ingen afhængigheder ved kørsel. Brug en terminal med understøttelse af UTF-8 og emoji (for eksempel Windows Terminal).

## Opsætning

```bash
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .\.venv\Scripts\Activate.ps1
python -m pip install -e ".[dev]"
```

## Kør

```bash
python -m blackjack
```

## Tests

```bash
python -m unittest discover -s tests
```

## Licens

Koden er udgivet under licensen the GNU Affero General Public License v3.0 or later (AGPL-3.0-or-later). Se filen `LICENSE`.

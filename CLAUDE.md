# Backend: Reforma Tributária platform

API and calculation engine for accounting firms preparing clients for Brazil's tax reform (CBS/IBS, 2026–2033). First module: the 2027 Impact Study (PDF with the firm's branding).

Product spec and decisions live outside this repo, in the parent workspace: `../plano-de-implementacao.md`, `../prompt-software-completo.md`, `../prompt-estudo-de-impacto.md`.

## Working with the user

- Reply to the user in Portuguese. Code, identifiers and this file stay in English.
- One small step at a time: explain, wait for the user's decision, then proceed.
- The user runs every command. Give each command with a short explanation of what it does and the expected output. Do not run project commands yourself.
- Create or change one file at a time, only after discussing it. Do not scaffold ahead.

## Stack

- Python 3.12, managed with uv (`uv add`, `uv run`). Never use pip directly.
- FastAPI + pydantic, SQLAlchemy, Postgres. Package code lives in `src/backend/`.
- `uv.lock` is committed; `pyproject.toml` declares compatible ranges (`>=`), the lock pins exact versions.

## Code style

- Follow PEP 8. Use type hints everywhere; code must pass `mypy --strict` and `ruff`.
- The product is in Brazilian Portuguese: API messages and errors, PDF and Excel reports, emails, README and commit messages.
- The code is in English: variables, functions, classes, modules, files, database tables and columns, JSON field names. Brazilian tax acronyms stay as they are (`cbs`, `ibs`, `iss`, `cnpj`).
- Keep comments to a minimum. Write clear names instead; comment only the non-obvious *why* (a legal source, a workaround). No comments that restate the code. Docstrings only on public functions where the name is not enough.
- Small functions, explicit over clever, no premature abstractions.

## Non-negotiable rules

- Never invent tax rules. Every rate, threshold or formula comes from a cited legal source and lives as versioned data with an effective period, not as a constant in code.
- Calculations are done by the rules engine, never by AI.
- Money is `Decimal` with `ROUND_HALF_UP`. Never `float`. Money is serialized as a string.
- CPF never reaches the database, logs or reports. It may exist only inside the encrypted original upload, deleted at the end of the retention period.
- AI never receives CNPJ, CPF, names or company names.
- Logs never contain client data (CPF, CNPJ, names, invoice content, amounts).

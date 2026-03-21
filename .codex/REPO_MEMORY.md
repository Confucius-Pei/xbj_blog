# Repo Memory

## Purpose

This repository is a markdown-only family child credit ledger.
The source of truth is `README.md`.
Do not create an app, HTML, CSS, or JavaScript unless the user explicitly asks for it.

## Ledger Format

`README.md` contains:
- `Family Members` table with each child's current points
- `Current Summary` with total family child credits and last updated date
- one `Credit Records` section per child
- each child table uses columns: `Date`, `Category`, `Record`, `Points`, `Total`

## Current Family Data

- Xinbao (`Fred`): `6` points
- Debao (`Dave`): `0` points
- Total family child credits: `6`
- Last updated in ledger: `2026-03-21`

## Update Rules

- Recalculate the running `Total` column after any add, delete, or correction.
- Keep each child's `Current points` aligned with the last row in that child's table.
- Keep the `Family Members` table aligned with each child's current points.
- Keep `Total family child credits` equal to the sum of all children.
- Preserve the existing markdown structure and names.

## Repo Skill

Repo-level skill exists at `.codex/skills/add-record`.
Use it when the user describes point records in natural language and wants `README.md` updated.

## Raw Record Log

Save raw user descriptions for ledger changes in `data/raw-records.md` before turning them into structured records.
Each raw log row should include the date and keep the user's text close to verbatim.

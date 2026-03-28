# Repo Memory

## Purpose

This repository is a markdown-only family credit ledger.
The source of truth is `README.md`.
Do not create an app, HTML, CSS, or JavaScript unless the user explicitly asks for it.

## Ledger Format

`README.md` contains:
- `Family Members` table with each member's current points
- `Current Summary` with total family credits and last updated date
- one `Credit Records` section per member
- each member table uses columns: `Date`, `Category`, `Record`, `Points`, `Total`
- each member table shows the newest record first
- `Total` means the member's balance immediately after that record happened

## Current Family Data

- Xinbao (`Fred`): `10` points
- Debao (`Dave`): `-3` points
- Mama (`Mom`): `14` points
- Baba (`Dad`): `1` points
- Total family credits: `22`
- Last updated in ledger: `2026-03-28`

## Update Rules

- Insert new ledger rows at the top of the affected member table.
- Recalculate row `Total` values using chronological order, even though the table is displayed newest first.
- Keep each member's `Current points` aligned with the first row in that member's table.
- Keep the `Family Members` table aligned with each member's current points.
- Keep `Total family credits` equal to the sum of all members.
- Preserve the existing markdown structure and names.

## Repo Skill

Repo-level skill exists at `.codex/skills/add-record`.
Use it when the user describes point records in natural language and wants `README.md` updated.

## Raw Record Log

Save raw user descriptions for ledger changes in `data/raw-records.md` before turning them into structured records.
Each raw log row should include the date, keep the user's text close to verbatim, and place the newest row first.

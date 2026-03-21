---
name: add-record
description: Add or correct family credit ledger entries in this repository's README.md. Use when the user describes earned points, spending, deductions, deleted rows, or corrections in natural language and wants Codex to infer the required record fields, update the correct child section, and recalculate all totals.
---

# Add Record

## Workflow

1. Read `.codex/REPO_MEMORY.md` before making any ledger change. If it is missing, continue with `README.md`.
2. Read `README.md` before making any change.
3. Treat `README.md` as the ledger source of truth for the current records and totals.
4. Use `.codex/REPO_MEMORY.md` as supporting repo context, not as a replacement for the ledger.
5. Append the user's raw record description to `data/raw-records.md` before converting it into structured ledger rows. If the file is missing, create it.
6. Save the raw description as close to verbatim as possible. Do not normalize away details that may matter later.
7. Include the effective record date at the start of the raw description text for each appended row.
8. Discover the current ledger structure from the file itself, not memory alone.
9. Parse the user's description into one or more point events.
10. Infer every field needed for each row.
11. Update the affected child tables.
12. Recalculate all dependent totals.
13. Read `README.md` again after editing to verify consistency.

## Raw Record Log

Use `data/raw-records.md` as the raw event log.

For each user request that changes the ledger, append a markdown table row that includes:

- `Logged Date`: the date the request is being processed
- `Effective Date`: the event date used for the ledger rows
- `Raw Description`: the user's raw description, kept as close to verbatim as possible

Prefix the `Raw Description` cell with the effective date in brackets, for example `[2026-03-21] ...`.

Keep the raw log in append-only order unless the user explicitly asks to edit or delete old raw entries.

## Required Data For Each Record

Every credit record row in this repo needs these fields:

- `Date`: the event date in `YYYY-MM-DD`
- `Category`: a short label such as `Initial Record`, `Spending`, `Learning`, `Sports`, `Dinner`, `Reward`, or `Penalty`
- `Record`: a short human-readable description of what happened
- `Points`: a signed integer, positive for earning and negative for spending or deductions
- `Total`: the running point total for that child after applying that row

Every row also belongs to one child section. Identify the child by matching the name the user gives against the `Family Members` table and section headings in `README.md`.

## Inference Rules

- Use the explicit date if the user gives one. Otherwise use today's date from the conversation context.
- Use one row per distinct point change.
- If the user says `each`, create a separate row for each child.
- If the user lists multiple activities with separate point changes, create separate rows.
- If the user describes a starting balance instead of a newly earned event, use category `Initial Record` and record text `Initial recorded balance`.
- Reuse existing category names when they fit the event.
- Keep `Record` text short, clear, and consistent with the rest of the ledger.
- Do not invent missing point values.
- If the child, point value, or whether an event applies to one child or both is still ambiguous after reading `README.md`, ask a brief follow-up question.

## Update Rules

After adding, deleting, or correcting any record:

- Recompute the `Total` column from top to bottom inside each affected child table.
- Set `Current points` under each child heading to the last running total in that table.
- Set the `Current Points` value in the `Family Members` table to the same per-child total.
- Set `Total family child credits` in `Current Summary` to the sum of all child totals.
- Set `Last updated` to the most recent affected date.
- If `.codex/REPO_MEMORY.md` contains current family data that becomes stale after the edit, update it to match the new ledger state.

## Editing Rules

- Preserve the existing markdown layout and section order in `README.md`.
- Keep child names and English names exactly as already written in the file.
- Prefer editing only the rows and totals affected by the user's request.
- If rows were deleted manually, trust the remaining rows in the file and recalculate from them.

## Quick Checks

Before finishing:

- Verify every row's `Total` matches the previous total plus that row's `Points`.
- Verify each child's displayed current points matches the last row in that child's table.
- Verify the family summary equals the sum of all children.
- Verify the markdown table formatting is still readable.
- Verify `.codex/REPO_MEMORY.md`, if present, is still aligned with the latest ledger totals and repo rules.
- Verify `data/raw-records.md` contains the raw description for the change request you just processed.

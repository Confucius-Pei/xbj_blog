---
name: add-record
description: Add or correct family credit ledger entries in this repository's README.md. Use when the user describes earned points, spending, deductions, deleted rows, or corrections in natural language and wants Codex to infer the required record fields, update the correct child section, and recalculate all totals.
---

# Add Record

## Workflow

1. Read `README.md` before making any change.
2. Discover the current ledger structure from the file itself, not memory.
3. Parse the user's description into one or more point events.
4. Infer every field needed for each row.
5. Update the affected child tables.
6. Recalculate all dependent totals.
7. Read `README.md` again after editing to verify consistency.

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

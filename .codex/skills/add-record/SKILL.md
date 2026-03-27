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
5. Insert the user's raw record description into `data/raw-records.md` before converting it into structured ledger rows. If the file is missing, create it.
6. Save the raw description as close to verbatim as possible. Do not normalize away details that may matter later.
7. Include the effective record date at the start of the raw description text for each inserted row.
8. Place the newest raw row first, directly under the markdown table header.
9. Discover the current ledger structure from the file itself, not memory alone.
10. Parse the user's description into one or more point events.
11. Infer every field needed for each row.
12. Insert new ledger rows at the top of the affected child tables so the newest records appear first.
13. Recalculate all dependent totals.
14. Read `README.md` again after editing to verify consistency.

## Raw Record Log

Use `data/raw-records.md` as the raw event log.

## Data Files

This repo has multiple data files that must be kept in sync:

1. **README.md** - The human-readable ledger (source of truth for display)
2. **data/records.json** - JSON format with all records and member points
3. **data/records_last7days.csv** - CSV export of recent records (all records, sorted by date desc)
4. **data/raw-records.md** - Raw user input log

All files must be updated together when adding, deleting, or correcting records:
- Update `data/records.json` with structured records and current member points
- Update `data/records_last7days.csv` to reflect the same data as JSON
- Update `README.md` with human-readable format
- Update `data/raw-records.md` with raw user input

For each user request that changes the ledger, insert a markdown table row that includes:

- `Logged Date`: the date the request is being processed
- `Effective Date`: the event date used for the ledger rows
- `Raw Description`: the user's raw description, kept as close to verbatim as possible

Prefix the `Raw Description` cell with the effective date in brackets, for example `[2026-03-21] ...`.

Show the newest raw record first by inserting each new row immediately below the table header.
Edit or delete old raw entries only if the user explicitly asks.

## Required Data For Each Record

Every credit record row in this repo needs these fields:

- `Date`: the event date in `YYYY-MM-DD`
- `Category`: a short label such as `Initial Record`, `Spending`, `Learning`, `Sports`, `Dinner`, `Reward`, or `Penalty`
- `Record`: a short human-readable description of what happened
- `Points`: a signed integer, positive for earning and negative for spending or deductions
- `Total`: the child's balance immediately after that record happened

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

- Recompute each row's `Total` using chronological order, even though the table is displayed newest first.
- Set `Current points` under each child heading to the first row's total in that child's table.
- Set the `Current Points` value in the `Family Members` table to the same per-child total.
- Set `Total family child credits` in `Current Summary` to the sum of all child totals.
- Set `Last updated` to the most recent affected date.
- **Update data/records.json**: Update `members.{key}.currentPoints` for each affected member, add new records to the `records` array (newest first), and update `lastUpdated` to today's date.
- **Update data/records_last7days.csv**: Regenerate the CSV to match the JSON records, using columns: `日期,成员,英文名,类别,记录,积分,累计` (Date,Member,EnglishName,Category,Record,Points,Total). Newest records first.
- If `.codex/REPO_MEMORY.md` contains current family data that becomes stale after the edit, update it to match the new ledger state.

## Editing Rules

- Preserve the existing markdown layout and section order in `README.md`.
- Keep child names and English names exactly as already written in the file.
- Keep each child ledger table sorted with the newest record first.
- Prefer editing only the rows and totals affected by the user's request.
- If rows were deleted manually, trust the remaining rows in the file and recalculate from them.

## Quick Checks

Before finishing:

- Verify the first row in each child table matches that child's current points.
- Verify the family summary equals the sum of all children.
- Verify every row's `Total` is correct when the table is read back into chronological order.
- Verify the markdown table formatting is still readable.
- **Verify data/records.json**: Check `members.{key}.currentPoints` match README, `lastUpdated` is today's date, and new records are in the array.
- **Verify data/records_last7days.csv**: Check that CSV records match JSON records and are sorted by date descending.
- Verify `.codex/REPO_MEMORY.md`, if present, is still aligned with the latest ledger totals and repo rules.
- Verify `data/raw-records.md` contains the raw description for the change request you just processed, with the newest row first.

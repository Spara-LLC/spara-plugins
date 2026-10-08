---
name: review-pass
description: Read-only second pass after weekly posting or before close. Check uncategorized activity, missing payees, large transactions, and aging. Use when the user asks for a review or QA pass.
---

# Review pass

Select one client. Read `kb/profile.md`, `kb/review-checklist.md`, `kb/decisions-log.md`, and `kb/accounting-finance-map.md`. Confirm the QuickBooks company name matches the profile.

For a weekly review, use the last 7 days. For a close review, use the named month. Flag:

- Uncategorized or parent-account postings from `search_qbo_records` and `find_transactions`
- Missing payees
- Transactions at or above $2,500, unless the checklist names another threshold
- Aged receivables and payables from `get_qbo_report`
- Activity the finance map says should have come from Ramp or another sync

Show the evidence, the amount, and the source. Do not post corrections unless the user asks, and then use a proposal. Save a markdown review under `closes/YYYY-MM/` only if the user asks.

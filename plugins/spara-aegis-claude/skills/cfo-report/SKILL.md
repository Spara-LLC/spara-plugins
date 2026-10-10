---
name: cfo-report
description: Client-facing monthly results from the profit and loss and balance sheet. Use when the user asks for a CFO report or a monthly financial summary.
---

# CFO report

Select one client and one month. Read `kb/profile.md` for the accounting basis. Call `get_financial_report` for profit and loss and the balance sheet for that month.

Give a short client-facing summary: revenue, expenses, net income, cash, and the few changes that matter. State the company, period, basis, and retrieval time. Do not mention close checklists, uncategorized cleanup, or journal-entry proposals.

Show dollars with a currency symbol and two decimals. Save the summary under `closes/YYYY-MM/reports/` only if the user asks.

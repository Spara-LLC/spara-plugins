---
name: variance-analysis
description: Compare a month's profit and loss with the prior month, the trailing three-month average, and the trailing twelve-month average. Use when the user asks for a variance analysis.
---

# Variance analysis

Select one client and one month. Pull profit and loss for the current month, the prior month, and the earlier months needed for a three-month average and a twelve-month average. Use `get_financial_report` or `get_qbo_report` with `profit_and_loss`.

Flag an account when the dollar change is at least $500 or the percent change is at least 10 percent. An expense increase is a watch item. State the company, period, basis, and retrieval time.

Show the comparison in the chat. Save it under `closes/YYYY-MM/reports/` only if the user asks.

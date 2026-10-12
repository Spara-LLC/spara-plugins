---
name: close-month
description: Run month-end close for one client. Pull statements, post statement transactions, post knowledge-base journal entries, review the books, and write the close notes. Use when the user asks to close a month.
---

# Close month

Select one client and one `YYYY-MM` period. Read the knowledge base, including `kb/accounting-finance-map.md`. If the finance map is missing, stop and follow `build-finance-map` first.

Run the close in this order:

1. `statement-pull` for every bank and card account.
2. `statement-posting` for the statements the user wants recorded.
3. `journal-entry-posting` from that client's schedules and required documents.
4. Review uncategorized activity, profit and loss, the balance sheet, and aged receivables and payables.

Do not start journal entries until statement intake is complete. Do not say the month is closed until the user says the human sign-off is done.

Show a close checklist in the chat: statements, residuals, skipped sync lines, journal entries, and review findings. Save `closes/YYYY-MM/CLOSE-CHECKLIST.md` only after the user agrees.

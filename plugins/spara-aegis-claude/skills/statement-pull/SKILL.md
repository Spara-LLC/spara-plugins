---
name: statement-pull
description: Pull the month-end statement list for one client from the knowledge base and QuickBooks, then collect each bank and card statement. Use at the start of close, before posting statement transactions.
---

# Statement pull

Pull the accounts that need a statement. Do this before `statement-posting`.

## 1. Client and period

Select one client and one `YYYY-MM` period. Read `kb/banks-and-recon.md`, `kb/accounting-finance-map.md`, and `kb/profile.md`. Call `get_qbo_company_details`. The company name must match the profile. If it does not, stop.

## 2. Pull the account list

Build one row for every reconcilable bank and card account in the knowledge base. A bank or card account needs a statement unless that file explicitly says it does not. Do not skip an account because a bank feed exists.

For each account, call `search_qbo_records` on `account` and record the QuickBooks account name, id, and book balance. Show the checklist and wait:

| Account | Institution | QuickBooks account | Book balance | Statement | Ending balance | Status |

Status is `needed`, `received`, or `n/a`. `n/a` requires the user to give a reason, such as a closed account or no activity. Do not mark `n/a` on your own.

## 3. Take in the statements

Ask the user to attach the PDF, CSV, or image for each `needed` account, including the ending balance. Read the statement date range and ending balance from the file. Match it to the confirmed account. A file name is not enough.

Compare the statement ending balance with the QuickBooks book balance. List every difference. Do not change QuickBooks to force the balances to match.

This plugin cannot log in to a bank and cannot store the statement file in the client repository. Keep the attached file in the chat. After the user agrees, save `closes/YYYY-MM/STATEMENT-INTAKE.md` with `save_client_files`. Include the account, status, ending balance, book balance, and any difference.

## 4. Hand off

When every account is `received` or `n/a`, stop unless the user asks to record the transactions. Recording follows `statement-posting`.

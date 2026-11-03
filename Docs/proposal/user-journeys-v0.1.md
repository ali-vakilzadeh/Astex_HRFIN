# Astex HR and Finance Platform: User Journeys

Task 3 companion to the screen list and mockup. Version 0.1, 2 November 2026.

Journeys are written from the user's side and name the screens they pass through (IDs from `docs/03-screen-list.md`). Part A journeys are written in full. Part B and C journeys are shorter, because each module follows the same pattern: something triggers it, the user reviews and decides on one or two screens, and the result flows into the core records.

All personas and names are invented for this document.

## Personas

| Persona | Role | Works with | Needs most |
| --- | --- | --- | --- |
| **Anna Kovács** | Finance accountant (one of 2 or 3) | Invoice register, review, cases, suppliers | To confirm a clean invoice in about a minute and never retype data |
| **Bence Szabó** | Finance manager and approver | Dashboard, approvals, payment batches | To see what is overdue and what needs his decision, without asking anyone |
| **Katalin Nagy** | HR administrator | People, invitations, documents, contracts | To know which permits and documents are about to expire, before anyone else tells her |
| **Ravi Kumar** | Newly hired worker abroad, phone only | The onboarding form | A form in his language that does not lose his progress |
| **Zoltán Varga** | Operations director, platform owner | Users, roles, settings, module catalog | Control without technical work, and a say in what is switched on |
| **Eszter Fekete** | External bookkeeper | Reports, accounting export | Clean, coded data in the format her software accepts |
| **Ravi, later** | Employee | Self-service portal (Part C) | To see his own documents and expiry dates |

## Part A journeys

### Platform and administration

#### J-P1. Sign in and start the day
**Who:** every user. **Module:** A1.

| Step | The user | Screen | The system |
| --- | --- | --- | --- |
| 1 | Opens the platform address and enters email and password | S-A00 | Checks credentials |
| 2 | Enters the code from the authenticator | S-A00 | Starts a session; shows language chosen last time |
| 3 | Sees tasks due today, items waiting, recent records | S-A01 | Lists only what this role may see |
| 4 | Opens the task inbox and picks the oldest overdue task | S-A02 | Opens the related record in context |
| 5 | Finishes the task | the record's screen | Closes the task and records who did what |

**What can go wrong:** five wrong codes lock the account for a period and notify the owner; a session left idle asks for the second factor again before showing anything.

#### J-P2. Manage users, roles and settings
**Who:** Zoltán. **Module:** A1.

1. Opens Users (S-A50) and invites Anna with the role "Finance accountant".
2. Opens Roles and permissions (S-A51), sees the permission tree per module, and switches off unmasking of tax and bank identifiers for the accountant role.
3. Uses "test as role" to confirm what Anna will see.
4. Opens Settings (S-A54) and sets reminder windows, invitation validity and the sending domain; checks Integrations and credentials (S-A59) for connection status; edits email wording in Notification templates (S-A55).
5. Opens Retention rules (S-A56) and confirms the category periods his adviser gave him.

**Outcome:** a new user can work on the first day with exactly the access intended.

#### J-P3. Investigate a failed job or answer an audit question
**Who:** Zoltán, or Anna when a task points her there. **Module:** A1.

1. A task "OCR failed for 3 files" appears in the inbox (S-A02) and points to the Job monitor (S-A53).
2. He sees the error class (provider timeout, retryable) and the owner, and retries the jobs.
3. A colleague asks who changed a supplier's bank account. He opens the Audit log (S-A52), filters by supplier and action, and sees actor, time, source and before and after values.

**Outcome:** failures are never silent, and "who changed this" is answered in under a minute.

#### J-P4. Migrate the Excel registers
**Who:** Zoltán with Anna. **Module:** A1, A2, A3.

1. Opens the Data migration wizard (S-A57) and chooses the invoice register spreadsheet.
2. Maps columns (invoice number, supplier, dates, amounts) and sees validation messages for bad rows.
3. Runs a trial load and reads the report of counts and totals per register.
4. Fixes the source sheet and runs a second trial; Anna signs off that totals reconcile.
5. At cutover the final load runs and the wizard records what was imported and by whom.

**Outcome:** Excel stops being the record; the platform's totals match Astex's own.

#### J-P5. Choose which modules are on
**Who:** Zoltán, with Bence and Katalin. **Module:** A1.

1. Opens the Module catalog (S-A58).
2. Sees every optional module with Part, tier, what it needs and what Astex must supply.
3. Ticks "Approval workflows"; the catalog also ticks what it depends on and explains why.
4. Sees a summary of selected modules and the list of inputs Astex has promised.
5. Saves; the menu shows the new screens the next time users sign in.

**Outcome:** the decision about scope is made in the product, with the dependencies visible.

#### J-P6. Find anything
**Who:** any user. **Module:** A1, A2.

1. Types "forklift" in the search box.
2. Search results (S-A03) show invoices whose Hungarian text says *targonca*, tagged "matched in translation".
3. Filters to the last six months and a supplier; opens the invoice.

**Outcome:** goods can be found regardless of the language of the original invoice.

### Finance core

#### J-F1. Register an invoice from scan to register
**Who:** Anna. **Module:** A2 (B1 adds email intake).

| Step | Anna | Screen | The system |
| --- | --- | --- | --- |
| 1 | Drops three scanned invoices on the upload area | S-A12 | Checks type and size, warns if a file was loaded before, queues OCR |
| 2 | Opens the New/Review tab | S-A11 | Shows each invoice with status "OCR waiting", then "Review" |
| 3 | Opens the first invoice | S-A13 | Original on the left; extracted fields on the right with confidence |
| 4 | Fixes the performance date and the VAT amount | S-A13 | Validates totals as she types; blocks confirmation while net + VAT does not equal gross |
| 5 | Reads the English translation of the item lines | S-A13 | Shows source and translation side by side |
| 6 | Presses Confirm | S-A13 | Stores the values and the translation together; tries matching to a purchase case |
| 7 | Moves to the next invoice with the keyboard | S-A13 | Loads the next in the queue |

**Outcome:** the invoice is in the Open view; the translation is searchable; the match result is on the invoice timeline.

**What can go wrong:** an unreadable scan is rejected with a plain reason and a request for a better file; an unknown supplier opens a quick "create supplier" panel (S-A19); a low-confidence field is highlighted and cannot be confirmed without a look.

**Success measure:** a clean invoice is confirmed in about 60 seconds.

#### J-F2. Build a purchase case and resolve matches
**Who:** Anna. **Module:** A2.

1. Opens the Matching review queue (S-A17), which lists a delivery note the system could not link with confidence.
2. Sees ranked candidate cases with, per candidate, which signals agree (supplier, weight, dates) and which conflict.
3. Picks the right case, or chooses "new case" if none fits.
4. Opens the case (S-A16) and sees document roles: invoice present, delivery note present, EKÁER declaration missing.
5. Uploads the missing document; the case moves from Incomplete towards Ready.

**Outcome:** the dossier is complete and every link has a stored explanation. Nothing is linked on a single weak signal.

#### J-F3. Track what is open, overdue and paid
**Who:** Anna, Bence. **Module:** A2.

1. Anna opens the Open view in the register (S-A11) and sorts by due date.
2. Opens Payment status (S-A21) for the due list; overdue invoices are highlighted.
3. After Astex pays, she marks the invoice paid with date and reference. For a partial payment she adds a note.
4. Bence checks the same list filtered to overdue.
5. The invoice timeline (S-A14) shows who marked it paid and when.

**Outcome:** there is no spreadsheet of payments; payment state is separate from completeness.

#### J-F4. Maintain suppliers and the goods dictionary
**Who:** Anna. **Module:** A2 (C13 adds a tax-number check).

1. In Suppliers (S-A18), searches for "Kovacs Ltd" and finds a supplier already known as "Kovács Kft." by alias.
2. In Supplier detail (S-A19), adds the new spelling as an alias, so the next invoice resolves automatically.
3. In the Goods dictionary (S-A20), adds *targonca = forklift* and sees a preview of what a search now finds.

**Outcome:** two spellings are one supplier; one search finds goods in any language. With C13 enabled, the tax-number check result appears as a panel in step 2.

#### J-F5. Manager reviews the finance dashboard
**Who:** Bence. **Module:** A2.

1. Opens the Finance dashboard (S-A10) and reads monthly spend, paid on time and incomplete cases.
2. Clicks "overdue" and lands in the register (S-A11) already filtered.
3. Exports the filtered list as a spreadsheet for a meeting.

**Outcome:** the manager answers "what is overdue" without asking the accountants.

### HR core

#### J-H1. Invite a new worker
**Who:** Katalin. **Module:** A3.

1. In People (S-A31) selects "New invitation".
2. In New invitation (S-A34) enters name, email, language and onboarding type.
3. Sees which fields and documents this worker will be asked for, from the requirement matrix.
4. Sends the invitation; the privacy notice version that Ravi will accept is shown.
5. Invitations (S-A33) shows the state: Draft, Sent, Opened, Submitted, Approved. She can resend (new token) or revoke.

**Outcome:** a worker has a secure, expiring link and Katalin sees where each invitation stands.

#### J-H2. Worker completes onboarding on a phone
**Who:** Ravi. **Module:** A3.

| Step | Ravi | Screen | The system |
| --- | --- | --- | --- |
| 1 | Opens the link in the email | S-W01 | Shows company, what he needs, and the privacy notice; he accepts it |
| 2 | Enters the code sent to his email | S-W02 | Verifies; logs acceptance and notice version |
| 3 | Fills name, birth details, nationality, passport | S-W03 | Validates as he types; saves each step |
| 4 | Fills contact and address | S-W04 | Saves; he can close the phone and return |
| 5 | Answers permit questions | S-W05 | Shows only sections that apply to his nationality and route |
| 6 | Photographs passport and residence permit | S-W06 | Checks size and legibility; shows the checklist |
| 7 | Reviews the summary and submits | S-W07 | Flags anything missing; on submit, locks key fields |
| 8 | Sees confirmation | S-W08 | Tells him what happens next |

**What can go wrong:** an expired or revoked link shows a clear message with no personal data and a way to ask for a new link (S-W08). A photo that is too large or unreadable is rejected with the reason.

**Outcome:** a complete submission arrives for review without email attachments.

#### J-H3. HR reviews and approves a submission
**Who:** Katalin. **Module:** A3.

1. A task "Submission from Ravi Kumar" appears in her inbox (S-A02).
2. In Submission review (S-A35) she sees answers by section and a list of required vs provided items.
3. For each document she opens Document review (S-A37): the scan on the left, extracted dates on the right with confidence. She confirms or corrects.
4. She opens the person's profile (S-A32) to check identity and permit details.
5. She approves. The status moves to Active and the requirement checklist starts its expiry reminders.

**Alternatives:** "request completion" sends the worker a link to the missing sections; "reopen" unlocks fields and needs a reason that is stored.

**Outcome:** a verified dossier with confirmed expiry dates, and every value has its original and who confirmed it.

#### J-H4. Act on an expiring document
**Who:** Katalin. **Module:** A3.

1. The HR dashboard (S-A30) shows "12 expiring soon".
2. She opens Expiring documents (S-A36), sorted by date, with severity and owner.
3. For a residence permit expiring in 30 days she opens the person (S-A32) and asks the worker for the renewed document.
4. When the new scan arrives she confirms it in Document review (S-A37). The old document stays in history.
5. The task closes. If she cannot act in time, an overdue item needs a stated reason before it can be closed.

**Outcome:** nobody finds out about an expired permit from an authority first.

#### J-H5. Generate and file a contract
**Who:** Katalin. **Module:** A3.

1. In Contract generation (S-A40) chooses the person and the approved template.
2. Reviews the preview, filled only with confirmed profile values.
3. Generates the versioned PDF; prints it; the worker signs on paper outside the platform.
4. Uploads the signed scan; the "unsigned" flag clears and the dashboard count drops.

**Outcome:** every contract is traceable to a template version and confirmed data.

#### J-H6. Resolve a possible duplicate person
**Who:** Katalin. **Module:** A3.

1. A new submission shares an email and passport number with an existing record. A task points to Possible duplicate people (S-A39).
2. She compares the two records side by side and sees which fields match.
3. She merges, links or keeps them separate. A merge keeps the history of both.

**Outcome:** no double records, with a reason on file.

#### J-H7. Change the requirement matrix
**Who:** Katalin, or Zoltán. **Module:** A3.

1. Opens the Requirement matrix (S-A38).
2. Adds "Health insurance certificate" as required for one permit route.
3. Previews what a sample worker on that route will now be asked.
4. Saves; the change applies to new invitations, and she chooses whether open onboardings are updated.

**Outcome:** rules change without a software release.

## Part B journeys

#### J-B1. Invoices arrive by email (B1)
1. A supplier emails an invoice to the intake address.
2. The platform checks sender, size and type, drops duplicates, and starts OCR.
3. Anna sees the message on the Email intake monitor (S-B01) as accepted; a rejected one shows a reason and a "reprocess" button.
4. The invoice is waiting in the New/Review view (S-A11) like an uploaded one.

#### J-B2. NAV import runs on schedule (B2)
1. Every night the platform queries NAV Online Invoice.
2. NAV import runs (S-B02) shows counts imported, matched to a received invoice, or not matched.
3. A failed run shows the error and a retry button; the previous successful run is not affected.
4. With C09, unmatched NAV invoices become "missing invoice" alerts.

#### J-B3. Finalise an EKÁER declaration (B3)
1. A purchase case reaches Ready and appears in EKÁER declarations (S-B03).
2. Anna opens it (S-B04) and sees the mandatory-data checklist; missing fields block submission.
3. She creates the declaration, modifies it if the delivery changes, then finalises it.
4. The request, response and finalisation time are stored and show on the case timeline. A rejection returns it to review with the reason.

#### J-B4. Prepare a payment file (B4)
1. Anna opens Payment file export (S-B05) and selects approved, unpaid invoices.
2. The system checks beneficiary, account and amount; invalid items are listed.
3. She generates the bank import file, uploads it at the bank, and returns to confirm "submitted".
4. The invoices show "sent to bank" with batch, user and time.

#### J-B5. Parse bank notices (B5)
1. A bank notification email arrives.
2. Bank notices (S-B06) shows the extracted reference, amount and currency and a proposed invoice or batch.
3. Anna confirms or corrects; unmatched notices remain visible.

#### J-B6. Connect the bank API (B6)
1. Zoltán or Bence opens Bank API connection (S-B07) and starts the bank's authorisation.
2. The screen shows accounts, authorisation expiry and the last retrieval.
3. Approved batches can be submitted from the platform; statements arrive automatically.

#### J-B7. Change the OCR provider (B7)
1. Zoltán opens OCR and AI provider settings (S-B08).
2. Compares accuracy and cost per provider on the labelled sample.
3. Chooses a provider per document type and sets a fallback and a monthly cap.

## Part C journeys

### Finance additions

#### J-C01. Approve an invoice (C01)
1. After review, an invoice above the limit goes to the Approval inbox (S-C01).
2. Bence opens the original and the coding, then approves, rejects with a reason, or delegates.
3. If he does nothing, a reminder is sent and, after the set time, it escalates to his deputy.
4. Rules (amount, supplier, cost centre, approvers, delegation periods) are kept in Approval rules (S-C02).

#### J-C02. Change a supplier's bank account (C02)
1. Anna requests a new account in Supplier detail (S-A19) and adds a verification note.
2. Supplier bank-account changes (S-C03) shows old vs new and needs two approvals, neither of them Anna.
3. Payments to this supplier are held for the set period after approval.

#### J-C03. Handle a credit note (C03)
1. Anna confirms an invoice and sets its type to Credit note.
2. Credit notes and corrections (S-C04) shows the chain with the original and the netted balance.
3. She links it to the original; the payable balance updates.

#### J-C04. Resolve a duplicate (C04)
1. A new invoice has the same supplier and number as one already confirmed.
2. Duplicate invoices (S-C05) shows both side by side with the matching keys.
3. She keeps one, merges the files, or rejects the second.

#### J-C05. Code an invoice (C05)
1. In the invoice review (S-A13), a coding panel suggests GL account, cost centre and VAT code from the supplier's history.
2. Anna accepts or changes them; split coding is allowed.
3. Accounts and cost centres (S-C06) holds the lists, which can be imported from the accountant.

#### J-C06. Check exchange rates (C06)
1. A foreign-currency invoice shows its HUF equivalent using the rule in Exchange rates (S-C07).
2. If the rate is missing, she sees why and may override with a reason.

#### J-C07. Approve a payment batch (C07)
1. Anna opens Payment proposals (S-C08), which suggests invoices by due date, with supplier holds shown.
2. She builds a batch, adjusts partial amounts, and submits it.
3. Bence reviews the batch (S-C09) and approves it; a second approver is needed above the limit.
4. With B4 the file is generated next; without it, the batch is printed or sent by the accountant.

#### J-C08. Reconcile a bank statement (C08)
1. Anna imports the day's statement on Bank statements and reconciliation (S-C10).
2. Most lines match payments automatically; she confirms proposed matches and handles the unmatched ones.
3. Matched invoices change to Paid with the statement line as evidence.

#### J-C09. Review NAV differences (C09)
1. NAV cross-check (S-C11) lists invoices where NAV and the reviewed data differ, and NAV invoices never received.
2. Anna resolves each with a note, or asks the supplier.

#### J-C10. Export to the accounting software (C10)
1. On Bookkeeping export (S-C12) she sees the count of coded, approved invoices ready.
2. She runs the export; the result lists accepted and rejected invoices.
3. Exported invoices are locked; changes later need a reason and a corrective export.

#### J-C11. Close a period (C11)
1. Bence locks the month on Accounting periods (S-C13) after the export.
2. Eszter opens AP reports (S-C14) with read-only access, picks ageing and input VAT, and exports them.

#### J-C12. Build a scheduled report (C12)
1. Bence opens Report builder (S-C15), picks a list and columns, and sets filters.
2. He schedules it weekly by email to himself and the owner.

#### J-C14. Issue a sales invoice (C14)
1. Anna creates an invoice for a customer (S-C17), sends it, and later records the receipt.
2. Receivables show in the same dashboards as payables.

#### J-C15. See the cash forecast (C15)
1. Bence opens Cash-flow forecast (S-C18) and sees expected payments by week from approved invoices and planned batches.

#### J-C16. Submit an expense claim (C16)
1. An employee submits a claim with receipts (S-C19); a manager approves; accounting adds it to a reimbursement batch.

### HR additions

#### J-C20. Record employment changes and assignments (C20)
1. Katalin opens Employment and assignments (S-C20) and sees an effective-dated timeline.
2. She records a position change from a future date and attaches the annex.
3. She assigns the worker to a customer site with start date and documents.

#### J-C21. Offboard a leaver (C21)
1. Katalin starts Offboarding (S-C21) with end date and reason.
2. A checklist (access, equipment, accommodation, final documents) creates tasks for their owners.
3. When complete, the retention clock starts and the status becomes Former.

#### J-C22. Track training, medical and licences (C22)
1. Medical, training and licences (S-C22) lists records by due date and what each job type requires.
2. Reminders use the same windows as identity documents.

#### J-C23. Run a permit renewal (C23)
1. Permit renewals (S-C23) shows a case opened from an expiring permit.
2. Katalin records filing date, receipt and, later, the decision.
3. While pending, the permit shows "renewal pending" rather than expired.

#### J-C24. Track statutory registrations (C24)
1. Statutory registrations (S-C24) lists the notification due for a new joiner with its deadline.
2. Katalin records the reference and confirmation date.

#### J-C25. Manage templates (C25)
1. In Template library (S-C25) she uploads a new bilingual template, checks merge fields, sends it for legal approval, and publishes a version.

#### J-C26. Export payroll changes (C26)
1. On Payroll export (S-C26) she picks the period, reviews joiners, changes and leavers, and exports the provider's file.

#### J-C27. Read workforce reports (C27)
1. Workforce reports (S-C27) show headcount by nationality and site, the permit expiry forecast and the onboarding funnel, with export.

#### J-C28. Employee portal (C28)
1. Ravi signs in to the Employee portal (S-C28), sees his documents and expiry dates, updates his address and uploads a renewed permit.
2. Katalin reviews the upload through the usual review screen (S-A37).

#### J-C29. Send reminders by SMS or WhatsApp (C29)
1. In Messaging (S-C29) Katalin sets templates per language and quiet hours.
2. Workers who gave consent get expiry reminders in their language; failures are logged.

#### J-C30. Manage accommodation (C30)
1. On Accommodation (S-C30) she assigns a worker to a room with dates and cost; offboarding vacates it.

#### J-C31. Sign a contract electronically (C31)
1. From a generated contract she sends it for signature; E-signature status (S-C31) shows who has signed; the signed copy is filed on the profile.

### Platform additions

#### J-C40. Manage legal entities (C40)
1. Zoltán adds a second entity on Legal entities (S-C40), with its numbering and bank accounts, and scopes roles to it.

#### J-C41. Add a custom field (C41)
1. On Custom fields (S-C41) he adds "Purchase order number" to invoices, with a validation rule; it appears in the form, list and export.

#### J-C42. Answer a data-subject request (C42)
1. A former worker asks for his data. Zoltán opens Data-subject requests (S-C42), finds the person, and produces an export package.
2. Retention exceptions are flagged; the completion is recorded.

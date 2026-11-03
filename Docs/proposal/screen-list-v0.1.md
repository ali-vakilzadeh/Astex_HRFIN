# Astex HR and Finance Platform: Screen List

Task 3 companion to the clickable mockup (`mockup/index.html`). Version 0.1, 2 November 2026.

Every screen carries the Part letter and module ID it belongs to. The mockup hides or greys screens whose module is not in the selected package, so Astex can see exactly what each choice adds.

## Conventions

- **Part A** screens belong to the core and are always present. **Part B** and **Part C** screens belong to optional modules.
- Screens are described by purpose and key elements only. Visual detail is in the mockup.
- The **Worker form** group is the public mobile form opened from an invitation link. It has no menu and no sign-in.
- Roles: **ACC** Finance accountant; **MGR** Finance manager / approver; **HRA** HR administrator; **OWN** Platform owner; **WRK** Worker (invitee); **EXT** External bookkeeper; **EMP** Employee; **ALL** Any signed-in user.
- Journey IDs refer to `docs/02-user-journeys.md`.

## Summary

| Group | Screens | Part A | Part B | Part C |
| --- | --- | --- | --- | --- |
| Auth | 1 | 1 | 0 | 0 |
| Home | 3 | 3 | 0 | 0 |
| Finance | 36 | 12 | 7 | 17 |
| HR | 22 | 11 | 0 | 11 |
| Administration | 15 | 10 | 1 | 4 |
| Worker form | 8 | 8 | 0 | 0 |
| Employee portal | 1 | 0 | 0 | 1 |
| **Total** | 86 | 45 | 8 | 33 |

## Navigation structure

- **Home**
    - (top level): My work [A]; Task inbox [A]; Search results [A]
- **Finance**
    - Overview: Finance dashboard [A]
    - Invoices: Invoice register [A]; Upload invoices [A]; Invoice review (OCR) [A]; Invoice detail [A]; Email intake monitor [B]; Credit notes and corrections [C]; Duplicate invoices [C]
    - Purchase cases: Purchase cases [A]; Purchase case detail [A]; Matching review queue [A]
    - Suppliers: Suppliers [A]; Supplier detail [A]; Goods dictionary [A]
    - Payments: Payment status [A]; Payment file export [B]; Payment proposals [C]; Payment batch approval [C]
    - Banking and tax: NAV import runs [B]; EKÁER declarations [B]; EKÁER declaration detail [B]; Bank notices [B]; Bank API connection [B]; Bank statements and reconciliation [C]; NAV cross-check [C]
    - Approvals: Approval inbox [C]; Approval rules [C]; Supplier bank-account changes [C]
    - Setup: Accounts and cost centres [C]; Exchange rates [C]
    - Accounting: Bookkeeping export [C]; Accounting periods [C]; AP reports [C]
    - Sales: Sales invoices [C]; Cash-flow forecast [C]; Expense claims [C]
- **HR**
    - Overview: HR dashboard [A]; Workforce reports [C]
    - People: People [A]; Person profile [A]; Possible duplicate people [A]; Employment and assignments [C]
    - Onboarding: Invitations [A]; New invitation [A]; Submission review [A]
    - Documents: Expiring documents [A]; Document review (OCR) [A]; Contract generation [A]; Medical, training and licences [C]; Template library [C]; E-signature status [C]
    - Setup: Requirement matrix [A]; Messaging (SMS / WhatsApp) [C]
    - Lifecycle: Offboarding [C]; Permit renewals [C]; Statutory registrations [C]; Payroll export [C]; Accommodation [C]
- **Administration**
    - Access: Users [A]; Roles and permissions [A]
    - Monitoring: Audit log [A]; Job monitor [A]; Report builder and schedules [C]; Data-subject requests [C]
    - Setup: Settings [A]; Notification templates [A]; Retention rules [A]; Data migration wizard [A]; Module catalog [A]; Integrations and credentials [A]; OCR and AI provider settings [B]; Legal entities [C]; Custom fields [C]
- **Employee portal**
    - (top level): Employee portal home [C]

## Auth

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-A00 | Sign-in and MFA | A / A1 | ALL | Sign in with email and password, then a second factor. | email + password; MFA code; lockout message; language switch EN/HU | J-P1 |

## Home

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-A01 | My work | A / A1 | ALL | Start page showing what needs my attention today. | my open tasks by type; items due today; shortcuts to my main lists; recent items | J-P1 |
| S-A02 | Task inbox | A / A1 | ALL | Every review, approval, expiry and exception as an owned task. | filter by type, owner, due; priority; overdue highlight; bulk reassign; open-in-context link | J-P1, J-H4 |
| S-A03 | Search results | A / A1 | ALL | Find invoices, people, suppliers and cases by any text, including translated text. | search box with filters; results grouped by record type; highlighted matches; language-of-match tag | J-P6 |

## Finance

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-A10 | Finance dashboard | A / A2 | MGR, ACC | Spend, overdue and review workload at a glance. | monthly spend; paid on time %; incomplete cases; OCR/match review backlog; overdue list; top suppliers | J-F5 |
| S-A11 | Invoice register | A / A2 | ACC, MGR | All invoices in four working views. | tabs New/Review, Open, Incomplete, Closed; sort, filter, saved views; status chips; CSV/XLSX export of current filter | J-F1, J-F3 |
| S-A12 | Upload invoices | A / A2 | ACC | Add invoices and related documents by drag and drop. | drop zone; file type and size rules; per-file progress; duplicate-file warning; choose document role | J-F1 |
| S-A13 | Invoice review (OCR) | A / A2 | ACC | Confirm extracted values against the original and the English translation. | original on the left; fields on the right with confidence; validation messages; translation panel; confirm / send to specialist | J-F1 |
| S-A14 | Invoice detail | A / A2 | ACC, MGR | One invoice with its data, files, translation, case and history. | header values; line items; files and versions; source and English text; linked case; payment status; timeline and notes | J-F1, J-F3 |
| S-A15 | Purchase cases | A / A2 | ACC | All purchase cases with completeness and payment state. | states New, Processing, Incomplete, Ready, Paid, Closed; missing-document badges; owner; filters | J-F2 |
| S-A16 | Purchase case detail | A / A2 | ACC | The dossier: invoice, delivery note, EKÁER, closing document and other evidence. | case header; document roles with present/missing; linked files; completeness vs consistency vs payment; timeline | J-F2 |
| S-A17 | Matching review queue | A / A2 | ACC | Documents the system could not link with high confidence. | ranked candidate cases; agreements and conflicts per signal; link, create new case or leave unmatched | J-F2 |
| S-A18 | Suppliers | A / A2 | ACC, MGR | Supplier master list. | name, tax ID, country, alias count, open invoices, last invoice; search by alias | J-F4 |
| S-A19 | Supplier detail | A / A2 | ACC | One supplier with aliases, bank accounts and history. | identity; controlled aliases; bank accounts with validity dates; purchase history; recurring goods; tax-number check result | J-F4 |
| S-A20 | Goods dictionary | A / A2 | ACC | Synonyms so one search finds goods in any language. | canonical term; synonyms per language; usage count; preview of what a search finds | J-F4, J-P6 |
| S-A21 | Payment status | A / A2 | ACC, MGR | Open invoices with due dates; mark paid manually. | due list; overdue highlight; mark paid with date, reference and user; partial payment note | J-F3 |
| S-B01 | Email intake monitor | B / B1 (R1) | ACC | What arrived by email and what happened to it. | received messages; accepted, rejected, duplicate, needs review; sender; attachments; reason; reprocess | J-B1 |
| S-B02 | NAV import runs | B / B2 (R1) | ACC | NAV query runs and imported invoices. | run history; counts; failures with retry; identity reconciliation result; run now; environment | J-B2 |
| S-B03 | EKÁER declarations | B / B3 (R1) | ACC | Declarations and what they still need. | states (draft, missing data, ready, finalised, rejected); linked case; mandatory-field checklist | J-B3 |
| S-B04 | EKÁER declaration detail | B / B3 (R1) | ACC | One declaration with request, response and finalisation. | mandatory data; validation; create, modify, finalise buttons; request and response log; timestamps | J-B3 |
| S-B05 | Payment file export | B / B4 (R1) | ACC, MGR | Generate the bank import file for selected invoices. | select approved unpaid; validation of beneficiary, account, amount; generate; download; 'did you submit it?' confirmation | J-B4 |
| S-B06 | Bank notices | B / B5 (O) | ACC | Parsed bank notification emails and proposed matches. | notice list; extracted reference, amount, currency, beneficiary; proposed invoice or batch; confirm or correct | J-B5 |
| S-B07 | Bank API connection | B / B6 (O) | OWN, MGR | Connect and monitor the direct bank link. | connection status; accounts; authorisation expiry; retrieval schedule; last payment submission | J-B6 |
| S-C01 | Approval inbox | C / C01 (R1) | MGR | Invoices waiting for my approval. | amount, supplier, cost centre; open original; approve, reject with reason, delegate; due and escalation | J-C01 |
| S-C02 | Approval rules | C / C01 (R1) | OWN, MGR | Who approves what. | rules by amount, supplier, cost centre; approvers and deputies; delegation periods; reminders | J-C01 |
| S-C03 | Supplier bank-account changes | C / C02 (R1) | ACC, MGR | Requested changes and their two approvals. | old vs new account; verification note; approvals; payment hold until date | J-C02 |
| S-C04 | Credit notes and corrections | C / C03 (R1) | ACC | Chains of original, credit, storno, advance and final invoices. | chain view; netted balance; link tool; type selector | J-C03 |
| S-C05 | Duplicate invoices | C / C04 (R1) | ACC | Suspected duplicates before approval. | side-by-side; matching keys; keep, merge, reject | J-C04 |
| S-C06 | Accounts and cost centres | C / C05 (R1) | ACC, OWN | Chart of accounts, cost centres and VAT codes used for coding. | three lists with import; active flag; default coding per supplier | J-C05 |
| S-C07 | Exchange rates | C / C06 (R1) | ACC | Stored rates and the date rule. | rate table by date and currency; rule setting; manual override with reason | J-C06 |
| S-C08 | Payment proposals | C / C07 (R1) | ACC | Suggested payments by due date. | proposal by due date; partial amounts; supplier hold flags; add to batch | J-C07 |
| S-C09 | Payment batch approval | C / C07 (R1) | MGR | Review and approve a batch (four-eyes). | batch total; invoices; approvers; approve or return; export (with B4) | J-C07 |
| S-C10 | Bank statements and reconciliation | C / C08 (R1) | ACC | Import statements and confirm matches. | import file; lines matched, proposed, unmatched; confirm or correct; account balance check | J-C08 |
| S-C11 | NAV cross-check | C / C09 (R2) | ACC | Differences between NAV and reviewed invoices. | mismatch list; missing-invoice alerts; resolve with note | J-C09 |
| S-C12 | Bookkeeping export | C / C10 (R1) | ACC | Send coded invoices to the accounting software. | ready-to-export count; target software; run export; result; locked records; re-export with reason | J-C10 |
| S-C13 | Accounting periods | C / C11 (R2) | ACC, MGR | Open and locked periods. | period list; lock; reopen with reason; corrective exports | J-C11 |
| S-C14 | AP reports | C / C11 (R2) | MGR, ACC, EXT | Ageing, due list, supplier balance, spend by cost centre, input VAT. | report picker; period; export | J-C11 |
| S-C17 | Sales invoices | C / C14 (O) | ACC | Outgoing invoices and receivables. | customer; invoice; status; due; receipts | J-C14 |
| S-C18 | Cash-flow forecast | C / C15 (O) | MGR | Expected cash by week. | weekly bars; payables, receivables; opening balance | J-C15 |
| S-C19 | Expense claims | C / C16 (O) | ACC, MGR | Claims, receipts and reimbursement. | claim list; receipt; approve; reimbursement batch | J-C16 |

## HR

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-A30 | HR dashboard | A / A3 | HRA | Expiries, missing documents and reviews waiting. | expiring soon; missing documents; unsigned contracts; OCR reviews; new submissions; incomplete profiles; by owner | J-H4 |
| S-A31 | People | A / A3 | HRA | Everyone with status and readiness. | status (candidate, onboarding, active, leaving, former); nationality; owner; completeness bar; filters; export | J-H1, J-H3 |
| S-A32 | Person profile | A / A3 | HRA | The verified dossier for one person. | tabs Overview, Identity and permits, Documents, Onboarding, Contract, Timeline; masked identifiers with logged unmask; owner | J-H3, J-H4 |
| S-A33 | Invitations | A / A3 | HRA | All invitations and where each stands. | Draft, Sent, Opened, Submitted, Approved; resend, revoke, copy link; expiry date | J-H1 |
| S-A34 | New invitation | A / A3 | HRA | Create a named record and send a secure link. | name, email, language, onboarding type; preview of requirement set; privacy notice version; send or save draft | J-H1 |
| S-A35 | Submission review | A / A3 | HRA | Check a worker's submission and approve or reopen. | answers by section; required vs provided; documents with OCR status; approve; request completion; reopen with reason | J-H3 |
| S-A36 | Expiring documents | A / A3 | HRA | What expires, when, and who owns the action. | windows 90/60/30/14/7; severity; owner; overdue need a reason; filter by document type | J-H4 |
| S-A37 | Document review (OCR) | A / A3 | HRA | Confirm extracted expiry values against the scan. | scan viewer; extracted fields with confidence; date format normalised; confirm, correct, reject; replacement keeps history | J-H3, J-H4 |
| S-A38 | Requirement matrix | A / A3 | HRA, OWN | Which fields and documents each kind of worker needs. | rows document types; columns nationality, permit route, job category; required/optional; preview for a sample worker | J-H7 |
| S-A39 | Possible duplicate people | A / A3 | HRA | Records that may be the same person. | side-by-side comparison; matching email, name, document number; merge, link or keep separate | J-H6 |
| S-A40 | Contract generation | A / A3 | HRA | Generate the printable contract and file the signed scan. | template; merge preview from confirmed values; version history; generate PDF; upload signed scan; unsigned flag | J-H5 |
| S-C20 | Employment and assignments | C / C20 (R1) | HRA | History of position, site and customer assignment. | effective-dated timeline; assignment documents; change with reason | J-C20 |
| S-C21 | Offboarding | C / C21 (R1) | HRA | Leaver checklist and retention clock. | end date and reason; checklist items with owners; final documents; close | J-C21 |
| S-C22 | Medical, training and licences | C / C22 (R2) | HRA | Fitness, safety training and licence expiry. | record types; due dates; required by job type; reminders | J-C22 |
| S-C23 | Permit renewals | C / C23 (R2) | HRA | Renewal cases for permits. | states: preparing, filed, receipt, pending, decided; expiry vs filing date | J-C23 |
| S-C24 | Statutory registrations | C / C24 (R2) | HRA | Registrations and notifications due. | due by deadline; reference and confirmation date; overdue | J-C24 |
| S-C25 | Template library | C / C25 (R2) | HRA, OWN | Contract and letter templates. | template list; languages; versions; approval; merge-field check | J-C25 |
| S-C26 | Payroll export | C / C26 (R2) | HRA | Joiners, changes and leavers for payroll. | period selector; changes list; export file; sent flag | J-C26 |
| S-C27 | Workforce reports | C / C27 (R2) | HRA, MGR | Headcount, permit forecast and onboarding funnel. | charts and tables; filters; export | J-C27 |
| S-C29 | Messaging (SMS / WhatsApp) | C / C29 (O) | HRA, OWN | Gateway settings and message log. | provider; templates per language; consent; delivery log | J-C29 |
| S-C30 | Accommodation | C / C30 (O) | HRA | Housing records and occupancy. | property, room, occupants, dates, cost | J-C30 |
| S-C31 | E-signature status | C / C31 (O) | HRA | Contracts out for electronic signature. | envelope status; signers; reminders; signed copy filed | J-C31 |

## Administration

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-A50 | Users | A / A1 | OWN | Back-office users and their roles. | invite user; roles; MFA status; last sign-in; deactivate | J-P2 |
| S-A51 | Roles and permissions | A / A1 | OWN | Build roles from permission sets. | permission tree per module; entity scope; field masking switches; segregation-of-duties rules; test as role | J-P2 |
| S-A52 | Audit log | A / A1 | OWN | Who did what, when, from which source. | filter by actor, record, action, source; before/after values; reason for overrides; export | J-P3 |
| S-A53 | Job monitor | A / A1 | OWN | Background work and failures. | queued, processing, review, completed, failed, dead-letter; retry; owner; error class; correlation ID | J-P3 |
| S-A54 | Settings | A / A1 | OWN | Company-wide configuration. | company and entity; languages; thresholds; reminder windows; invitation validity; sending domain | J-P2 |
| S-A55 | Notification templates | A / A1 | OWN, HRA | Wording of every email, per language. | template list; EN/HU/other editor; variables; test send; version | J-P2 |
| S-A56 | Retention rules | A / A1 | OWN | How long each record and document type is kept. | rule per category; legal hold; scheduled deletion runs needing approval | J-P2 |
| S-A57 | Data migration wizard | A / A1 | OWN | Import Excel registers and scan folders. | choose source; map columns; validate; trial load report with counts and totals; cutover | J-P4 |
| S-A58 | Module catalog | A / A1 | OWN | Switch optional modules on and see what each needs. | Part A/B/C; tier; requires; what Astex must supply; status; selection summary | J-P5 |
| S-A59 | Integrations and credentials | A / A1 | OWN | Connection status for outside systems. | per adapter: configured, test, last success; secrets write-only; environment (test/production) | J-P2 |
| S-B08 | OCR and AI provider settings | B / B7 (O) | OWN | Choose providers per document type. | provider per document type; fallback; monthly cost and cap; accuracy comparison | J-B7 |
| S-C15 | Report builder and schedules | C / C12 (R2) | MGR, OWN | Build, save and email reports. | pick list and columns; filters; schedule; recipients | J-C12 |
| S-C40 | Legal entities | C / C40 (R2) | OWN | Entities, numbering, bank accounts and scopes. | entity list; per-entity settings; role scope | J-C40 |
| S-C41 | Custom fields | C / C41 (O) | OWN | Add validated fields without a release. | record type; field type; validation; show in list, form, export | J-C41 |
| S-C42 | Data-subject requests | C / C42 (R2) | OWN, HRA | Export or correct one person's data. | request log; person lookup; export package; retention exceptions; completion record | J-C42 |

## Worker form

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-W01 | Invitation landing | A / A3 | WRK | Welcome and privacy notice. | company name; what you will need; privacy notice with version acknowledgement; language choice; start | J-H2 |
| S-W02 | Email code | A / A3 | WRK | Confirm it is really the invited person. | 6-digit code; resend; attempts left | J-H2 |
| S-W03 | Form 1: personal and identity | A / A3 | WRK | Name, birth details, nationality, travel document. | legal and former name; birth place and date; mother's name; nationality; passport number, issuer, dates; save and continue | J-H2 |
| S-W04 | Form 2: contact and address | A / A3 | WRK | How to reach the worker. | email; phone; permanent and current address; address card number; emergency contact; preferred language | J-H2 |
| S-W05 | Form 3: permits and compliance | A / A3 | WRK | Sections that depend on nationality and permit route. | tax ID; TAJ; health insurance; residence permit; work permit; visa where required; unused sections hidden | J-H2 |
| S-W06 | Form 4: upload documents | A / A3 | WRK | Add scans or photos of required documents. | checklist from requirement matrix; camera or file; size and type rules; preview; retake | J-H2 |
| S-W07 | Form 5: review and submit | A / A3 | WRK | Check everything and submit. | summary by section; missing items flagged; consent; submit; fields lock after submission | J-H2 |
| S-W08 | Confirmation and link states | A / A3 | WRK | Thank-you page and the states of an expired or revoked link. | submitted confirmation; expired link shows no personal data; revoked; already submitted; ask for a new link | J-H2 |

## Employee portal

| ID | Screen | Part / module | Roles | Purpose | Key elements | Journeys |
| --- | --- | --- | --- | --- | --- | --- |
| S-C28 | Employee portal home | C / C28 (O) | EMP | My data, documents and reminders. | my profile; documents and expiry; update address; upload renewal | J-C28 |

## Screens that are panels, not pages

- **C13 Supplier VAT number validation** has no page of its own. Its result shows as a panel inside S-A19 Supplier detail.
- **Search box** is in the top bar of every signed-in screen and opens S-A03.
- **Notes and timeline** appear as a tab on every detail screen (S-A14, S-A16, S-A19, S-A32).

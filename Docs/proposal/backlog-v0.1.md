# Astex HR and Finance Platform: Scope and Requirements Backlog

Task 1 of the proposal. Version 0.1, 2 November 2026. Prepared from the Astex proposal document (11 of 13 pages received) and the earlier Requirements and Delivery Plan.

## 1. How this backlog is organised

The scope is split into three parts, so Astex can say yes or no to each piece instead of to one large project.

| Part | What it contains | How it is built and priced |
| --- | --- | --- |
| **A. Core** | Functions described in the Astex document and clear enough to build, plus a small set of cheap *extension points* that stop later modules forcing a redesign | One fixed scope |
| **B. Conditional modules** | Functions that are in the Astex document but depend on a format, access right or decision we do not have yet (bank file format, NAV and EKÁER access, mailbox set-up, bank API) | Plugin modules with a fixed contract; the outside-system adapter is estimated once its specification arrives |
| **C. Suggested additions** | Functions that are not in the Astex document but that this kind of operation almost always needs (approvals, accounting export, statement reconciliation, offboarding and so on) | Plugin modules with the same contract; each optional |

Every module has an ID (A1, B1, C01), a tier, and a list of other modules it requires:

| Tier | Meaning |
| --- | --- |
| **core** | Part A. Always delivered |
| **R1** | Recommended first wave. We expect Astex to need it at or soon after go-live |
| **R2** | Recommended later. Valuable, can follow once R1 is running |
| **O** | Optional. Only if Astex confirms the need |

Requirement IDs such as FI-25 or HR-10 refer to the numbered requirements in the Requirements and Delivery Plan document. They stay as cross-references so nothing in that document is lost. The plan's old B/R/E tags are replaced by the Part letters used here.

### Summary

| Part | Modules | Tiers | Screens |
| --- | --- | --- | --- |
| A. Core | 3 | 3 core modules (platform, finance, HR) | 45 |
| B. Conditional | 7 | R1: 4, O: 3 | 8 |
| C. Suggested | 31 | R1: 11, R2: 12, O: 8 | 33 |

Total: 41 modules and 86 screens.

## 2. Part A: core backlog

Source column: "Client p.N" means the Astex document page; "Plan" means added by us to protect the design.

### A1. Platform foundation

| ID | Story | Plan ref | Source | Done when |
| --- | --- | --- | --- | --- |
| A1.01 | Back-office users sign in with email and password; a second factor is mandatory | SH-02 | Client p.10 (MFA optional; we make it mandatory) | A sign-in without a second factor is impossible |
| A1.02 | Roles are built from permission sets per module and action; sensitive identifiers are masked by field | SH-03 | Client p.10 | Allow and deny tests pass for every role; each unmask is logged |
| A1.03 | Every create, change, delete and unmask is recorded with actor, role, source, before and after values, and a reason for overrides | SH-05 | Client p.10 | Any record's full history can be shown |
| A1.04 | Files are stored privately with original, versions, checksum and short-lived download links | SH-06 | Client p.2, 6, 9 | Replacing a file keeps the old version |
| A1.05 | Uploads are checked for type and size and pass a malware-scan hook before use | SH-07 | Client p.10 | An infected test file is quarantined |
| A1.06 | OCR runs through a provider-independent interface; one default provider is included; field value, confidence, source page and raw result are kept | SH-08 | Client p.4, 9 | The provider can be swapped by configuration |
| A1.07 | Background jobs move through queued, processing, review, completed, failed, retry and dead-letter, and every failure has an owner and a recovery action | SH-09 | Client p.10 | Fault-injection runs recover with no duplicates |
| A1.08 | Notifications appear in the app and by email with de-duplication and per-language templates | SH-12 | Client p.4 | One event never sends two identical emails |
| A1.09 | A task inbox lists every review, approval, expiry and exception with owner, due date and priority | SH-11 | Client p.4, 8 | No exception exists without an owner |
| A1.10 | Full-text search covers record fields, OCR text and English translations, with prefix, alias and synonym matching | SH-13 | Client p.8 | Searching "forklift" finds a Hungarian invoice for *targonca* |
| A1.11 | Every list has sort, filter, pagination, saved views and CSV/XLSX export that respects filters and masking | SH-14 | Client p.5 | An export equals the on-screen list |
| A1.12 | Administrators change document types, thresholds, reminder windows and templates without a release | SH-16 | Client p.4 | A new document requirement is added in the admin screen |
| A1.13 | The interface is available in Hungarian and English | SH-17 | Plan | Every screen and email exists in both |
| A1.14 | Retention rules per record category, legal hold, and deletion or anonymisation runs that need approval | SH-18 | Client p.10 | A record past retention is removed only after an approved run |
| A1.15 | Notes and one timeline on every main record | SH-19 | Client p.6 | A record's history shows in one place |
| A1.16 | A migration wizard imports Excel registers and scan folders, with a trial-load report of counts and totals | none | Client p.8 (goal only) | Each trial load reconciles and is signed by the module owner |
| A1.17 | The module host: modules register their menu slots, permissions, settings, events and adapters, and can be enabled per legal entity | EP-01 | Plan | A test module appears in the menu without changing core code |
| A1.18 | A workflow engine with states, transitions, approvers, deadlines and escalation, running a single-step default | EP-02 | Plan | An approval rule can be added later by configuration only |
| A1.19 | An event outbox and event catalog so modules react to recorded events | EP-07 | Plan | A new consumer is added without changing the producer |
| A1.20 | An adapter registry and credential vault: secrets are write-only, test and production are separate | EP-08 | Client p.9 (secrets server-side) | No secret is ever shown after saving |
| A1.21 | The organisation model carries legal entity and cost centre on every business row, even while Astex uses one of each | EP-03, SH-01 | Client p.9 (organisation ID) | A second entity can be added with no data migration |

### A2. Finance core

| ID | Story | Plan ref | Source | Done when |
| --- | --- | --- | --- | --- |
| A2.01 | Accountants upload invoices and related files by drag and drop; the same file is not loaded twice | FI-01 | Client p.5 | A repeated upload is flagged by checksum |
| A2.02 | OCR proposes the invoice fields Astex listed (supplier, tax ID, number, three dates, reference, subject, currency, net, VAT, gross, items, language) with confidence | FI-02 | Client p.5 | Field mapping confirmed on representative Astex invoices |
| A2.03 | A side-by-side review shows the original and editable values with validation messages | FI-03 | Client p.5 | Totals that do not add up block confirmation |
| A2.04 | After review, an English translation of the subject, items, line descriptions and relevant text is stored with the original and is searchable | FI-04 | Client p.5, 8 | Every confirmed invoice has a linked translation |
| A2.05 | The invoice register shows New/Review, Open, Incomplete and Closed views | FI-05 | Client p.5 | Each invoice appears in exactly one view |
| A2.06 | A supplier master with tax ID first, controlled aliases and effective-dated bank accounts | FI-13 | Client p.6 | Two spellings of one supplier resolve to one record |
| A2.07 | A goods dictionary of synonyms maintained by finance users | FI-14 | Client p.6, 8 | A new synonym works in search at once |
| A2.08 | Purchase cases link invoice, delivery note, EKÁER declaration, closing document and other files; states New, Processing, Incomplete, Ready, Paid, Closed; one active case per file; reassignment is audited | FI-06 | Client p.6 | Required document roles are configurable by purchase type |
| A2.09 | Matching scores reference numbers, supplier, dates, amounts, weight and goods; high confidence links automatically with a stored explanation | FI-07 | Client p.6 | A single weak signal never auto-links |
| A2.10 | Medium and low matches go to a review queue with ranked candidates and conflicts shown | FI-07 | Client p.6 | An ambiguous document stays unmatched until resolved |
| A2.11 | Accountants mark invoices paid manually with date, reference and user, and can note partial payments | FI-10 (manual part) | Plan: client p.7 starts at the bank file | Payment state is separate from completeness |
| A2.12 | A finance dashboard with monthly spend, paid on time, incomplete cases, review backlog, outstanding and overdue invoices and supplier history | FI-12 | Client p.8 | Totals reconcile with the UAT dataset |
| A2.13 | Invoices carry a type (default "standard") and a relation table, so credit notes and advances can be added later | EP-04 | Plan | Adding a type needs no migration of existing invoices |

### A3. HR core

| ID | Story | Plan ref | Source | Done when |
| --- | --- | --- | --- | --- |
| A3.01 | A person record stores every field in the Astex list: identity and travel, contact, employment and payroll, compliance identifiers | HR-01 | Client p.3 | All listed fields are stored, validated and reportable |
| A3.02 | People move through candidate, onboarding, active, leaving and former | HR-02 | Plan | Status drives dashboards and the requirement matrix |
| A3.03 | HR creates, sends, resends and revokes invitations; the link is a hashed, expiring, single-purpose token | HR-03 | Client p.3 | An expired or revoked link shows no personal data |
| A3.04 | A one-time email code is required before the form opens | HR-04 | Client p.3 (optional; we default it on) | A forwarded link cannot open the form |
| A3.05 | A mobile-first guided form with save-and-resume, conditional sections and privacy-notice version logging | HR-05 | Client p.3 | Each tested profile sees only its required sections |
| A3.06 | Server-side validation; final submission locks key fields until an audited reopen | HR-06 | Client p.3 | A locked field changes only with a recorded reason |
| A3.07 | HR reviews a submission for completeness and approves, requests completion or reopens | HR-03, HR-06 | Client p.3 | Each exception on the Astex list has its admin action |
| A3.08 | Possible duplicate people are proposed from email, name and document numbers; HR merges, links or keeps separate | HR-07 | Client p.3 | A merge keeps both histories |
| A3.09 | Document upload by type with OCR of the expiry fields Astex listed; every value needs human confirmation | HR-08 | Client p.4 | No extracted value becomes active unconfirmed |
| A3.10 | A configurable requirement matrix of fields and documents by nationality, permit route, job category and customer assignment | HR-09 | Client p.4 | A new requirement is added with no code change |
| A3.11 | Expiry reminders at configurable windows (default 90, 60, 30, 14, 7 days) with owner and severity; overdue items need a reason | HR-10 | Client p.4 | Reminders are never duplicated |
| A3.12 | Replacing a document keeps the previous one in history | HR-11 | Client p.4 | The full history is visible on the person |
| A3.13 | One Astex-approved template is filled from confirmed data into a versioned PDF; the signed scan is uploaded back | HR-12 | Client p.4 | The PDF matches the template and confirmed data |
| A3.14 | An HR dashboard (expiring, missing, unsigned, reviews, new submissions, incomplete profiles) and an assigned owner per person | HR-13, HR-14 | Client p.4, 8 | Counts match the lists |
| A3.15 | HR spreadsheets and document folders are imported through the A1.16 wizard | none | Client p.8 (goal only) | Trial loads reconcile |
| A3.16 | Employment data is stored as effective-dated records, with only current values shown in Part A | EP-05 | Plan | Module C20 can show history with no data conversion |

### Extension points built into Part A

These are the price of avoiding a redesign. Each is small, and none gives Astex a visible feature by itself.

| ID | What Part A builds | Why it matters | Used by |
| --- | --- | --- | --- |
| EP-01 | Module host: registry, menu slots, enable per entity, dependency checks | B and C modules appear without changing core code | all B and C |
| EP-02 | Workflow engine with a single-step default | Approvals, offboarding and change requests need real flows later | C01, C02, C21, C16 |
| EP-03 | Legal entity, cost centre and currency with a nullable HUF equivalent on every financial row | Adding coding, rates or a second entity later would otherwise migrate live data | C05, C06, C40 |
| EP-04 | Invoice type field and invoice relation table | Credit notes, storno and advances change how balances are computed | C03, C14 |
| EP-05 | Effective-dated tables for supplier bank accounts and employment records | History cannot be rebuilt once overwritten | C02, C20, C26 |
| EP-06 | One generic document-link table | New record types need no new file storage | C16, C22, C25, C30 |
| EP-07 | Event outbox and event catalog | Modules react to events instead of reaching into each other | all B and C |
| EP-08 | Adapter registry and credential vault | Banks, accounting software and providers are swapped by configuration | B1 to B7, C06, C10, C13, C26, C29, C31 |
| EP-09 | Permission registry that modules add to | Roles stay consistent as modules arrive | all B and C |
| EP-10 | Task-type registry | Every module's exceptions land in the one inbox | all B and C |
| EP-11 | Notification-template registry | Modules ship their own emails in both languages | C01, C09, C23, C29 |
| EP-12 | Settings with a per-module schema and feature flags | Modules are configured in admin screens, not in code | all B and C |

## 3. Part B: conditional modules

Each is described in the Astex document but cannot be fully designed until Astex supplies the item under "Client must supply". Module work is split in two for estimating: the **framework** (screens, rules, events, which are known now) and the **adapter** (the outside format or API, estimated as a range until the specification arrives).

| ID | Module | Area | Tier | Requires | Plan refs |
| --- | --- | --- | --- | --- | --- |
| B1 | Inbound email intake | Finance | R1 recommended | A2 | FI-01 |
| B2 | NAV Online Invoice import | Finance | R1 recommended | A2 | FI-08 |
| B3 | EKÁER declarations | Finance | R1 recommended | A2 | FI-09 |
| B4 | Bank payment file export | Finance | R1 recommended | A2 | FI-10 |
| B5 | Bank notification email parsing | Finance | O optional | A2 | FI-11 |
| B6 | Direct bank API | Finance | O optional | B4, C08 | FI-10 |
| B7 | Alternative OCR and AI providers | Platform | O optional | A1 | SH-08 |

### B1. Inbound email intake

Dedicated mailbox address on Astex's domain. Extracts attachments, validates sender, size and type, de-duplicates by Message-ID and checksum, and feeds the same OCR pipeline as manual upload.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-01
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** Dedicated address on Astex's domain; DNS or mail-forwarding change by Astex IT; Sender allow-list; Maximum attachment size policy
- **Events in:** none. **Events out:** invoice.received, email.rejected
- **Permissions:** `finance.intake.view`, `finance.intake.manage`
- **Settings:** intake address, allowed senders, max size MB, allowed file types. **Secrets:** inbound webhook signing secret
- **Adapter contract:** InboundMailAdapter (section 9)
- **UI slots:** S-B01 Email intake monitor
- **Test samples:** 20 sample emails: forwarded, multi-attachment, zip, duplicate, oversize
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### B2. NAV Online Invoice import

Scheduled query of NAV Online Invoice for incoming invoices, identity reconciliation with registered invoices, paging, retry and run log. NAV data supports but never replaces the source invoice.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-08
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** NAV technical user, signature key and exchange key; Astex tax number; Test environment access; Decision: NAV data as primary capture source or cross-check only
- **Events in:** none. **Events out:** nav.run_completed, nav.invoice_imported, nav.run_failed
- **Permissions:** `finance.nav.view`, `finance.nav.run`
- **Settings:** tax number, environment (test/production), schedule, lookback days. **Secrets:** NAV login, NAV signature key, NAV exchange key
- **Adapter contract:** NavInvoiceAdapter (section 9)
- **UI slots:** S-B02 NAV import runs
- **Test samples:** NAV sandbox runs; Recorded sample XML with paging; Failure and retry simulation
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### B3. EKÁER declarations

Creates, modifies and finalises EKÁER declarations through the NAV API once mandatory data on the purchase case is complete and validated. Stores request, response and finalisation time.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-09
- **Requires:** A2. **Uses if present:** B2
- **Client must supply:** EKÁER API access and credentials; Which goods and transports trigger EKÁER; Sample declarations with their related invoices and delivery notes; Who finalises today
- **Events in:** case.ready. **Events out:** ekaer.created, ekaer.modified, ekaer.finalised, ekaer.rejected
- **Permissions:** `finance.ekaer.view`, `finance.ekaer.submit`
- **Settings:** trigger rules by goods/transport, mandatory field map, environment. **Secrets:** EKÁER API credentials
- **Adapter contract:** EkaerAdapter (section 9)
- **UI slots:** S-B03 EKÁER declarations, S-B04 EKÁER declaration detail
- **Test samples:** EKÁER test environment scenarios; Rejected-data recovery cases
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### B4. Bank payment file export

Select approved unpaid invoices, validate beneficiary, account and amount, generate the bank's import file, record who confirmed submission and when.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-10
- **Requires:** A2. **Uses if present:** C07
- **Client must supply:** Bank name; Import file specification (HUF domestic format; SEPA pain.001 for EUR); Sample file accepted by the bank; Who authorises payments at the bank
- **Events in:** invoice.confirmed. **Events out:** payment.batch_exported, payment.submission_confirmed
- **Permissions:** `finance.payments.export`, `finance.payments.confirm`
- **Settings:** bank format, ordering account, execution date rules. **Secrets:** none
- **Adapter contract:** BankPaymentFormat (section 9)
- **UI slots:** S-B05 Payment file export
- **Test samples:** Bank test import of generated files; Invalid IBAN and amount cases
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### B5. Bank notification email parsing

Parses the bank's notification emails into payment events and proposes invoice or batch matches. Kept as a fallback; C08 statement import is more reliable.

- **Tier:** O optional. **Area:** Finance. **Plan refs:** FI-11
- **Requires:** A2. **Uses if present:** B1
- **Client must supply:** Bank notification email samples (at least 30); Agreement on one email format
- **Events in:** email.received (bank notices). **Events out:** payment.notice_parsed, payment.match_proposed
- **Permissions:** `finance.bank_notices.view`, `finance.bank_notices.confirm`
- **Settings:** sender address, field extraction pattern. **Secrets:** none
- **Adapter contract:** BankNoticeParser (section 9)
- **UI slots:** S-B06 Bank notices
- **Test samples:** 30 real notices incl. partial and foreign currency
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### B6. Direct bank API

Automated payment submission and statement retrieval through the bank's API. Needs separate discovery and a quote once the bank agreement exists.

- **Tier:** O optional. **Area:** Finance. **Plan refs:** FI-10
- **Requires:** B4, C08. **Uses if present:** C07
- **Client must supply:** Bank API agreement; Sandbox access; Strong-authentication method for payment signing; Bank's rate limits and SLAs
- **Events in:** payment.batch_approved. **Events out:** payment.batch_submitted, statement.retrieved
- **Permissions:** `finance.bank_api.manage`
- **Settings:** bank, accounts, retrieval schedule. **Secrets:** bank API client credentials, signing certificate
- **Adapter contract:** BankApiAdapter (section 9)
- **UI slots:** S-B07 Bank API connection
- **Test samples:** Bank sandbox end-to-end; Authentication expiry handling
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### B7. Alternative OCR and AI providers

Additional or replacement OCR and translation provider adapters, selectable by document type, with accuracy comparison on the labelled sample.

- **Tier:** O optional. **Area:** Platform. **Plan refs:** SH-08
- **Requires:** A1. **Uses if present:** none
- **Client must supply:** Provider contract, DPA and EU processing confirmation; Cost cap; Labelled sample for comparison
- **Events in:** none. **Events out:** provider.switched
- **Permissions:** `platform.providers.manage`
- **Settings:** provider per document type, fallback provider, monthly cost cap. **Secrets:** provider API keys
- **Adapter contract:** OcrProvider / TranslationProvider (section 9)
- **UI slots:** S-B08 OCR and AI provider settings
- **Test samples:** Side-by-side accuracy run on the labelled sample
- **Effort:** to be estimated in task 2 (framework and adapter separately)


## 4. Part C: suggested additions

None of these is in the Astex document. Each follows the same contract as Part B and can be accepted or declined on its own. Several require another module; the dependency table in section 5 prevents invalid selections.

| ID | Module | Area | Tier | Requires | Plan refs |
| --- | --- | --- | --- | --- | --- |
| C01 | Approval workflows | Finance | R1 recommended | A2 | FI-25 |
| C02 | Supplier bank-account change control | Finance | R1 recommended | A2, C01 | FI-21 |
| C03 | Invoice types and credit-note chains | Finance | R1 recommended | A2 | FI-22 |
| C04 | Duplicate invoice detection | Finance | R1 recommended | A2 | FI-23 |
| C05 | Coding: GL accounts, cost centres, VAT codes | Finance | R1 recommended | A2 | FI-26 |
| C06 | Exchange rates and HUF equivalents | Finance | R1 recommended | A2 | FI-29 |
| C07 | Payment proposals and batch approval | Finance | R1 recommended | A2 | FI-27 |
| C08 | Bank statement import and reconciliation | Finance | R1 recommended | A2 | FI-28 |
| C09 | NAV cross-check and missing-invoice alerts | Finance | R2 recommended later | A2, B2 | FI-24 |
| C10 | Bookkeeping export | Finance | R1 recommended | A2, C05 | FI-30 |
| C11 | Period lock and AP reports | Finance | R2 recommended later | A2, C10 | FI-31, FI-32 |
| C12 | Report builder and scheduled reports | Platform | R2 recommended later | A1 | SH-15 |
| C13 | Supplier VAT number validation | Finance | R2 recommended later | A2 | FI-20 |
| C14 | Sales invoices and receivables | Finance | O optional | A2, C05 | FI-34 |
| C15 | Cash-flow forecast | Finance | O optional | C07 | FI-35 |
| C16 | Expense claims and petty cash | Finance | O optional | A2 | FI-36 |
| C20 | Employment history and customer assignments | HR | R1 recommended | A3 | HR-20, HR-21 |
| C21 | Offboarding workflow | HR | R1 recommended | A3 | HR-22 |
| C22 | Medical, training and licence tracking | HR | R2 recommended later | A3 | HR-24 |
| C23 | Permit renewal cases | HR | R2 recommended later | A3 | HR-25 |
| C24 | Statutory registration tracking | HR | R2 recommended later | A3 | HR-23 |
| C25 | Template library and bilingual contracts | HR | R2 recommended later | A3 | HR-26 |
| C26 | Payroll export | HR | R2 recommended later | A3 | HR-27 |
| C27 | Workforce reports | HR | R2 recommended later | A3 | HR-28 |
| C28 | Employee self-service portal | HR | O optional | A3 | HR-29 |
| C29 | SMS and WhatsApp reminders | HR | O optional | A3 | HR-30 |
| C30 | Accommodation records | HR | O optional | A3 | HR-31 |
| C31 | Qualified e-signature | HR | O optional | C25 | HR-12 |
| C40 | Multi-entity management | Platform | R2 recommended later | A1 | SH-01 |
| C41 | Custom fields | Platform | O optional | A1 | SH-20 |
| C42 | Data-subject request tooling | Platform | R2 recommended later | A1, A3 | SH-21 |

### C01. Approval workflows

Invoice approval by amount, supplier and cost centre with delegation, rejection reasons, reminders and escalation. Runs on the A1 workflow engine.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-25
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** Approval matrix: who approves what, limits, deputies
- **Events in:** invoice.confirmed. **Events out:** invoice.approved, invoice.rejected, approval.escalated
- **Permissions:** `finance.approvals.act`, `finance.approvals.configure`
- **Settings:** approval rules, delegations, reminder interval, escalation time. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C01 Approval inbox, S-C02 Approval rules
- **Test samples:** Rule matrix by amount and cost centre; Delegation and absence cases
- **Effort:** to be estimated in task 2

### C02. Supplier bank-account change control

A change to a supplier's bank account needs a verification note and two approvals; payments to that supplier are held for a set period afterwards.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-21
- **Requires:** A2, C01. **Uses if present:** none
- **Client must supply:** Verification procedure (call-back, letter); Hold period
- **Events in:** supplier.bank_changed. **Events out:** supplier.bank_approved, supplier.bank_rejected
- **Permissions:** `finance.suppliers.bank_request`, `finance.suppliers.bank_approve`
- **Settings:** hold period days, required approvers. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C03 Supplier bank-account changes
- **Test samples:** Requester cannot approve own change; Payment hold enforced
- **Effort:** to be estimated in task 2

### C03. Invoice types and credit-note chains

Credit notes, corrective and storno invoices, advance invoices with final settlement, pro forma (not posted); chains link to the original and net the balance.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-22
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** How often each type occurs; Accountant's handling rules for advances
- **Events in:** invoice.confirmed. **Events out:** invoice.related, balance.adjusted
- **Permissions:** `finance.invoices.relate`
- **Settings:** type list, netting rules. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C04 Credit notes and corrections
- **Test samples:** Credit-note chain totals; Advance then final invoice
- **Effort:** to be estimated in task 2

### C04. Duplicate invoice detection

Flags the same supplier and number, or same amount and date, before approval; reviewer keeps, merges or rejects.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-23
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** Duplicate definition agreed with accountants
- **Events in:** invoice.extracted. **Events out:** duplicate.flagged, duplicate.resolved
- **Permissions:** `finance.duplicates.resolve`
- **Settings:** match keys, tolerance. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C05 Duplicate invoices
- **Test samples:** Same invoice by email and by upload; Near-duplicate with changed date
- **Effort:** to be estimated in task 2

### C05. Coding: GL accounts, cost centres, VAT codes

Coding lines on each invoice with GL account, cost centre, project and VAT code; suggestions learned from the supplier's history.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-26
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** Chart of accounts; Cost-centre list; VAT code list; Coding rules the accountant uses today
- **Events in:** invoice.confirmed. **Events out:** invoice.coded
- **Permissions:** `finance.coding.edit`, `finance.coding.configure`
- **Settings:** chart of accounts, cost centres, VAT codes, default coding per supplier. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C06 Accounts and cost centres
- **Test samples:** Coding suggestions on a year of history; Split coding totals
- **Effort:** to be estimated in task 2

### C06. Exchange rates and HUF equivalents

Daily central-bank rates stored; HUF equivalent computed per invoice on the date rule the accountant sets.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-29
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** Rate-date rule (issue, performance or payment date); Rounding rule
- **Events in:** invoice.confirmed. **Events out:** rate.updated
- **Permissions:** `finance.rates.view`, `finance.rates.override`
- **Settings:** rate source, date rule, rounding. **Secrets:** none
- **Adapter contract:** ExchangeRateSource (section 9)
- **UI slots:** S-C07 Exchange rates
- **Test samples:** Weekend and holiday rate lookups; Rounding cases
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C07. Payment proposals and batch approval

Proposals by due date, partial payments, batch grouping, four-eyes batch approval; works with the B4 export when present.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-27
- **Requires:** A2. **Uses if present:** C01, B4
- **Client must supply:** Payment run frequency; Batch approval limit; Partial-payment practice
- **Events in:** invoice.approved. **Events out:** payment.batch_created, payment.batch_approved
- **Permissions:** `finance.batches.create`, `finance.batches.approve`
- **Settings:** run schedule, batch limit, approver count. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C08 Payment proposals, S-C09 Payment batch approval
- **Test samples:** Batch limit and two-person rule; Partial payment balances
- **Effort:** to be estimated in task 2

### C08. Bank statement import and reconciliation

Import CAMT.053, MT940 or CSV; match statement lines to invoices and batches automatically; unmatched lines go to review.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-28
- **Requires:** A2. **Uses if present:** C07, B4
- **Client must supply:** Statement file samples from each bank account; Format used (CAMT.053, MT940 or CSV)
- **Events in:** none. **Events out:** statement.imported, payment.confirmed, statement.line_unmatched
- **Permissions:** `finance.statements.import`, `finance.statements.reconcile`
- **Settings:** bank accounts, format per account, matching tolerance. **Secrets:** none
- **Adapter contract:** StatementParser (section 9)
- **UI slots:** S-C10 Bank statements and reconciliation
- **Test samples:** Statements with partial, merged and foreign-currency lines
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C09. NAV cross-check and missing-invoice alerts

Compares NAV data with the reviewed invoice; flags mismatches and invoices present in NAV that were never received.

- **Tier:** R2 recommended later. **Area:** Finance. **Plan refs:** FI-24
- **Requires:** A2, B2. **Uses if present:** none
- **Client must supply:** Tolerance rules; Who investigates missing invoices
- **Events in:** nav.invoice_imported, invoice.confirmed. **Events out:** navcheck.mismatch, navcheck.missing
- **Permissions:** `finance.navcheck.view`, `finance.navcheck.resolve`
- **Settings:** tolerance, alert owner. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C11 NAV cross-check
- **Test samples:** Matching and mismatch cases from NAV sandbox
- **Effort:** to be estimated in task 2

### C10. Bookkeeping export

Exports coded, approved invoices in the accounting software's import format with links to stored documents; exported records are locked. One adapter per accounting package.

- **Tier:** R1 recommended. **Area:** Finance. **Plan refs:** FI-30
- **Requires:** A2, C05. **Uses if present:** C06
- **Client must supply:** Accounting software name and version; Import format specification and a sample; Mapping of accounts and VAT codes; Accountant contact
- **Events in:** invoice.coded. **Events out:** export.created, export.failed
- **Permissions:** `finance.export.run`, `finance.export.view`
- **Settings:** target software, export schedule, field mapping. **Secrets:** accounting API credentials (if API)
- **Adapter contract:** AccountingExporter (section 9)
- **UI slots:** S-C12 Bookkeeping export
- **Test samples:** Round-trip import into the accounting software test company
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C11. Period lock and AP reports

Lock periods after export; changes after lock need a reason and a corrective export. Reports: AP ageing, due list, supplier balance, spend by cost centre, input-VAT summary.

- **Tier:** R2 recommended later. **Area:** Finance. **Plan refs:** FI-31, FI-32
- **Requires:** A2, C10. **Uses if present:** none
- **Client must supply:** Period close calendar; Report layouts the manager uses today
- **Events in:** export.created. **Events out:** period.locked, period.reopened
- **Permissions:** `finance.periods.lock`, `finance.reports.view`
- **Settings:** period calendar, report layouts. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C13 Accounting periods, S-C14 AP reports
- **Test samples:** Reports reconcile with ledger for one closed month
- **Effort:** to be estimated in task 2

### C12. Report builder and scheduled reports

Build saved reports from any list, schedule them by email, export CSV/XLSX/PDF.

- **Tier:** R2 recommended later. **Area:** Platform. **Plan refs:** SH-15
- **Requires:** A1. **Uses if present:** none
- **Client must supply:** Top five reports management wants weekly
- **Events in:** none. **Events out:** report.sent
- **Permissions:** `platform.reports.build`, `platform.reports.subscribe`
- **Settings:** schedules, recipients. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C15 Report builder and schedules
- **Test samples:** Scheduled run with masked fields
- **Effort:** to be estimated in task 2

### C13. Supplier VAT number validation

Validates EU VAT numbers and Hungarian tax numbers when a supplier is created or changed.

- **Tier:** R2 recommended later. **Area:** Finance. **Plan refs:** FI-20
- **Requires:** A2. **Uses if present:** none
- **Client must supply:** Decision on blocking vs warning for invalid numbers
- **Events in:** supplier.created. **Events out:** supplier.vat_checked
- **Permissions:** `finance.suppliers.validate`
- **Settings:** block or warn. **Secrets:** none
- **Adapter contract:** VatValidator (section 9)
- **UI slots:** Panel inside S-A19 Supplier detail
- **Test samples:** Valid, invalid and service-unavailable cases
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C14. Sales invoices and receivables

Customer master, outgoing invoices and receivables tracking. Needs confirmation that Astex wants this inside the platform rather than in its invoicing tool.

- **Tier:** O optional. **Area:** Finance. **Plan refs:** FI-34
- **Requires:** A2, C05. **Uses if present:** C08
- **Client must supply:** Current invoicing tool; Volume of outgoing invoices; NAV reporting responsibility
- **Events in:** none. **Events out:** salesinvoice.issued, receivable.paid
- **Permissions:** `finance.sales.manage`
- **Settings:** numbering, templates, payment terms. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C17 Sales invoices
- **Test samples:** Issue, send, receive payment cycle
- **Effort:** to be estimated in task 2

### C15. Cash-flow forecast

Forecast from approved payables, planned batches and, if C14 is present, receivables.

- **Tier:** O optional. **Area:** Finance. **Plan refs:** FI-35
- **Requires:** C07. **Uses if present:** C14
- **Client must supply:** Opening balances; Forecast horizon
- **Events in:** none. **Events out:** none
- **Permissions:** `finance.forecast.view`
- **Settings:** horizon weeks. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C18 Cash-flow forecast
- **Test samples:** Forecast vs actual for one month
- **Effort:** to be estimated in task 2

### C16. Expense claims and petty cash

Employee expense claims with receipts, approval and reimbursement list; petty-cash register.

- **Tier:** O optional. **Area:** Finance. **Plan refs:** FI-36
- **Requires:** A2. **Uses if present:** C01, C05
- **Client must supply:** Expense policy; Who claims today
- **Events in:** none. **Events out:** claim.submitted, claim.approved
- **Permissions:** `finance.expenses.submit`, `finance.expenses.approve`
- **Settings:** categories, limits. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C19 Expense claims
- **Test samples:** Claim with split categories
- **Effort:** to be estimated in task 2

### C20. Employment history and customer assignments

Effective-dated employment records (position, site, employer entity, annexes) and customer assignments with documents. Conditional on Astex placing workers at customer sites.

- **Tier:** R1 recommended. **Area:** HR. **Plan refs:** HR-20, HR-21
- **Requires:** A3. **Uses if present:** none
- **Client must supply:** Confirmation whether Astex is a staffing agency; Customer and site list; Assignment document types
- **Events in:** person.approved. **Events out:** employment.changed, assignment.started, assignment.ended
- **Permissions:** `hr.employment.view`, `hr.employment.edit`
- **Settings:** assignment types, required documents per customer. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C20 Employment and assignments
- **Test samples:** Overlapping and back-dated changes
- **Effort:** to be estimated in task 2

### C21. Offboarding workflow

End date and reason, checklist (access, equipment, accommodation, final documents), retention clock started.

- **Tier:** R1 recommended. **Area:** HR. **Plan refs:** HR-22
- **Requires:** A3. **Uses if present:** C20
- **Client must supply:** Current leaver steps and owners; Final documents list
- **Events in:** none. **Events out:** person.left, offboarding.completed
- **Permissions:** `hr.offboarding.manage`
- **Settings:** checklist template, retention trigger. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C21 Offboarding
- **Test samples:** Leaver with open assignment and open documents
- **Effort:** to be estimated in task 2

### C22. Medical, training and licence tracking

Medical fitness, safety training and licences (for example forklift) with expiry control on the same reminder engine as identity documents.

- **Tier:** R2 recommended later. **Area:** HR. **Plan refs:** HR-24
- **Requires:** A3. **Uses if present:** none
- **Client must supply:** List of record types and validity periods; Who issues them
- **Events in:** none. **Events out:** record.expiring
- **Permissions:** `hr.records.manage`
- **Settings:** record types, validity, required by job type. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C22 Medical, training and licences
- **Test samples:** Expiry windows per record type
- **Effort:** to be estimated in task 2

### C23. Permit renewal cases

Renewal case with application date, receipt, pending decision and outcome so a permit under renewal does not show as plainly expired.

- **Tier:** R2 recommended later. **Area:** HR. **Plan refs:** HR-25
- **Requires:** A3. **Uses if present:** none
- **Client must supply:** Current renewal procedure; Who files with authorities
- **Events in:** document.expiring. **Events out:** renewal.opened, renewal.decided
- **Permissions:** `hr.renewals.manage`
- **Settings:** case states, lead times. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C23 Permit renewals
- **Test samples:** Renewal filed before expiry; decision after expiry
- **Effort:** to be estimated in task 2

### C24. Statutory registration tracking

Records employer registrations and authority notifications with date and reference. Tracked, not submitted automatically.

- **Tier:** R2 recommended later. **Area:** HR. **Plan refs:** HR-23
- **Requires:** A3. **Uses if present:** none
- **Client must supply:** List of required notifications and deadlines (from Astex's adviser)
- **Events in:** person.approved, person.left. **Events out:** registration.due, registration.confirmed
- **Permissions:** `hr.registrations.manage`
- **Settings:** registration types, deadlines. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C24 Statutory registrations
- **Test samples:** Deadline calculation for joiner and leaver
- **Effort:** to be estimated in task 2

### C25. Template library and bilingual contracts

Many templates (contract, annex, notice, certificate), bilingual layouts, legal approval and version control.

- **Tier:** R2 recommended later. **Area:** HR. **Plan refs:** HR-26
- **Requires:** A3. **Uses if present:** none
- **Client must supply:** Template set approved by Astex's counsel; Languages needed
- **Events in:** none. **Events out:** template.approved, document.generated
- **Permissions:** `hr.templates.author`, `hr.templates.approve`
- **Settings:** templates, languages, merge fields. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C25 Template library
- **Test samples:** Merge-field coverage per template
- **Effort:** to be estimated in task 2

### C26. Payroll export

Joiners, changes and leavers in the payroll provider's import format, one adapter per provider.

- **Tier:** R2 recommended later. **Area:** HR. **Plan refs:** HR-27
- **Requires:** A3. **Uses if present:** C20, C21
- **Client must supply:** Payroll provider; Import format and sample; Field mapping
- **Events in:** person.approved, employment.changed, person.left. **Events out:** payroll.exported
- **Permissions:** `hr.payroll.export`
- **Settings:** provider, field mapping, export schedule. **Secrets:** provider credentials (if API)
- **Adapter contract:** PayrollExporter (section 9)
- **UI slots:** S-C26 Payroll export
- **Test samples:** Test import into payroll sandbox
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C27. Workforce reports

Headcount by nationality, entity, site and customer; permit expiry forecast; onboarding funnel.

- **Tier:** R2 recommended later. **Area:** HR. **Plan refs:** HR-28
- **Requires:** A3. **Uses if present:** C20, C12
- **Client must supply:** Reports management asks for today
- **Events in:** none. **Events out:** none
- **Permissions:** `hr.reports.view`
- **Settings:** report layouts. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C27 Workforce reports
- **Test samples:** Figures reconcile with HR dashboard
- **Effort:** to be estimated in task 2

### C28. Employee self-service portal

Workers sign in to see their own data, update address, upload renewed documents and see expiry reminders.

- **Tier:** O optional. **Area:** HR. **Plan refs:** HR-29
- **Requires:** A3. **Uses if present:** C29
- **Client must supply:** Decision on sign-in method for workers; Which fields workers may edit
- **Events in:** document.expiring. **Events out:** self.update_submitted
- **Permissions:** `self.profile.view`, `self.profile.edit`
- **Settings:** editable fields, sign-in method. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C28 Employee portal home
- **Test samples:** Worker sees only own data
- **Effort:** to be estimated in task 2

### C29. SMS and WhatsApp reminders

Reminders to workers in their language through a messaging gateway, with consent record.

- **Tier:** O optional. **Area:** HR. **Plan refs:** HR-30
- **Requires:** A3. **Uses if present:** none
- **Client must supply:** Messaging provider account; Consent wording; Languages
- **Events in:** document.expiring, person.invited. **Events out:** message.sent, message.failed
- **Permissions:** `hr.messaging.send`
- **Settings:** templates per language, quiet hours. **Secrets:** messaging provider key
- **Adapter contract:** MessagingProvider (section 9)
- **UI slots:** S-C29 Messaging (SMS / WhatsApp)
- **Test samples:** Delivery failure handling
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C30. Accommodation records

Address, room, dates and cost per worker with occupancy overview.

- **Tier:** O optional. **Area:** HR. **Plan refs:** HR-31
- **Requires:** A3. **Uses if present:** C21
- **Client must supply:** Whether Astex provides housing; Cost recovery rules
- **Events in:** person.left. **Events out:** accommodation.vacated
- **Permissions:** `hr.accommodation.manage`
- **Settings:** properties, rooms. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C30 Accommodation
- **Test samples:** Overlapping stays
- **Effort:** to be estimated in task 2

### C31. Qualified e-signature

Sign generated contracts electronically instead of by paper scan.

- **Tier:** O optional. **Area:** HR. **Plan refs:** HR-12
- **Requires:** C25. **Uses if present:** none
- **Client must supply:** Signature provider; Legal view on required signature level
- **Events in:** document.generated. **Events out:** document.signed
- **Permissions:** `hr.signature.send`
- **Settings:** provider, signing order. **Secrets:** signature provider key
- **Adapter contract:** SignatureProvider (section 9)
- **UI slots:** S-C31 E-signature status
- **Test samples:** Signing flow with two signers
- **Effort:** to be estimated in task 2 (framework and adapter separately)

### C40. Multi-entity management

Screens for several legal entities with per-entity roles, numbering, bank accounts and reporting. The data model is in A1.

- **Tier:** R2 recommended later. **Area:** Platform. **Plan refs:** SH-01
- **Requires:** A1. **Uses if present:** none
- **Client must supply:** Number of legal entities and how they share suppliers and staff
- **Events in:** none. **Events out:** entity.created
- **Permissions:** `platform.entities.manage`
- **Settings:** entities, entity-scoped roles. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C40 Legal entities
- **Test samples:** Entity isolation in lists, search and export
- **Effort:** to be estimated in task 2

### C41. Custom fields

Administrators add validated custom fields to people, suppliers, invoices and cases without a release.

- **Tier:** O optional. **Area:** Platform. **Plan refs:** SH-20
- **Requires:** A1. **Uses if present:** none
- **Client must supply:** First list of extra fields
- **Events in:** none. **Events out:** field.defined
- **Permissions:** `platform.fields.manage`
- **Settings:** field definitions. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C41 Custom fields
- **Test samples:** Field appears in forms, lists, search and export
- **Effort:** to be estimated in task 2

### C42. Data-subject request tooling

Export or correct all data about one person within a working day, with retention exceptions flagged.

- **Tier:** R2 recommended later. **Area:** Platform. **Plan refs:** SH-21
- **Requires:** A1, A3. **Uses if present:** none
- **Client must supply:** DPO procedure for requests
- **Events in:** none. **Events out:** dsr.completed
- **Permissions:** `platform.dsr.manage`
- **Settings:** export format, retention exceptions. **Secrets:** none
- **Adapter contract:** none
- **UI slots:** S-C42 Data-subject requests
- **Test samples:** Full export for a person with documents and history
- **Effort:** to be estimated in task 2


## 5. Dependencies and packages

### Module dependencies

| Module | Name | Requires | Uses if present | Required by |
| --- | --- | --- | --- | --- |
| B1 | Inbound email intake | A2 | - | - |
| B2 | NAV Online Invoice import | A2 | - | C09 |
| B3 | EKÁER declarations | A2 | B2 | - |
| B4 | Bank payment file export | A2 | C07 | B6 |
| B5 | Bank notification email parsing | A2 | B1 | - |
| B6 | Direct bank API | B4, C08 | C07 | - |
| B7 | Alternative OCR and AI providers | A1 | - | - |
| C01 | Approval workflows | A2 | - | C02 |
| C02 | Supplier bank-account change control | A2, C01 | - | - |
| C03 | Invoice types and credit-note chains | A2 | - | - |
| C04 | Duplicate invoice detection | A2 | - | - |
| C05 | Coding: GL accounts, cost centres, VAT codes | A2 | - | C10, C14 |
| C06 | Exchange rates and HUF equivalents | A2 | - | - |
| C07 | Payment proposals and batch approval | A2 | C01, B4 | C15 |
| C08 | Bank statement import and reconciliation | A2 | C07, B4 | B6 |
| C09 | NAV cross-check and missing-invoice alerts | A2, B2 | - | - |
| C10 | Bookkeeping export | A2, C05 | C06 | C11 |
| C11 | Period lock and AP reports | A2, C10 | - | - |
| C12 | Report builder and scheduled reports | A1 | - | - |
| C13 | Supplier VAT number validation | A2 | - | - |
| C14 | Sales invoices and receivables | A2, C05 | C08 | - |
| C15 | Cash-flow forecast | C07 | C14 | - |
| C16 | Expense claims and petty cash | A2 | C01, C05 | - |
| C20 | Employment history and customer assignments | A3 | - | - |
| C21 | Offboarding workflow | A3 | C20 | - |
| C22 | Medical, training and licence tracking | A3 | - | - |
| C23 | Permit renewal cases | A3 | - | - |
| C24 | Statutory registration tracking | A3 | - | - |
| C25 | Template library and bilingual contracts | A3 | - | C31 |
| C26 | Payroll export | A3 | C20, C21 | - |
| C27 | Workforce reports | A3 | C20, C12 | - |
| C28 | Employee self-service portal | A3 | C29 | - |
| C29 | SMS and WhatsApp reminders | A3 | - | - |
| C30 | Accommodation records | A3 | C21 | - |
| C31 | Qualified e-signature | C25 | - | - |
| C40 | Multi-entity management | A1 | - | - |
| C41 | Custom fields | A1 | - | - |
| C42 | Data-subject request tooling | A1, A3 | - | - |

### Proposed packages

These are starting points for the decision sheet in the proposal, not prices.

| Package | What it gives Astex | Adds these modules |
| --- | --- | --- |
| **Core** | Part A only. Manual upload, OCR, review, translation, register, cases, matching, manual payment status; HR onboarding and expiry control | A1, A2, A3 |
| **Recommended** | Core plus everything we expect Astex to need at go-live: email intake, NAV, EKÁER, bank file, approvals, coding, rates, payment batches, statement reconciliation, accounting export, employment history, offboarding | B1, B2, B3, B4, C01, C02, C03, C04, C05, C06, C07, C08, C10, C20, C21 |
| **Extended** | Recommended plus the second wave: NAV cross-check, period lock and reports, report builder, VAT checks, HR records, renewals, templates, payroll export, workforce reports, multi-entity, data-subject tooling | C09, C11, C12, C13, C22, C23, C24, C25, C26, C27, C40, C42 |
| **Full** | Everything in this backlog, including optional modules | B5, B6, B7, C14, C15, C16, C28, C29, C30, C31, C41 |

Modules per package: Core 3, Recommended 18, Extended 30, Full 41. All packages are dependency-complete (checked by `tools/build_docs.py`).

## 6. What Astex must supply

Most delays on earlier projects came from missing inputs. This table is the client's homework list, grouped by module.

| Module | Name | Astex must supply |
| --- | --- | --- |
| B1 | Inbound email intake | Dedicated address on Astex's domain; DNS or mail-forwarding change by Astex IT; Sender allow-list; Maximum attachment size policy |
| B2 | NAV Online Invoice import | NAV technical user, signature key and exchange key; Astex tax number; Test environment access; Decision: NAV data as primary capture source or cross-check only |
| B3 | EKÁER declarations | EKÁER API access and credentials; Which goods and transports trigger EKÁER; Sample declarations with their related invoices and delivery notes; Who finalises today |
| B4 | Bank payment file export | Bank name; Import file specification (HUF domestic format; SEPA pain.001 for EUR); Sample file accepted by the bank; Who authorises payments at the bank |
| B5 | Bank notification email parsing | Bank notification email samples (at least 30); Agreement on one email format |
| B6 | Direct bank API | Bank API agreement; Sandbox access; Strong-authentication method for payment signing; Bank's rate limits and SLAs |
| B7 | Alternative OCR and AI providers | Provider contract, DPA and EU processing confirmation; Cost cap; Labelled sample for comparison |
| C01 | Approval workflows | Approval matrix: who approves what, limits, deputies |
| C02 | Supplier bank-account change control | Verification procedure (call-back, letter); Hold period |
| C03 | Invoice types and credit-note chains | How often each type occurs; Accountant's handling rules for advances |
| C04 | Duplicate invoice detection | Duplicate definition agreed with accountants |
| C05 | Coding: GL accounts, cost centres, VAT codes | Chart of accounts; Cost-centre list; VAT code list; Coding rules the accountant uses today |
| C06 | Exchange rates and HUF equivalents | Rate-date rule (issue, performance or payment date); Rounding rule |
| C07 | Payment proposals and batch approval | Payment run frequency; Batch approval limit; Partial-payment practice |
| C08 | Bank statement import and reconciliation | Statement file samples from each bank account; Format used (CAMT.053, MT940 or CSV) |
| C09 | NAV cross-check and missing-invoice alerts | Tolerance rules; Who investigates missing invoices |
| C10 | Bookkeeping export | Accounting software name and version; Import format specification and a sample; Mapping of accounts and VAT codes; Accountant contact |
| C11 | Period lock and AP reports | Period close calendar; Report layouts the manager uses today |
| C12 | Report builder and scheduled reports | Top five reports management wants weekly |
| C13 | Supplier VAT number validation | Decision on blocking vs warning for invalid numbers |
| C14 | Sales invoices and receivables | Current invoicing tool; Volume of outgoing invoices; NAV reporting responsibility |
| C15 | Cash-flow forecast | Opening balances; Forecast horizon |
| C16 | Expense claims and petty cash | Expense policy; Who claims today |
| C20 | Employment history and customer assignments | Confirmation whether Astex is a staffing agency; Customer and site list; Assignment document types |
| C21 | Offboarding workflow | Current leaver steps and owners; Final documents list |
| C22 | Medical, training and licence tracking | List of record types and validity periods; Who issues them |
| C23 | Permit renewal cases | Current renewal procedure; Who files with authorities |
| C24 | Statutory registration tracking | List of required notifications and deadlines (from Astex's adviser) |
| C25 | Template library and bilingual contracts | Template set approved by Astex's counsel; Languages needed |
| C26 | Payroll export | Payroll provider; Import format and sample; Field mapping |
| C27 | Workforce reports | Reports management asks for today |
| C28 | Employee self-service portal | Decision on sign-in method for workers; Which fields workers may edit |
| C29 | SMS and WhatsApp reminders | Messaging provider account; Consent wording; Languages |
| C30 | Accommodation records | Whether Astex provides housing; Cost recovery rules |
| C31 | Qualified e-signature | Signature provider; Legal view on required signature level |
| C40 | Multi-entity management | Number of legal entities and how they share suppliers and staff |
| C41 | Custom fields | First list of extra fields |
| C42 | Data-subject request tooling | DPO procedure for requests |

## 7. Module contract standard

Every B and C module is delivered as a plugin that follows this standard. It is deliberately independent of the technology stack, which is not yet decided.

### 7.1 Module manifest

Each module ships a manifest the module host reads:

```yaml
id: C01                      # unique, matches this backlog
name: Approval workflows
version: 1.0.0
requires: [A2]               # must be enabled first
optional: []                 # used if present
permissions:                 # added to the permission registry (EP-09)
  - finance.approvals.act
  - finance.approvals.configure
menu:                        # slots in the shell (EP-01)
  - slot: finance.approvals.inbox
    label: { en: Approval inbox, hu: Jóváhagyási postafiók }
    route: /finance/approvals
    permission: finance.approvals.act
settings: schema/settings.json     # JSON Schema; secrets are referenced by name, never stored here
events:
  consumes: [invoice.confirmed]
  produces: [invoice.approved, invoice.rejected, approval.escalated]
tasks: [approval.pending]          # task types added to the inbox (EP-10)
templates: [approval-request, approval-reminder]   # notification templates (EP-11)
adapter: null                      # or the adapter interface name (section 9)
health: /health                    # reports ok, degraded or failed
migrations: migrations/            # the module owns its tables
```

### 7.2 Rules every module follows

1. **Own its data.** A module creates its own tables, prefixed with its ID. It reads core data through published views and changes core data only through the declared extension points (EP-01 to EP-12).
2. **Talk through events.** A module never calls another module directly. It publishes and consumes events from the catalog in section 8.
3. **Enable, disable, never delete.** Enabling checks that required modules are on. Disabling is blocked while another enabled module requires it. A disabled module's data stays, read-only, and is still covered by retention rules.
4. **Everything audited and permissioned.** Each action is checked against a registered permission and written to the audit trail with the module ID as the source.
5. **Failures are visible.** A failed job creates a task with an owner. Retries are idempotent, and an adapter error carries a code and a `retryable` flag.
6. **Tested against samples.** A module is accepted only when it passes its own test samples (listed on each card) supplied by Astex or recorded from the outside system.
7. **Both languages.** Labels, emails and validation messages exist in English and Hungarian.

## 8. Event catalog

Every event uses one envelope:

```json
{ "id": "uuid", "type": "invoice.confirmed", "version": 1, "occurredAt": "ISO-8601",
  "entityId": "legal entity", "actor": "user or system", "correlationId": "uuid",
  "payload": { } }
```

| Event | Produced by | Payload essentials | Typical consumers |
| --- | --- | --- | --- |
| invoice.received | A2, B1 | fileId, source (upload, email), receivedAt | A2 OCR job |
| invoice.extracted | A2 | invoiceId, fields with confidence | C04 duplicate check |
| invoice.confirmed | A2 | invoiceId, supplierId, gross, currency | C01, C05, C06, C09, B4 |
| invoice.translated | A2 | invoiceId, language, textId | search index |
| invoice.approved | C01 | invoiceId, approvers | C07, B4 |
| invoice.coded | C05 | invoiceId, coding lines | C10 |
| invoice.paid | A2, C08 | invoiceId, amount, date, reference | A2 case state, C15 |
| case.ready | A2 | caseId, completeness | B3 |
| supplier.bank_changed | A2 | supplierId, old, new | C02 |
| payment.batch_created / _approved / _exported | C07, B4 | batchId, invoice ids, total | B4, B6, C08 |
| payment.confirmed | C08 | batchId or invoiceId, statementLineId | A2, C07 |
| nav.invoice_imported | B2 | navInvoiceId, matchedInvoiceId or null | C09 |
| ekaer.finalised | B3 | caseId, declarationRef, time | A2 case timeline |
| export.created | C10 | exportId, period, invoice ids | C11 |
| person.invited / .submitted / .approved / .left | A3, C21 | personId | C20, C24, C26, C29 |
| document.expiring / .expired / .replaced | A3 | personId, documentType, expiryDate, window | C23, C28, C29 |
| employment.changed | C20 | personId, effective date, fields | C26 |
| job.failed | A1 | jobId, class, owner | task inbox |

## 9. Adapter interfaces

Adapters are the only code that knows about an outside system. They are shown in TypeScript-style notation for readability; the signatures are the contract, not the language. Each is estimated separately once the specification for that system is supplied.

```ts
type Result<T> =
  | { ok: true; value: T }
  | { ok: false; error: { code: string; retryable: boolean; message: string } };

// A1, B7
interface OcrProvider {
  extract(file: FileRef, docType: DocType, opts?: { languages?: string[] }): Promise<Result<{
    fields: { name: string; value: string; confidence: number; page: number; box?: BBox }[];
    fullText: string; language?: string; raw: unknown }>>;
}
interface TranslationProvider {
  translate(text: string, from: string | "auto", to: "en"): Promise<Result<{ text: string; detectedFrom: string }>>;
}

// B1
interface InboundMailAdapter {
  parse(raw: RawMessage): Promise<Result<{ messageId: string; sender: string; subject: string; attachments: FileRef[] }>>;
}

// B2
interface NavInvoiceAdapter {
  queryInvoices(range: DateRange, page: number): Promise<Result<{ invoices: NavInvoice[]; nextPage?: number }>>;
  health(): Promise<Result<{ environment: "test" | "production" }>>;
}

// B3
interface EkaerAdapter {
  create(d: EkaerDeclaration): Promise<Result<{ ref: string }>>;
  modify(ref: string, patch: Partial<EkaerDeclaration>): Promise<Result<void>>;
  finalise(ref: string): Promise<Result<{ finalisedAt: string }>>;
  status(ref: string): Promise<Result<EkaerStatus>>;
}

// B4, B6
interface BankPaymentFormat {
  id: string; // e.g. "hu-domestic-huf", "sepa-pain001"
  validate(batch: PaymentBatch): Result<void>;
  generate(batch: PaymentBatch): Result<{ file: FileRef; checksum: string }>;
}
interface BankApiAdapter {
  submit(batch: PaymentBatch): Promise<Result<{ bankRef: string }>>;
  fetchStatements(account: string, since: string): Promise<Result<StatementLine[]>>;
  authStatus(): Promise<Result<{ validUntil: string }>>;
}

// B5, C08
interface BankNoticeParser { parse(email: ParsedEmail): Result<PaymentNotice | null>; }
interface StatementParser {
  id: string; // "camt053", "mt940", "csv-<bank>"
  parse(file: FileRef): Result<StatementLine[]>;
}

// C06
interface ExchangeRateSource { rate(currency: string, date: string): Promise<Result<{ rate: string; date: string }>>; }

// C10
interface AccountingExporter {
  id: string; // one per accounting package
  mapping(): MappingSchema;
  export(period: Period, entries: CodedInvoice[]): Promise<Result<{ file?: FileRef; externalRef?: string; rejected: { invoiceId: string; reason: string }[] }>>;
}

// C13
interface VatValidator { validate(country: string, number: string): Promise<Result<{ valid: boolean; name?: string; address?: string }>>; }

// C26
interface PayrollExporter { export(changes: PayrollChange[], period: Period): Promise<Result<{ file: FileRef }>>; }

// C29
interface MessagingProvider { send(to: string, templateId: string, vars: Record<string, string>, lang: string): Promise<Result<{ messageId: string }>>; }

// C31
interface SignatureProvider {
  send(doc: FileRef, signers: Signer[]): Promise<Result<{ envelopeId: string }>>;
  status(envelopeId: string): Promise<Result<SignatureStatus>>;
}
```

Amounts are decimal strings, never floating-point numbers. Dates are ISO 8601.

## 10. Parked: not planned unless Astex asks

- **Leave, absence and timesheets** (HR-32): Belongs with payroll or a time system; data model leaves room for it.
- **Conversational or semantic AI search** (B p.8): Named by Astex as a separate extension; priced only on request.

## 11. How this feeds the next tasks

- **Task 2 (estimate).** Each module in `data/modules.json` gets low, likely and high hours for framework and adapter, at 25 USD per hour and two developers. Part A epics are estimated story by story from the tables above.
- **Task 3 (screens).** The screen list and clickable mockup read the same module registry, so every screen is tagged A, B or C and can be hidden when its module is not in the chosen package.

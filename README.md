# Balancd: AI-Assisted Month-End Close Automation
 
> **Graduate Direct Project** · Northwest Missouri State University · Aug 2025 - May 2026  
> Source code is private (client-owned). This repository documents the project's architecture, features, and my role.  
> **[View the official product landing page →](https://www.vossaiconsulting.com/balancd/)**
 
---
 
## What It Is
 
Balancd is a privacy-first, AI-assisted bookkeeping desktop application built to compress the month-end close workflow that QuickBooks and Xero don't address (reconciliation, accruals, intercompany matches, journal entries, and close-the-books) into a single local-first platform. Accounting firms keep their data on-premises; Balancd gives them their nights back.
 
Built as a working architectural prototype, Balancd is currently in pre-seed capital formation under [Voss AI Consulting](https://www.vossaiconsulting.com).
 
---
 
## My Role
 
**Project Manager & Lead Developer** on a three-person team, year-long capstone, 477 total commits across the team.
 
**What I built:**
- Project management and team coordination
- Most of the frontend (React/TypeScript)
- Two-factor authentication (TOTP), email activation codes, and a password-reset flow
- AES-256-GCM encryption for stored API keys
- The audit trail (per-transaction history and linked transactions)
- The five-report financial suite with CSV, QuickBooks, and Xero export
- The month-end close workflow
- The AI classification workflow (classify, batch-classify, and apply-suggestion, with user review before anything is applied)
**What my teammates built:** One teammate built the login system and later restructured the backend (versioned SQL migrations, modular Rust layers, the Rust test suite, and validation of AI responses). Another handled documentation.
 
**Tools:** I used AI-assisted development tools (Claude Code) throughout, reviewing and directing the work.
 
---
 
## Tech Stack
 
| Layer | Technology |
|---|---|
| Desktop runtime | Tauri (Rust + Web) |
| Frontend | React, TypeScript, Vite, TanStack Query |
| Backend | Rust, SQLx, PostgreSQL |
| AI | Anthropic Claude API |
| Auth | TOTP 2FA (RFC 6238), bcrypt, AES-256-GCM |
| Export | ExcelJS, QuickBooks / Xero format support |
| Standards | IEEE 830, GAAP/IFRS, OWASP, RFC 6238 |
 
**Language breakdown (per repository):** TypeScript 43.6% · Rust 40.1% · CSS 15.6% · PL/pgSQL 0.4%
 
---
 
## Core Features
 
### Accounting & Close Workflow
- **Double-entry ledger** with full debit/credit transaction management
- **Month-End Close workflow** - structured close cycle with task assignment, sign-off stages, and audit trail
- **Automated reconciliation** with AI-suggested matches
- **Recurring journal entry templates** with rules engine
- **Intercompany matching** with deterministic rules
- **Flux analysis** with AI-assisted variance commentary
### Financial Reporting (5-report suite)
- Balance Sheet
- Income Statement
- Cash Flow Statement
- Trial Balance
- General Ledger
### Data & Integration
- File import - Excel (.xlsx) and CSV
- QuickBooks / Xero format export
- PBC (Prepared by Client) request portal with audit trail
- PostgreSQL schema with full double-entry accounting primitives
### Security & Multi-Tenancy
- Multi-tenant organization management, with separate firm and client data
- Two-factor authentication (TOTP, RFC 6238 compliant)
- AES-256-GCM data encryption at rest
- Full audit trail on all financial operations
- Local-first architecture, so client data never leaves the firm's perimeter
### AI Integration
- Anthropic Claude API for automated audit assistance
- AI-assisted reconciliation suggestions
- Journal entry recommendations
- Flux commentary generation
---
 
## Architecture Overview
 
Balancd uses **Tauri** as its desktop runtime, pairing a **Rust** backend with a **React/TypeScript** frontend. This local-first architecture is a deliberate design decision: by running natively on the firm's hardware, client financial data never has to leave the firm's perimeter to be processed, a structural privacy advantage over cloud-only competitors.
 
```
┌─────────────────────────────────────────────────┐
│               React / TypeScript UI              │
│         (Vite · TanStack Query · CSS)            │
└────────────────────┬────────────────────────────┘
                     │  Tauri IPC bridge
┌────────────────────▼────────────────────────────┐
│              Rust Backend (Tauri)                │
│      SQLx · PostgreSQL · bcrypt                  │
│      AES-256-GCM · TOTP · ExcelJS               │
└────────────┬──────────────────┬─────────────────┘
             │                  │
    ┌─────────▼──────┐  ┌───────▼────────┐
    │   PostgreSQL   │  │  Claude API    │
    │  (firm data)   │  │  (AI features) │
    └────────────────┘  └────────────────┘
```
 
---
 
## Key Technical Decisions
 
- **Local-first over cloud-first.** The client's core requirement was data residency, so the team built Balancd as a local-first desktop app on Tauri with a local PostgreSQL database. The database URL is parameterized, so moving to a hosted database is a configuration change, not a rewrite.
- **A human reviews AI output.** Claude's responses are validated against a strict, versioned schema (built by a teammate during the backend restructuring). I built the classify, batch-classify, and apply-suggestion workflow so a user reviews every suggestion before anything is applied.
- **Bring-your-own-key for AI.** Users supply their own Anthropic API key, which I encrypted at rest with AES-256-GCM using a unique nonce per key. This keeps API costs and key control with the firm and keeps credentials out of the codebase.
- **Layered authentication.** On top of the login system a teammate built, I added TOTP two-factor authentication (RFC 6238), email activation codes, and a password-reset flow.
- **Audit trail as a first-class feature.** The users are accountants, so I built per-transaction history and linked transactions so edits and close sign-offs can be traced.
- **A debugging lesson on timestamps.** A TIMESTAMP vs TIMESTAMPTZ mismatch between PostgreSQL and Rust's `DateTime<Utc>` compiled cleanly but caused runtime panics. The team fixed it by casting in queries (`AT TIME ZONE 'UTC'`) and documented the pitfall so it would not recur.
---
 
## Project Context
 
- **Client:** Dr. Rob Voss · [Voss AI Consulting](https://www.vossaiconsulting.com)
- **Institution:** Northwest Missouri State University, School of Computer Science
- **Advisors:** Dr. Ajay Bandi (Primary), Dr. Zhiling Tu (Secondary)
- **Team:** Group 1, 3 members
- **Duration:** Aug 2025 - May 2026 (2 semesters of Graduate Direct Project)
- **Grades:** A in GDP I · A in GDP II
The project was publicly defended and presented to faculty and external reviewers in Spring 2026. Balancd is currently positioned as a pre-seed startup, actively seeking capital and strategic partnerships.
 
---
 
## What This Demonstrates
 
For recruiters and engineers reviewing this project:
 
- **Systems thinking** - local-first architecture with a clear rationale (data residency), not just framework preference
- **Security implementation** - real auth (TOTP, AES-256-GCM), not placeholder login screens
- **AI integration** - Claude API features (classification and reconciliation suggestions) with a human review step before anything is applied
- **Full-stack work** - from the TypeScript UI to Rust command handlers backed by PostgreSQL
- **Project leadership** - PM role on a year-long, client-facing, multi-person engineering project
- **Testing discipline** - a team test suite of 130 passing tests across the TypeScript and Rust code, not "I tested it manually"
- **Domain fluency** - GAAP/IFRS, double-entry accounting, and close workflow, learned and applied during the project
---
 
## Links
 
- [Balancd product landing page](https://www.vossaiconsulting.com/balancd/)
- [My LinkedIn](https://www.linkedin.com/in/wyatt-morgan20/)
- [My GitHub](https://github.com/WyattMorgan20)
- [Source repository](https://github.com/RobVoss-AI/Balancd) *(private, client-owned)*
---
 
# Demos:
 
 
## Dashboard Overview
 
![Dashboard Demo](gifs/loginDashboard.gif)
_Secure login with TOTP two-factor authentication, followed by the main dashboard, displaying real-time account balances and recent transaction activity at a glance._
 
---
 
## Transaction Management
 
![Transaction Workflow Demo](gifs/transaction.gif)
_Double-entry transaction management - creating, categorizing, and reviewing journal entries with full debit/credit ledger tracking. Features AI-assisted classification, transaction linking, document attachments, variance flagging, search, and file import._
 
---
 
## AI-Assisted Audit Features
 
![AI Audit Demo](gifs/financeReconcilliations.gif)
_AI-assisted reconciliation workflow - matching transactions against imported bank data with Claude API-powered suggestions to flag discrepancies and accelerate the close process. Prepared by Client (PBC) Requests and review integration._
 
---
 
## Month End Close
 
![Month End Close Demo](gifs/monthEndClose.gif)
_Structured month-end close workflow - task assignment, stage-by-stage sign-off, and a full audit trail ensuring nothing gets closed without proper review. Projects page manages multi-client engagements._
 
---
 
## Reports
 
![Reports Demo](gifs/reports.gif)
_Five-report financial suite - Balance Sheet, Income Statement, Cash Flow Statement, Trial Balance, and General Ledger - generated from live ledger data and exportable to QuickBooks/Xero formats._
 
---
 
## Business Simulation
 
![Business Simulation Demo](gifs/businessSimulation.gif)
_Organization management - create departments, build teams within them, and define member roles and permissions. Admins can reassign roles, generate invite codes for new users, and manage the full organizational structure of the firm._
 
---
 
## Receivables/Payables/System
 
![Receivables/Payables/System Demo](gifs/receivablesPayablesSystem.gif)
_Accounts receivable and payable management, system configuration, and the Settings page, including API key input for connecting the Anthropic Claude integration that powers Balancd's AI audit features._

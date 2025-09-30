# Project ReplyPilot — Sprint Plan (Windsurf)

## Sprint 0: Foundations & Setup (Week 1-2)
**Goal:** Establish development environment, repo, architecture, and basic connectivity.

- **Epics Covered:** Epic 1 (partial), Epic 6 (partial).
- **Tasks:**
  - Setup project repo in Windsurf.
  - Define CI/CD pipeline (GitHub Actions).
  - Setup base Python backend structure.
  - Implement system logging module (`SystemLog`).
  - Configure database schema (SQLite/Postgres).
  - Setup environment variables for IMAP/SMTP (secure storage).
  - Implement user authentication (OAuth2 + password hash).
- **Deliverables:**
  - Running backend service with DB migrations.
  - Secure connection handling tested with dummy email credentials.

---

## Sprint 1: Email Retrieval & Storage (Week 3-4)
**Goal:** Fetch emails via IMAP and persist in DB.

- **Epics Covered:** Epic 1 fully.
- **Tasks:**
  - Implement IMAP connector (using `imapclient`).
  - Sync unread/flagged emails into `EmailMessage`.
  - Store metadata: sender, recipients, subject, body.
  - Log retrieval events in `SystemLog`.
  - Error handling & retry logic for failed syncs.
- **Deliverables:**
  - Dashboard endpoint listing pulled emails.
  - Verified storage of multiple accounts.

---

## Sprint 2: Draft Generation with ChatGPT (Week 5-6)
**Goal:** Generate AI-based draft replies.

- **Epics Covered:** Epic 2 fully.
- **Tasks:**
  - Integrate OpenAI GPT API.
  - Create prompt template system (tone, signature).
  - Generate drafts for each new `EmailMessage`.
  - Save drafts in `DraftResponse` with metadata.
  - Implement spam/notification filtering pre-processor.
- **Deliverables:**
  - Automatic draft generation visible in DB.
  - API endpoint to retrieve drafts per user.

---

## Sprint 3: Review & Approval Dashboard (Week 7-8)
**Goal:** Build user-facing review & approval workflow.

- **Epics Covered:** Epic 3 fully, Epic 8 (partial).
- **Tasks:**
  - Build web dashboard (React + Tailwind).
  - Display list of pending drafts (sender, subject, snippet).
  - Add approval/reject/edit controls.
  - Persist actions in `ApprovalLog`.
  - Support inline editing of drafts.
  - Batch approval/reject feature.
- **Deliverables:**
  - Fully functional dashboard for draft review.
  - All approvals logged.

---

## Sprint 4: Sending Replies via SMTP (Week 9-10)
**Goal:** Deliver approved drafts as actual email replies.

- **Epics Covered:** Epic 4 fully.
- **Tasks:**
  - Implement SMTP connector (`smtplib`).
  - Send approved draft as threaded reply.
  - Store record in `SentMessage`.
  - Retry logic for failed sends.
  - Confirmation notification in dashboard.
- **Deliverables:**
  - End-to-end workflow: IMAP → GPT draft → approval → SMTP send.
  - Logging of all sent messages.

---

## Sprint 5: Rules & Prioritization Engine (Week 11-12)
**Goal:** Implement user-defined filters and prioritization.

- **Epics Covered:** Epic 5 fully, Epic 8 (partial).
- **Tasks:**
  - Define rule schema in `Rule` table.
  - Support rules: keyword, sender, subject.
  - Apply rules in pre-processor.
  - Highlight or skip messages in dashboard based on rules.
- **Deliverables:**
  - Rules configuration panel in dashboard.
  - Prioritized email queues visible.

---

## Sprint 6: Security & Compliance Enhancements (Week 13-14)
**Goal:** Harden system for privacy and compliance.

- **Epics Covered:** Epic 6 fully.
- **Tasks:**
  - Encrypt DB fields (AES-256 at rest).
  - TLS for all in-transit communication.
  - Implement retention policies (configurable delete).
  - Immutable audit logs.
- **Deliverables:**
  - Security-compliant storage layer.
  - Documentation for compliance audits.

---

## Sprint 7: Logging & Analytics (Week 15-16)
**Goal:** Provide insights into AI performance and user actions.

- **Epics Covered:** Epic 7 fully.
- **Tasks:**
  - Metrics dashboard (approve/edit/reject rates).
  - System health dashboard (fetch/send success rates).
  - Export logs for admin use.
- **Deliverables:**
  - User-facing analytics panel.
  - Admin-level system monitoring.

---

## Sprint 8: Final Settings & Polishing (Week 17-18)
**Goal:** Complete admin/settings, polish UX, and prep release.

- **Epics Covered:** Epic 8 fully.
- **Tasks:**
  - Add UI for user preferences (tone, sync frequency, signature).
  - Enable/disable accounts from dashboard.
  - Polish UX with notifications and error messages.
  - Documentation and onboarding flow.
- **Deliverables:**
  - Production-ready system.
  - Release v1.0 of Project ReplyPilot.

---

## Release Plan
- **MVP (End of Sprint 4):** End-to-end draft → approval → send.  
- **Full v1.0 (End of Sprint 8):** Secure, rules-driven, analytics-backed platform.  


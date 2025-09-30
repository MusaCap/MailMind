# Project ReplyPilot — Backlog

## Epic 1: Email Retrieval & Connection
Enable the system to connect securely to email services, fetch emails, and prepare them for processing.

### User Stories
1. **As a user, I want to connect my email account (via IMAP/SMTP) so that the system can fetch and send emails on my behalf.**
   - **Acceptance Criteria:**
     - Support OAuth2 and password-based authentication.
     - Connection health check must be visible to the user.
     - Errors are logged in `SystemLog`.

2. **As a user, I want the system to fetch only unread or flagged emails so that my review queue remains focused.**
   - **Acceptance Criteria:**
     - IMAP filters: unread, flagged, priority folders.
     - Configurable sync interval (e.g., every 5 minutes).

3. **As a system admin, I want to log all fetch attempts so I can monitor reliability.**
   - **Acceptance Criteria:**
     - All fetch operations recorded in `SystemLog`.
     - Errors retried automatically with exponential backoff.

---

## Epic 2: AI Response Generation
Use ChatGPT to generate draft replies tailored to email context and user preferences.

### User Stories
1. **As a user, I want AI to generate a draft reply for every new email so I save time.**
   - **Acceptance Criteria:**
     - Draft stored in `DraftResponse`.
     - Includes subject line and formatted body.
     - Supports plain text and HTML.

2. **As a user, I want to configure tone (formal, casual, concise, detailed) so drafts match my style.**
   - **Acceptance Criteria:**
     - Preferences stored in `User.preferences`.
     - Tone used recorded in `DraftResponse.tone_used`.

3. **As a system, I want to handle spam or automated emails by skipping draft generation.**
   - **Acceptance Criteria:**
     - Pre-processor flags emails with spam characteristics.
     - No draft generated for filtered emails.

---

## Epic 3: Review & Approval Workflow
Ensure that no draft is sent without explicit user approval.

### User Stories
1. **As a user, I want to see a dashboard of AI-generated drafts so I can review them.**
   - **Acceptance Criteria:**
     - Draft queue visible by date, sender, subject.
     - Drafts marked as `pending_review` until acted upon.

2. **As a user, I want to edit drafts before sending so I can refine the AI’s response.**
   - **Acceptance Criteria:**
     - Inline editing supported in the dashboard.
     - Edited version replaces `DraftResponse.generated_body`.
     - Change logged in `ApprovalLog`.

3. **As a user, I want to approve or reject drafts so I have control.**
   - **Acceptance Criteria:**
     - Approval sets status = `approved`.
     - Rejection sets status = `rejected`.
     - Both actions logged in `ApprovalLog`.

4. **As a user, I want batch approval options so I can process multiple drafts quickly.**
   - **Acceptance Criteria:**
     - Multi-select drafts for approve/reject.
     - Log entries created for each draft.

---

## Epic 4: Sending Replies
Allow approved drafts to be sent using SMTP.

### User Stories
1. **As a user, I want approved drafts to be sent immediately after I confirm, so I don’t have to copy-paste.**
   - **Acceptance Criteria:**
     - Email threaded correctly (In-Reply-To, References).
     - Stored in `SentMessage`.

2. **As a user, I want confirmation that my reply was successfully sent so I trust the system.**
   - **Acceptance Criteria:**
     - Notification in UI.
     - Record of sent mail in `SentMessage`.

3. **As a system, I want to retry failed sends so that delivery reliability is high.**
   - **Acceptance Criteria:**
     - Automatic retries with backoff.
     - Failures logged.

---

## Epic 5: Rules & Prioritization
Enable customizable workflows for prioritizing messages.

### User Stories
1. **As a user, I want to create rules that flag emails containing “urgent” so I can review them first.**
   - **Acceptance Criteria:**
     - Rule stored in `Rule` table.
     - Emails flagged in dashboard.

2. **As a user, I want to ignore newsletters and promotional mail so I reduce clutter.**
   - **Acceptance Criteria:**
     - Rule-based filtering (subject, sender).
     - Ignored emails skipped in processing.

3. **As a user, I want to highlight emails from VIP senders so I don’t miss them.**
   - **Acceptance Criteria:**
     - Custom labels or visual highlights.
     - Configurable per user.

---

## Epic 6: Security & Compliance
Ensure data safety and regulatory compliance.

### User Stories
1. **As a user, I want my emails encrypted in storage so they remain private.**
   - **Acceptance Criteria:**
     - AES-256 encryption at rest.
     - TLS in transit.

2. **As a system, I want to avoid storing raw emails long-term so privacy risk is minimized.**
   - **Acceptance Criteria:**
     - Emails deleted after processing (configurable retention).
     - Drafts and logs anonymized where possible.

3. **As a compliance officer, I want audit logs so I can track actions for compliance.**
   - **Acceptance Criteria:**
     - `SystemLog` captures all major events.
     - Logs immutable.

---

## Epic 7: Logging & Analytics
Provide transparency into system operations.

### User Stories
1. **As a user, I want to see how many drafts I approved, edited, or rejected so I understand AI usefulness.**
   - **Acceptance Criteria:**
     - Dashboard shows metrics (% edited vs. approved directly).
     - Data sourced from `ApprovalLog`.

2. **As a system admin, I want to track fetch, draft, approval, and send rates so I monitor performance.**
   - **Acceptance Criteria:**
     - Metrics dashboard with success/error rates.
     - Export logs for analysis.

---

## Epic 8: Admin & Settings
Give users full control over preferences and system behavior.

### User Stories
1. **As a user, I want to configure my signature so it’s automatically included in drafts.**
   - **Acceptance Criteria:**
     - Signature stored in `User.preferences`.
     - Appended automatically.

2. **As a user, I want to set sync frequency (5 min, 15 min, hourly) so I control performance.**
   - **Acceptance Criteria:**
     - Configurable per account.
     - Stored in `EmailAccount`.

3. **As a user, I want to enable/disable accounts so I can pause processing temporarily.**
   - **Acceptance Criteria:**
     - `EmailAccount.status` reflects state.
     - Disabled accounts not polled.


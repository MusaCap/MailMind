# Project ReplyPilot — Data Model

## 1. Entities & Relationships  

### 1.1 User  
Represents the end-user of the system.  
- **Fields**:  
  - `user_id` (PK)  
  - `name`  
  - `email`  
  - `password_hash` / `oauth_token`  
  - `preferences` (JSON: tone, signature, etc.)  
  - `created_at`  
  - `updated_at`  

---

### 1.2 EmailAccount  
Stores connection details for IMAP/SMTP.  
- **Fields**:  
  - `account_id` (PK)  
  - `user_id` (FK → User.user_id)  
  - `imap_host`  
  - `imap_port`  
  - `smtp_host`  
  - `smtp_port`  
  - `auth_type` (OAuth2 / password)  
  - `status` (active, error, disabled)  
  - `created_at`  
  - `updated_at`  

---

### 1.3 EmailMessage  
Represents an email pulled from IMAP.  
- **Fields**:  
  - `message_id` (PK)  
  - `account_id` (FK → EmailAccount.account_id)  
  - `imap_uid` (unique identifier from IMAP server)  
  - `sender`  
  - `recipients` (array/string)  
  - `subject`  
  - `body_plain`  
  - `body_html`  
  - `status` (unread, processed, archived)  
  - `received_at`  
  - `created_at`  

---

### 1.4 DraftResponse  
Stores AI-generated draft replies for messages.  
- **Fields**:  
  - `draft_id` (PK)  
  - `message_id` (FK → EmailMessage.message_id)  
  - `generated_body` (text)  
  - `tone_used` (formal, casual, etc.)  
  - `confidence_score` (optional, from AI)  
  - `status` (pending_review, approved, rejected, edited)  
  - `created_at`  
  - `updated_at`  

---

### 1.5 ApprovalLog  
Tracks user actions on drafts.  
- **Fields**:  
  - `approval_id` (PK)  
  - `draft_id` (FK → DraftResponse.draft_id)  
  - `user_id` (FK → User.user_id)  
  - `action` (approved, edited, rejected)  
  - `comments` (optional)  
  - `timestamp`  

---

### 1.6 SentMessage  
Stores records of sent replies.  
- **Fields**:  
  - `sent_id` (PK)  
  - `draft_id` (FK → DraftResponse.draft_id)  
  - `smtp_message_id` (ID returned by SMTP server)  
  - `recipients`  
  - `subject`  
  - `body_sent`  
  - `sent_at`  

---

### 1.7 Rule  
Defines user-specific prioritization rules.  
- **Fields**:  
  - `rule_id` (PK)  
  - `user_id` (FK → User.user_id)  
  - `rule_type` (keyword, sender, subject, etc.)  
  - `condition` (JSON: e.g., `{ "keyword": "urgent" }`)  
  - `action` (flag, ignore, prioritize)  
  - `created_at`  
  - `updated_at`  

---

### 1.8 SystemLog  
Captures operational and error logs.  
- **Fields**:  
  - `log_id` (PK)  
  - `level` (info, warning, error)  
  - `component` (fetcher, generator, sender, ui)  
  - `message`  
  - `timestamp`  

---

## 2. Relationships Diagram (Textual)

- **User** ⟶ **EmailAccount** (1:N)  
- **EmailAccount** ⟶ **EmailMessage** (1:N)  
- **EmailMessage** ⟶ **DraftResponse** (1:N)  
- **DraftResponse** ⟶ **ApprovalLog** (1:N)  
- **DraftResponse** ⟶ **SentMessage** (1:1, if approved and sent)  
- **User** ⟶ **Rule** (1:N)  

---

## 3. Example Flow

1. `EmailMessage` fetched from IMAP.  
2. AI generates a `DraftResponse`.  
3. User reviews via dashboard: action logged in `ApprovalLog`.  
4. If approved, draft becomes a `SentMessage` via SMTP.  
5. All steps logged in `SystemLog`.  


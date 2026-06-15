# Database Schema: EduPlan AI (Django-Inspired)

This schema supports multi-tenant schools, curriculum standards, lesson plan lifecycle, AI generation logging, RAG with `pgvector`, subscription management, advertising, and audit trails.

---

## Table of Contents

1. [Naming Conventions](#naming-conventions)
2. [Extensions](#extensions)
3. [Tables](#tables)
4. [Relationships](#relationships)
5. [Implementation Notes](#implementation-notes)

---

## Naming Conventions

- **Tables**: `snake_case`, plural (e.g., `schools`, `lesson_plans`)
- **Primary keys**: `id` (auto-increment integer or UUID)
- **Foreign keys**: `{referenced_table}_id`
- **Timestamps**: `created_at`, `updated_at` (where applicable)

---

## Extensions (PostgreSQL)

```sql
CREATE EXTENSION IF NOT EXISTS "pgvector";
```

---

## Tables

### 1. schools

Multi-tenant school organization.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| name | VARCHAR(255) NOT NULL | School name |
| address | TEXT | Physical address |
| state | VARCHAR(100) | State/Province |
| country | VARCHAR(100) DEFAULT 'Nigeria' | Country |
| status | VARCHAR(20) DEFAULT 'pending' | Status: `trial`, `free`, `premium_active`, `premium_expired`, `suspended` |
| tier | VARCHAR(20) | Tier: `free`, `premium` (derived from status) |
| trial_start_date | DATE | NULL if not in trial |
| trial_end_date | DATE | NULL if no trial or trial ended |
| subscription_end_date | DATE | NULL for free/trial; set when premium paid |
| subscription_plan | VARCHAR(20) | Plan: `basic`, `standard`, `premium` |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_schools_status_tier ON schools(status, tier);
CREATE INDEX idx_schools_trial_end ON schools(trial_end_date) WHERE status = 'trial';
```

---

### 2. users

System users (teachers, admins, supervisors).

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| email | VARCHAR(255) UNIQUE | Email address |
| role | VARCHAR(50) | Role: `teacher`, `admin`, `supervisor` |
| full_name | VARCHAR(255) | Full name |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_users_school_id ON users(school_id);
CREATE INDEX idx_users_email ON users(email);
```

---

### 3. curriculum_standards

Curriculum standards per school.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| subject | VARCHAR(100) NOT NULL | Subject name |
| class_level | VARCHAR(50) NOT NULL | e.g., "JSS 2", "SS 3" |
| term | INTEGER | Term: 1, 2, or 3 |
| week | INTEGER | Week number within term |
| topic | VARCHAR(255) NOT NULL | Topic/Unit name |
| objectives | TEXT | Learning objectives |
| version | INTEGER DEFAULT 1 | Curriculum version |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_curriculum_standards_school ON curriculum_standards(school_id, subject, class_level, version);
```

---

### 4. schemes_of_work

Detailed scheme of work (one-to-one with curriculum_standards).

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| curriculum_id | INTEGER (FK) UNIQUE | Foreign key → `curriculum_standards.id` |
| sub_topics | JSONB | Array of sub-topics |
| activities | JSONB | Array of activities |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

---

### 5. lesson_plans

Main lesson plan entity.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| teacher_id | INTEGER (FK) | Foreign key → `users.id` |
| subject | VARCHAR(100) NOT NULL | Subject name |
| class_level | VARCHAR(50) NOT NULL | Class/Grade level |
| topic | VARCHAR(255) NOT NULL | Lesson topic |
| objectives | TEXT | Learning objectives |
| content | JSONB | Structured lesson content |
| ai_generated | BOOLEAN DEFAULT FALSE | AI generation flag |
| status | VARCHAR(20) DEFAULT 'draft' | Status: `draft`, `submitted`, `approved`, `rejected`, `archived` |
| version | INTEGER DEFAULT 1 | Plan version |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_lesson_plans_school_status ON lesson_plans(school_id, status);
CREATE INDEX idx_lesson_plans_teacher ON lesson_plans(teacher_id, subject, class_level);
```

---

### 6. lesson_plan_versions

Version history and snapshots of lesson plans.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| lesson_id | INTEGER (FK) | Foreign key → `lesson_plans.id` |
| version_number | INTEGER NOT NULL | Version number |
| snapshot | JSONB NOT NULL | Full plan data at that version |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Constraints**:
```sql
UNIQUE (lesson_id, version_number)
```

---

### 7. lesson_approvals

Approval workflow for supervisor review.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| lesson_id | INTEGER (FK) | Foreign key → `lesson_plans.id` |
| reviewer_id | INTEGER (FK) | Foreign key → `users.id` |
| status | VARCHAR(20) DEFAULT 'pending' | Status: `pending`, `approved`, `rejected` |
| comment | TEXT | Reviewer comments |
| reviewed_at | TIMESTAMP | Review timestamp |

**Indexes**:
```sql
CREATE INDEX idx_lesson_approvals_lesson ON lesson_approvals(lesson_id);
```

---

### 8. ai_generation_logs

Traceability for AI-generated content.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| user_id | INTEGER (FK) | Foreign key → `users.id` (SET NULL if deleted) |
| model_used | VARCHAR(100) NOT NULL | Model: `gemini-pro`, `groq-llama`, etc. |
| prompt | TEXT | User prompt |
| input_data | JSONB | Structured input context |
| output | TEXT | Generated output |
| tokens_used | INTEGER DEFAULT 0 | Tokens consumed |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_ai_logs_school_model ON ai_generation_logs(school_id, model_used);
```

---

### 9. document_embeddings

Vector embeddings for RAG and semantic search.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| content_type | VARCHAR(100) NOT NULL | Type: `lesson`, `curriculum`, `worksheet` |
| object_id | INTEGER NOT NULL | ID of referenced entity |
| embedding | VECTOR(1536) | pgvector field (1536-dim for OpenAI) |
| text_content | TEXT | Original text that was embedded |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_document_embeddings_school ON document_embeddings(school_id, content_type);
CREATE INDEX idx_embedding_ivfflat ON document_embeddings USING ivfflat (embedding vector_cosine_ops);
```

---

### 10. documents

File references (Supabase/S3 storage).

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| lesson_id | INTEGER (FK) | Foreign key → `lesson_plans.id` (nullable) |
| file_type | VARCHAR(10) NOT NULL | Type: `pdf`, `docx`, `pptx`, `xlsx` |
| file_url | TEXT NOT NULL | Storage URL |
| file_name | VARCHAR(255) NOT NULL | File name |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_documents_lesson ON documents(lesson_id);
```

---

### 11. notifications

In-app, email, or WhatsApp notifications.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| user_id | INTEGER (FK) | Foreign key → `users.id` |
| type | VARCHAR(20) NOT NULL | Type: `email`, `whatsapp`, `in_app` |
| title | VARCHAR(255) NOT NULL | Notification title |
| message | TEXT | Notification message |
| is_read | BOOLEAN DEFAULT FALSE | Read status (in-app only) |
| created_at | TIMESTAMP DEFAULT NOW() | Creation timestamp |

**Indexes**:
```sql
CREATE INDEX idx_notifications_user_read ON notifications(user_id, is_read);
```

---

### 12. subscriptions

Payment and subscription management (supports multiple subscriptions per school with `is_current` flag).

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| plan | VARCHAR(20) NOT NULL | Plan: `basic`, `standard`, `premium` |
| status | VARCHAR(20) DEFAULT 'pending' | Status: `pending`, `active`, `expired`, `suspended` |
| start_date | DATE NOT NULL | Subscription start date |
| end_date | DATE NOT NULL | Subscription end date |
| payment_reference | VARCHAR(255) | Payment gateway reference |
| amount_paid | DECIMAL(10,2) NOT NULL | Amount paid |
| is_current | BOOLEAN DEFAULT TRUE | Flag for current active subscription |

**Indexes**:
```sql
CREATE INDEX idx_subscriptions_status_date ON subscriptions(status, end_date);
CREATE INDEX idx_subscriptions_is_current ON subscriptions(school_id, is_current);
```

---

### 13. feature_usage

Track feature usage per school for subscription tiers.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| feature_name | VARCHAR(50) NOT NULL | Feature: `ai_generations`, `exports`, etc. |
| usage_count | INTEGER DEFAULT 0 | Current usage count |
| reset_date | DATE NOT NULL | Typically current date; resets monthly |

**Constraints**:
```sql
UNIQUE (school_id, feature_name, reset_date)
```

**Indexes**:
```sql
CREATE INDEX idx_feature_usage_reset ON feature_usage(reset_date);
```

---

### 14. ad_campaigns

Vendor advertising campaigns.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| vendor_name | VARCHAR(255) NOT NULL | Vendor name |
| title | VARCHAR(255) NOT NULL | Campaign title |
| description | TEXT | Campaign description |
| target_audience | JSONB | Targeting: `{"state": "Lagos", "school_type": "private"}` |
| start_date | DATE NOT NULL | Campaign start date |
| end_date | DATE NOT NULL | Campaign end date |
| status | VARCHAR(20) DEFAULT 'pending' | Status: `pending`, `approved`, `rejected`, `active` |

**Indexes**:
```sql
CREATE INDEX idx_ad_campaigns_status_date ON ad_campaigns(status, start_date);
```

---

### 15. ad_analytics

Daily ad performance metrics.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| ad_id | INTEGER (FK) | Foreign key → `ad_campaigns.id` |
| impressions | INTEGER DEFAULT 0 | Number of impressions |
| clicks | INTEGER DEFAULT 0 | Number of clicks |
| date | DATE NOT NULL | Analytics date |

**Constraints**:
```sql
UNIQUE (ad_id, date)
```

---

### 16. curriculum_coverage

Tracks progress per school against curriculum standards.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| curriculum_id | INTEGER (FK) | Foreign key → `curriculum_standards.id` |
| status | VARCHAR(20) DEFAULT 'not_started' | Status: `not_started`, `in_progress`, `completed` |
| evidence | JSONB | Links to lesson plans or documents |
| updated_at | TIMESTAMP DEFAULT NOW() | Last update timestamp |

**Indexes**:
```sql
CREATE INDEX idx_curriculum_coverage_school ON curriculum_coverage(school_id, curriculum_id);
```

---

### 17. audit_logs

Complete audit trail for compliance and debugging.

| Column | Type | Description |
|--------|------|-------------|
| id | SERIAL (PK) | Primary key |
| school_id | INTEGER (FK) | Foreign key → `schools.id` |
| user_id | INTEGER (FK) | Foreign key → `users.id` (SET NULL if deleted) |
| action | VARCHAR(255) NOT NULL | Action: `lesson.approve`, `ai.generate`, etc. |
| metadata | JSONB | Old/new values, IP address, etc. |
| created_at | TIMESTAMP DEFAULT NOW() | Action timestamp |

**Indexes**:
```sql
CREATE INDEX idx_audit_logs_school_date ON audit_logs(school_id, created_at);
```

---

## Relationships

```
schools ─────┬── users
             ├── curriculum_standards ──┬── schemes_of_work
             │                          └── curriculum_coverage
             ├── lesson_plans ──────┬── lesson_plan_versions
             │                      ├── lesson_approvals
             │                      └── documents
             ├── ai_generation_logs
             ├── document_embeddings
             ├── notifications
             ├── subscriptions (one-to-many)
             ├── feature_usage
             ├── ad_campaigns ────── ad_analytics
             └── audit_logs
```

---

## Implementation Notes

### 1. Multi-Tenancy
All tables containing `school_id` enforce row-level isolation. Use Django's `CurrentSchoolMiddleware` to automatically filter queries by `school_id`.

### 2. Vector Search
- The `document_embeddings` table stores 1536-dimension vectors compatible with OpenAI `text-embedding-3-small`.
- For local models (e.g., Ollama with all-MiniLM), reduce dimensions accordingly.
- Use `ivfflat` index for similarity search performance.

### 3. File Storage
- `documents.file_url` should point to Supabase Storage or AWS S3.
- Generate pre-signed URLs on access for security.

### 4. Approval Workflow
- Combine `lesson_plans.status` with `lesson_approvals` records.
- When a teacher submits a plan: set status to `submitted` and create a pending approval record.
- Update to `approved` or `rejected` upon reviewer action.

### 5. Subscription Management
- Keep `schools.status` and `subscriptions.status` in sync.
- Use `subscriptions.is_current = TRUE` to identify the active subscription for each school.
- Use a cron job or database trigger to expire schools when `subscriptions.end_date` passes.

### 6. Feature Usage Tracking
- Track usage in `feature_usage` table for subscription tier enforcement.
- Reset counts daily/monthly depending on business rules.
- Query this table before allowing feature access to enforce limits.

### 7. AI Logging
- Always log every AI generation request to `ai_generation_logs`.
- Critical for billing, debugging, and regulatory compliance.

### 8. Audit Logging
- Use Django signals or middleware to automatically write to `audit_logs` for all write operations on key models:
  - `lesson_plans` (create, update, delete)
  - `lesson_approvals` (status changes)
  - `subscriptions` (creation, renewal, cancellation)

### 9. Foreign Key Constraints
```sql
ALTER TABLE users ADD CONSTRAINT fk_users_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE curriculum_standards ADD CONSTRAINT fk_curriculum_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE lesson_plans ADD CONSTRAINT fk_lesson_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE lesson_plans ADD CONSTRAINT fk_lesson_teacher 
  FOREIGN KEY (teacher_id) REFERENCES users(id) ON DELETE SET NULL;

ALTER TABLE lesson_plan_versions ADD CONSTRAINT fk_lesson_versions_lesson 
  FOREIGN KEY (lesson_id) REFERENCES lesson_plans(id) ON DELETE CASCADE;

ALTER TABLE lesson_approvals ADD CONSTRAINT fk_approvals_lesson 
  FOREIGN KEY (lesson_id) REFERENCES lesson_plans(id) ON DELETE CASCADE;

ALTER TABLE lesson_approvals ADD CONSTRAINT fk_approvals_reviewer 
  FOREIGN KEY (reviewer_id) REFERENCES users(id) ON DELETE SET NULL;

ALTER TABLE ai_generation_logs ADD CONSTRAINT fk_ai_logs_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE ai_generation_logs ADD CONSTRAINT fk_ai_logs_user 
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL;

ALTER TABLE document_embeddings ADD CONSTRAINT fk_embeddings_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE documents ADD CONSTRAINT fk_documents_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE documents ADD CONSTRAINT fk_documents_lesson 
  FOREIGN KEY (lesson_id) REFERENCES lesson_plans(id) ON DELETE SET NULL;

ALTER TABLE notifications ADD CONSTRAINT fk_notifications_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE notifications ADD CONSTRAINT fk_notifications_user 
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;

ALTER TABLE subscriptions ADD CONSTRAINT fk_subscriptions_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE feature_usage ADD CONSTRAINT fk_feature_usage_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE ad_analytics ADD CONSTRAINT fk_analytics_campaign 
  FOREIGN KEY (ad_id) REFERENCES ad_campaigns(id) ON DELETE CASCADE;

ALTER TABLE curriculum_coverage ADD CONSTRAINT fk_coverage_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE curriculum_coverage ADD CONSTRAINT fk_coverage_curriculum 
  FOREIGN KEY (curriculum_id) REFERENCES curriculum_standards(id) ON DELETE CASCADE;

ALTER TABLE audit_logs ADD CONSTRAINT fk_audit_school 
  FOREIGN KEY (school_id) REFERENCES schools(id) ON DELETE CASCADE;

ALTER TABLE audit_logs ADD CONSTRAINT fk_audit_user 
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL;
```

---

## Migration Strategy

1. Create all tables with `CREATE TABLE` statements.
2. Add indexes and constraints via `ALTER TABLE`.
3. Enable `pgvector` extension.
4. Set up triggers for `audit_logs` and automatic status synchronization.
5. Test multi-tenant row-level security policies (if using PostgreSQL RLS).

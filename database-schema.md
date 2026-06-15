Here is a clean database-schema.md file based on the Django model definitions you provided. I’ve converted the models into standard SQL table definitions, added data types, primary/foreign keys, indexes, and explanatory notes for clarity.

```markdown
# Database Schema: EduPlan AI (Django‑Inspired)

This schema supports multi‑tenant schools, curriculum standards, lesson plan lifecycle, AI generation logging, RAG with `pgvector`, subscription management, advertising, and audit trails.

---

## Naming Conventions
- Tables: `snake_case`, plural (e.g., `schools`, `lesson_plans`)
- Primary keys: `id` (auto‑increment integer or UUID – simplified to `SERIAL` here)
- Foreign keys: `{referenced_table}_id`
- Timestamps: `created_at`, `updated_at` (where applicable)

---

## Extensions (PostgreSQL)
```sql
CREATE EXTENSION IF NOT EXISTS "pgvector";
```

---

Table Definitions

1. schools

Column Type Description
id SERIAL (PK) 
name VARCHAR(255) NOT NULL 
address TEXT 
state VARCHAR(100) 
country VARCHAR(100) DEFAULT 'Nigeria' 
status VARCHAR(20) DEFAULT 'pending' pending, active, expired, suspended
subscription_plan VARCHAR(20) basic, standard, premium
created_at TIMESTAMP DEFAULT NOW() 
Index (status, subscription_plan) 

2. users

(Assumed – not fully defined in the ERD, but referenced as foreign keys. Add as needed.)

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
email VARCHAR(255) UNIQUE 
role VARCHAR(50) teacher, admin, supervisor
full_name VARCHAR(255) 
created_at TIMESTAMP DEFAULT NOW() 

3. curriculum_standards

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
subject VARCHAR(100) NOT NULL 
class_level VARCHAR(50) NOT NULL e.g., "JSS 2"
term INTEGER 1, 2, or 3
week INTEGER Week number within term
topic VARCHAR(255) NOT NULL 
objectives TEXT Learning objectives
version INTEGER DEFAULT 1 For curriculum versioning
created_at TIMESTAMP DEFAULT NOW() 
Index (school_id, subject, class_level, version) 

4. schemes_of_work

One‑to‑one with a curriculum standard (optional extra detail).

Column Type Description
id SERIAL (PK) 
curriculum_id INTEGER (FK → curriculum_standards.id) UNIQUE
sub_topics JSONB Array of sub‑topics
activities JSONB Array of activities
created_at TIMESTAMP DEFAULT NOW() 

5. lesson_plans

Main lesson plan entity.

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
teacher_id INTEGER (FK → users.id) 
subject VARCHAR(100) NOT NULL 
class_level VARCHAR(50) NOT NULL 
topic VARCHAR(255) NOT NULL 
objectives TEXT 
content JSONB Structured lesson content
ai_generated BOOLEAN DEFAULT FALSE 
status VARCHAR(20) DEFAULT 'draft' draft, submitted, approved, rejected, archived
version INTEGER DEFAULT 1 
created_at TIMESTAMP DEFAULT NOW() 
Index (school_id, status) 
Index (teacher_id, subject, class_level) 

6. lesson_plan_versions

Version history (snapshots).

Column Type Description
id SERIAL (PK) 
lesson_id INTEGER (FK → lesson_plans.id) 
version_number INTEGER NOT NULL 
snapshot JSONB NOT NULL Full plan data at that version
created_at TIMESTAMP DEFAULT NOW() 
Unique constraint (lesson_id, version_number) 

7. lesson_approvals

Workflow for supervisor review.

Column Type Description
id SERIAL (PK) 
lesson_id INTEGER (FK → lesson_plans.id) 
reviewer_id INTEGER (FK → users.id) 
status VARCHAR(20) DEFAULT 'pending' pending, approved, rejected
comment TEXT 
reviewed_at TIMESTAMP 
Index (lesson_id) 

8. ai_generation_logs

Traceability for AI‑generated content.

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
user_id INTEGER (FK → users.id) NULL if user deleted (SET NULL)
model_used VARCHAR(100) NOT NULL e.g., gemini-pro, groq-llama
prompt TEXT 
input_data JSONB Structured input context
output TEXT Generated output
tokens_used INTEGER DEFAULT 0 
created_at TIMESTAMP DEFAULT NOW() 
Index (school_id, model_used) 

9. document_embeddings

Vector embeddings for RAG (semantic search).

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
content_type VARCHAR(100) NOT NULL lesson, curriculum, worksheet
object_id INTEGER NOT NULL ID of the referenced entity
embedding VECTOR(1536) pgvector field
text_content TEXT Original text that was embedded
created_at TIMESTAMP DEFAULT NOW() 
Index (school_id, content_type) 
Index embedding (ivfflat) For similarity search

10. documents

File references (Supabase/S3).

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
lesson_id INTEGER (FK → lesson_plans.id) NULL for standalone documents
file_type VARCHAR(10) NOT NULL pdf, docx, pptx, xlsx
file_url TEXT NOT NULL Storage URL
file_name VARCHAR(255) NOT NULL 
created_at TIMESTAMP DEFAULT NOW() 
Index (lesson_id) 

11. notifications

In‑app, email, or WhatsApp notifications.

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
user_id INTEGER (FK → users.id) 
type VARCHAR(20) NOT NULL email, whatsapp, in_app
title VARCHAR(255) NOT NULL 
message TEXT 
is_read BOOLEAN DEFAULT FALSE For in‑app only
created_at TIMESTAMP DEFAULT NOW() 
Index (user_id, is_read) 

12. subscriptions

Separate from schools.subscription_plan – holds payment details.

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) UNIQUE One‑to‑one
plan VARCHAR(20) NOT NULL basic, standard, premium
status VARCHAR(20) DEFAULT 'pending' pending, active, expired, suspended
start_date DATE NOT NULL 
end_date DATE NOT NULL 
payment_reference VARCHAR(255) 
amount_paid DECIMAL(10,2) NOT NULL 
Index (status, end_date) 

13. ad_campaigns

For secondary monetisation (ads from vendors).

Column Type Description
id SERIAL (PK) 
vendor_name VARCHAR(255) NOT NULL 
title VARCHAR(255) NOT NULL 
description TEXT 
target_audience JSONB e.g., {"state": "Lagos", "school_type": "private"}
start_date DATE NOT NULL 
end_date DATE NOT NULL 
status VARCHAR(20) DEFAULT 'pending' pending, approved, rejected, active
Index (status, start_date) 

14. ad_analytics

Daily ad performance.

Column Type Description
id SERIAL (PK) 
ad_id INTEGER (FK → ad_campaigns.id) 
impressions INTEGER DEFAULT 0 
clicks INTEGER DEFAULT 0 
date DATE NOT NULL 
Unique constraint (ad_id, date) 

15. curriculum_coverage

Tracks progress per school against a curriculum standard.

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
curriculum_id INTEGER (FK → curriculum_standards.id) 
status VARCHAR(20) DEFAULT 'not_started' not_started, in_progress, completed
evidence JSONB Links to lesson plans or documents
updated_at TIMESTAMP DEFAULT NOW() 
Index (school_id, curriculum_id) 

16. audit_logs

Complete audit trail for compliance.

Column Type Description
id SERIAL (PK) 
school_id INTEGER (FK → schools.id) 
user_id INTEGER (FK → users.id) NULL if user deleted
action VARCHAR(255) NOT NULL e.g., lesson.approve, ai.generate
metadata JSONB Old/new values, IP address, etc.
created_at TIMESTAMP DEFAULT NOW() 
Index (school_id, created_at) 

---

Relationships Diagram

```
schools ──┬── users
          ├── curriculum_standards ──┬── schemes_of_work
          │                           └── curriculum_coverage
          ├── lesson_plans ──┬── lesson_plan_versions
          │                  ├── lesson_approvals
          │                  └── documents
          ├── ai_generation_logs
          ├── document_embeddings
          ├── notifications
          ├── subscriptions (one‑to‑one)
          ├── ad_campaigns ── ad_analytics
          └── audit_logs
```

---

Notes for Implementation

1. Multi‑tenancy – All tables containing school_id enforce row‑level isolation. Use Django’s CurrentSchoolMiddleware to automatically filter queries.
2. Vector Search – The document_embeddings table stores 1536‑dimension vectors (compatible with OpenAI text-embedding-3-small). For local models (e.g., Ollama with all‑MiniLM), reduce dimensions to 768.
3. File Storage – documents.file_url should point to Supabase Storage or AWS S3. Generate pre‑signed URLs on access.
4. Approval Workflow – Combine lesson_plans.status with lesson_approvals. When a teacher submits a plan, set status to submitted and create a pending approval record.
5. Subscription Enforcement – The schools.status and subscriptions.status should be kept in sync. A cron job or trigger can expire schools when subscriptions.end_date passes.
6. AI Logs – Always log every AI generation. This is critical for billing, debugging, and regulatory compliance.
7. Audit Logs – Use a middleware or Django signal to automatically write to audit_logs for all write operations on key models (lesson plans, approvals, subscriptions).

```

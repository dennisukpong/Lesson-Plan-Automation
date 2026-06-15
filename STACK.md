# EduPlan AI - Technology Stack (MVP - Open Source & Free Tier)

## Overview

EduPlan AI is a comprehensive lesson plan automation platform designed for schools in Nigeria. The MVP focuses on open-source, free, and lightweight solutions for rapid development and deployment without vendor lock-in.

**Stack Choices:**
- **Frontend**: React + Axios
- **Backend**: Django + Django REST Framework
- **Database & Storage**: Supabase (PostgreSQL + Storage)
- **Automation**: n8n (workflow automation)
- **WhatsApp Notifications**: WAWP (WhatsApp Webhook Provider)
- **AI Inference**: Multiple free-tier platforms (Gemini, HuggingFace, Groq, Together.ai)
- **Deployment**: Free tier services

---

## Frontend Stack

### Core Framework
- **React 18+** - UI library for component-based architecture
- **JavaScript/TypeScript** - Development language
- **Vite** - Lightning-fast build tool (faster alternative to Create React App)
- **Tailwind CSS** - Utility-first CSS framework for responsive design
- **React Router v6** - Client-side routing and URL state management

### HTTP Client
- **Axios** - Promise-based HTTP client for API communication
  - Interceptors for auth token management
  - Request/response transformers
  - Error handling middleware
  - Retry logic with exponential backoff
- **axios-retry** - Automatic retry on network failures

### State Management & Data Fetching
- **Zustand** - Lightweight state management (open-source)
- **Fetch API** + Custom hooks - For caching layer
- **LocalStorage/SessionStorage** - Client-side data persistence
- **cross-tab-communication** via BroadcastChannel API - Sync state across tabs

### Form Management
- **React Hook Form** - Lightweight form state management
  - Multi-step form support
  - Auto-save with debounce
  - Dirty state tracking
  - File upload integration
- **Zod** - Schema validation library (open-source, TypeScript-first)

### UI Components & Styling
- **Shadcn/ui** - Headless React components (open-source)
  - Dialog/Modal with focus trap
  - Form components
  - Table components
- **Recharts** - Data visualization library (open-source)
- **Lucide React** - Icon library (open-source)
- **Sonner** - Toast notifications (open-source)

### Tables & Data Display
- **TanStack Table (React Table)** - Headless table library (open-source)
  - Server-side pagination
  - Sorting and filtering
  - Column visibility management
- **React Window** - Virtual scrolling for large datasets
- **Framer Motion** - Animation library (free tier available)

### File Handling
- **React Dropzone** - File upload component (open-source)
  - Drag-and-drop support
  - Paste from clipboard
  - File preview
- **xlsx** or **papaparse** - CSV/Excel parsing for bulk uploads (open-source)
- **JSZip** - ZIP file handling for bulk exports (open-source)
- **react-image-crop** - Image cropping before upload

### Performance Optimization
- **React.lazy()** & **Suspense** - Code splitting and lazy loading
- **React.memo** - Component memoization
- **useMemo, useCallback** - Expensive computation optimization
- **web-vitals** - Core Web Vitals monitoring
- **lighthouse** - Performance auditing

### Error Handling & Monitoring
- **react-error-boundary** - Error boundary component
  - Global error catching
  - Fallback UI
  - Error logging
- **@sentry/react** - Error and performance monitoring
- **Global error handler** middleware for API errors
- **Custom error page** for 404, 500 errors

### Accessibility (a11y)
- **Semantic HTML** - Proper heading hierarchy, landmarks
- **ARIA labels** - For screen readers
- **axe-core** or **jest-axe** - Automated a11y testing
- **Focus management** - Keyboard navigation, focus traps
- **Skip links** - Skip to main content
- **WCAG 2.1 AA** compliance target

### Real-time & Cross-Tab Communication
- **BroadcastChannel API** - Cross-tab state synchronization
  - Tab-to-tab communication
  - Synchronized logout
  - Live notifications across tabs
- **use-broadcast-channel** - Custom React hook

### Rich Text & Content Editing
- **@tiptap/react** - Modern rich text editor
  - Support for various content types
  - Collaborative editing ready (Yjs compatible)
  - Markdown support
- **markdown-it** - Markdown parsing
- **prism-react-renderer** - Syntax highlighting for code blocks

### Development Tools
- **ESLint** - Code linting (open-source)
- **Prettier** - Code formatting (open-source)
- **Vitest** - Unit testing (open-source, faster than Jest)
- **@testing-library/react** - React component testing
- **Playwright** - E2E testing (open-source)
- **Storybook** - Component development and documentation
- **Mock Service Worker (MSW)** - API mocking for development/testing
- **Husky** - Git hooks (open-source)

### Analytics & Telemetry
- **google-analytics/react** - Google Analytics integration
- **@sentry/react** - Error tracking (already listed, but also for analytics)
- **web-vitals** - Core Web Vitals monitoring
- **privacy-compliant tracking** - Opt-in analytics

### Development Browser Tools
- **@react-devtools/shell** - React DevTools
- **@storybook/react** - Storybook for component development
- **chromatic** - Visual regression testing

### Deployment
- **Vercel** - Free tier for frontend deployment
  - Automatic deployments on git push
  - Preview deployments
  - Analytics included
- **GitHub Pages** - Alternative free option
- **Netlify** - Free tier available

---

## Backend Stack

### Core Framework
- **Django 4.2+** - Python web framework (open-source)
- **Django REST Framework** - Building REST APIs (open-source)
- **Python 3.10+** - Runtime environment
- **asgiref** - ASGI utilities for async support
- **Daphne** - ASGI HTTP server for async views and WebSockets

### API Development
- **DRF (Django REST Framework)** - REST API toolkit
  - Serializers for data validation
  - ViewSets and Routers for auto-routing
  - Authentication (Token-based, JWT)
  - Permissions and throttling
  - Custom exception handlers
- **drf-spectacular** - OpenAPI 3.0 schema generation
  - Auto-generated Swagger UI
  - ReDoc documentation
  - Interactive API explorer
- **django-cors-headers** - CORS configuration (open-source)
- **djangorestframework-recursive** - Nested serializers

### Database & ORM
- **Supabase** (PostgreSQL 14+) - Managed database
  - Free tier: 500 MB storage
  - 2 million monthly egress
  - Realtime capabilities
- **PostgreSQL Adapter**: psycopg2-binary or psycopg
- **Django ORM** - Built-in object-relational mapping
- **PgBouncer** - Connection pooling for Supabase
- **Database transactions** - Atomic operations, post-commit hooks

### Query Optimization & Profiling
- **django-silk** - SQL query profiling and debugging
- **django-extensions** - Additional management commands
- **query_debugger** - Query analysis middleware
- **select_related()** / **prefetch_related()** - ORM optimization

### Search & Filtering
- **django-filter** - Advanced filtering on queryset
- **PostgreSQL full-text search** - Built-in FTS capabilities
- **pgvector** - Vector embeddings for semantic search (Phase 3)

### Authentication & Security
- **Django Allauth** - User authentication and social auth (open-source)
  - Email verification
  - Password reset
  - Multi-backend support
- **djangorestframework-simplejwt** - JWT authentication (open-source)
  - Access & refresh tokens
  - Token rotation
  - Custom claims
- **django-ratelimit** - Rate limiting (open-source)
  - Per-user rate limiting
  - Throttle classes
- **django-cors-headers** - CORS handling (open-source)
- **SecurityMiddleware** - Security headers (Django built-in)
  - HSTS (HTTP Strict Transport Security)
  - X-Frame-Options
  - X-Content-Type-Options
  - Content Security Policy (CSP)
- **python-magic** - MIME type validation for file uploads
- **django-defender** - Brute force attack protection

### File Storage & Management
- **Supabase Storage** - Cloud object storage (free tier included)
  - File upload/download
  - CDN delivery
  - Built-in security policies
  - RLS (Row-Level Security)
- **django-storages** - Storage backend abstraction (open-source)
- **Pillow** - Image processing and optimization (open-source)
- **Signed URLs** - Secure file access without exposing storage directly

### Async & Background Jobs
- **Celery** - Distributed task queue (open-source)
  - Async task execution
  - Task scheduling
  - Task prioritization
  - Dead letter queues
  - Retry policies with exponential backoff
- **Redis** - Message broker for Celery (free self-hosted or Upstash free tier)
  - In-memory caching
  - Session storage
  - Job queue
- **django-celery-beat** - Scheduled tasks (open-source)
  - Periodic tasks
  - Cron-like scheduling
- **Flower** - Celery monitoring and administration UI
  - Task monitoring
  - Worker monitoring
  - Real-time stats

### Caching Strategy
- **Django Caching Framework** - Built-in caching
  - Redis cache backend (for production)
  - Database caching (development)
  - HTTP cache headers (ETag, Last-Modified)
  - Browser caching strategies
- **Cache invalidation** patterns and decorators

### Automation & Workflows
- **n8n** - Open-source workflow automation platform
  - Trigger: Webhook from Django API
  - Process: AI generation, email notifications, WhatsApp messages
  - Nodes: Multiple AI inference platforms
  - Webhook: Send results back to Django
  - Self-hosted or cloud (n8n Cloud free tier)

### AI Inference Platforms (Free Tier)

#### **Primary: Google Generative AI (Gemini)**
- **Rate**: 60 API calls/minute
- **Cost**: Free tier (no paid required for MVP)
- **Use Cases**: Default lesson plan generation
- **Fallback**: Yes, can route to other models

#### **Secondary: HuggingFace Inference API**
- **Rate**: 30,000 requests/month free
- **Models**: Access to 150,000+ open models
- **Use Cases**: Fallback for text generation, embeddings
- **Documentation**: https://huggingface.co/inference-api
- **Advantages**: Open models, community-driven

#### **Tertiary: Groq Cloud**
- **Rate**: 1,440 requests/day free
- **Speed**: Ultra-fast inference (fastest available)
- **Models**: Mixtral 8x7b, Llama 2
- **Use Cases**: Fast lesson plan generation
- **URL**: https://groq.com

#### **Quaternary: Together.ai**
- **Rate**: Free tier available
- **Models**: Access to open models (Llama, Mistral, Falcon)
- **Speed**: Optimized for speed and cost
- **Use Cases**: Alternative fallback for generation

#### **Selection Strategy**
```python
# backends/ai_service.py
AI_PROVIDERS = {
    'primary': {
        'name': 'google_gemini',
        'rate_limit': 60,  # calls/minute
        'model': 'gemini-1.5-flash',
        'priority': 1,
        'timeout': 30  # seconds
    },
    'secondary': {
        'name': 'huggingface',
        'rate_limit': 30000,  # calls/month
        'model': 'mistralai/Mistral-7B-Instruct-v0.1',
        'priority': 2,
        'timeout': 60
    },
    'tertiary': {
        'name': 'groq',
        'rate_limit': 1440,  # calls/day
        'model': 'mixtral-8x7b-32768',
        'priority': 3,
        'timeout': 30
    },
    'quaternary': {
        'name': 'together',
        'rate_limit': 'free_tier',
        'model': 'togethercomputer/llama-2-70b-chat',
        'priority': 4,
        'timeout': 60
    }
}

async def generate_lesson_plan(prompt, fallback=True, timeout=30):
    """
    Try primary provider, fallback to others if needed
    """
    for provider in sorted(AI_PROVIDERS.values(), key=lambda x: x['priority']):
        try:
            result = await asyncio.wait_for(
                call_provider(provider, prompt),
                timeout=provider.get('timeout', timeout)
            )
            return result
        except asyncio.TimeoutError:
            logger.warning(f"{provider['name']} timed out")
            if not fallback:
                raise
            continue
        except RateLimitError:
            logger.warning(f"{provider['name']} rate limited, trying next")
            if not fallback:
                raise
            continue
        except Exception as e:
            logger.error(f"{provider['name']} failed: {e}")
            continue
    
    raise AIGenerationFailedError("All AI providers exhausted")
```

### Email & Notifications
- **Django's built-in email backend** - SMTP configuration
  - Gmail SMTP (free tier)
  - SendGrid (12,000 emails/month free)
  - Brevo (formerly Sendinblue) - 300 emails/day free
- **Email templates** - Django templating system
- **Async email sending** - Via Celery tasks
- **Email verification** - Confirmation links

### WhatsApp Notifications (WAWP)
- **WAWP (WhatsApp Webhook Provider)** - Open-source WhatsApp integration
  - Send messages via official WhatsApp Business API
  - Webhook support for incoming messages
  - Template messages
  - Document/image/media support
  - Message delivery status tracking

#### **WAWP Setup & Features**
```python
# settings.py
WAWP_CONFIG = {
    'phone_number': '234XXXXXXXXXX',  # Nigeria number
    'api_key': os.getenv('WAWP_API_KEY'),
    'webhook_url': os.getenv('WAWP_WEBHOOK_URL'),
    'max_retries': 3,
    'retry_delay': 5,  # seconds
}

# Use case: Notify teachers about approvals
from wawp import WhatsAppClient

client = WhatsAppClient(
    phone_number=settings.WAWP_CONFIG['phone_number'],
    api_key=settings.WAWP_CONFIG['api_key']
)

# Send lesson approval notification with retry logic
message = f"""
Hello {teacher_name},

Your lesson plan "{lesson_title}" has been approved!

View: {lesson_url}
"""

try:
    response = client.send_message(
        to=teacher_phone,
        message=message,
        template='lesson_approved'
    )
    # Log successful send
except WhatsAppError as e:
    # Retry logic
    logger.error(f"WhatsApp send failed: {e}")
```

#### **WAWP Notification Triggers**
```python
# Lesson Plan Approved
→ Send WhatsApp: "Your lesson has been approved"

# Curriculum Coverage Alert
→ Send WhatsApp: "Curriculum coverage below 50%"

# Subscription Expiring
→ Send WhatsApp: "Your subscription expires in 7 days"

# New Teacher Invited
→ Send WhatsApp: "You've been invited to EduPlan AI"

# AI Generation Complete
→ Send WhatsApp: "Your lesson plan is ready"

# Approval Needed
→ Send WhatsApp to Supervisor: "New lesson plan awaiting approval"
```

### Logging & Monitoring
- **Python logging** - Built-in logging module
  - Structured logging with dictConfig
  - File and console handlers
  - Different log levels (DEBUG, INFO, WARNING, ERROR)
- **structlog** - Structured JSON logging for better analysis
- **Sentry** (open-source) - Error tracking and monitoring
  - Self-hosted version available
  - Free tier on SaaS: 5,000 events/month
  - Release tracking
  - Performance monitoring
- **Django Debug Toolbar** - Development debugging (open-source)
  - SQL query analysis
  - Template rendering stats
  - HTTP request/response inspection
- **OpenTelemetry** - Distributed tracing (optional for Phase 2)
- **prometheus-client** - Metrics export
  - Django metrics via django-prometheus
  - Custom application metrics

### Testing
- **pytest** - Testing framework (open-source)
  - More flexible than unittest
  - Better fixtures and plugins
  - Parallel test execution
- **pytest-django** - Django integration (open-source)
- **pytest-cov** - Coverage reporting
- **factory-boy** - Test data factories (open-source)
  - Relationship handling
  - Sequences and post-generation
- **faker** - Fake data generation
- **responses** - Mock HTTP requests
- **freezegun** - Time freezing for testing time-dependent code
- **coverage** - Code coverage measurement (target: 80%+)

### Development & DevOps
- **Django-environ** - Environment configuration (open-source)
  - .env file parsing
  - Type casting
  - Environment validation at startup
- **python-decouple** - Alternative environment management
- **Gunicorn** - WSGI application server (open-source)
  - Worker configuration
  - Request timeout settings
- **Whitenoise** - Static file serving (open-source)
- **Black** - Code formatter (open-source)
- **Flake8** - Linting (open-source)
  - Style compliance checking
  - Error detection
- **isort** - Import sorting (open-source)
- **mypy** - Static type checking (optional but recommended)

### Performance Optimization
- **Django Caching Framework** - Built-in caching
  - Redis cache backend (free self-hosted)
  - Database caching
  - Cache invalidation patterns
  - HTTP cache headers (ETag, Last-Modified, Cache-Control)
- **django-ratelimit** - API rate limiting (open-source)
  - Per-user limits
  - Per-IP limits
  - Custom throttle classes
- **drf-spectacular** - API schema caching (open-source)
- **select_related()** / **prefetch_related()** - Query optimization
- **Database indexes** - Strategic indexing on frequently queried columns
- **Pagination** - Offset and cursor-based pagination

---

## Database Architecture

### Supabase PostgreSQL Setup

**Connection String:**
```
postgresql://username:password@db.xxxxx.supabase.co:5432/postgres
```

**Core Tables:**

```sql
-- Schools
CREATE TABLE schools (
  school_id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  address TEXT,
  state VARCHAR(100),
  country VARCHAR(100),
  contact_email VARCHAR(255),
  phone VARCHAR(20),
  whatsapp_number VARCHAR(20),
  logo_url TEXT,
  status VARCHAR(20) DEFAULT 'active',
  subscription_plan VARCHAR(50),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Users
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id) ON DELETE CASCADE,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  phone_number VARCHAR(20),
  whatsapp_number VARCHAR(20),
  role VARCHAR(50),
  status VARCHAR(20) DEFAULT 'active',
  is_email_verified BOOLEAN DEFAULT FALSE,
  is_whatsapp_verified BOOLEAN DEFAULT FALSE,
  notifications_enabled BOOLEAN DEFAULT TRUE,
  last_login TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Curriculum
CREATE TABLE curriculum (
  curriculum_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id) ON DELETE CASCADE,
  subject VARCHAR(100) NOT NULL,
  class_level VARCHAR(50),
  topic VARCHAR(255) NOT NULL,
  standards TEXT,
  description TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Lesson Plans
CREATE TABLE lesson_plans (
  lesson_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id) ON DELETE CASCADE,
  teacher_id INTEGER REFERENCES users(user_id),
  curriculum_id INTEGER REFERENCES curriculum(curriculum_id),
  title VARCHAR(255) NOT NULL,
  subject VARCHAR(100),
  class_level VARCHAR(50),
  topic VARCHAR(255),
  term VARCHAR(20),
  week INTEGER,
  objectives TEXT,
  introduction TEXT,
  main_content JSONB,
  activities JSONB,
  assessment TEXT,
  resources TEXT,
  duration_minutes INTEGER,
  status VARCHAR(50) DEFAULT 'draft',
  ai_generated BOOLEAN DEFAULT FALSE,
  ai_provider_used VARCHAR(100),
  ai_model_used VARCHAR(100),
  created_by INTEGER REFERENCES users(user_id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Lesson Plan Versions
CREATE TABLE lesson_plan_versions (
  version_id SERIAL PRIMARY KEY,
  lesson_id INTEGER REFERENCES lesson_plans(lesson_id) ON DELETE CASCADE,
  content JSONB,
  version_number INTEGER,
  change_summary TEXT,
  created_by INTEGER REFERENCES users(user_id),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Lesson Plan Approvals
CREATE TABLE lesson_plan_approvals (
  approval_id SERIAL PRIMARY KEY,
  lesson_id INTEGER REFERENCES lesson_plans(lesson_id) ON DELETE CASCADE,
  submitted_by INTEGER REFERENCES users(user_id),
  approved_by INTEGER REFERENCES users(user_id),
  approval_status VARCHAR(50),
  feedback TEXT,
  submitted_at TIMESTAMP DEFAULT NOW(),
  reviewed_at TIMESTAMP
);

-- Subscriptions
CREATE TABLE subscriptions (
  subscription_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id) ON DELETE CASCADE,
  plan VARCHAR(50),
  status VARCHAR(50) DEFAULT 'active',
  amount_paid DECIMAL(10, 2),
  payment_reference VARCHAR(255),
  billing_cycle VARCHAR(20),
  start_date DATE,
  end_date DATE,
  auto_renewal BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- AI Generation Logs
CREATE TABLE ai_generation_logs (
  log_id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(user_id),
  school_id INTEGER REFERENCES schools(school_id),
  prompt TEXT,
  ai_provider VARCHAR(100),
  ai_model VARCHAR(100),
  tokens_used INTEGER,
  cost_estimated DECIMAL(8, 4),
  response_time_ms INTEGER,
  status VARCHAR(50),
  error_message TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Audit Logs
CREATE TABLE audit_logs (
  log_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id),
  user_id INTEGER REFERENCES users(user_id),
  action VARCHAR(255),
  resource_type VARCHAR(100),
  resource_id INTEGER,
  old_values JSONB,
  new_values JSONB,
  ip_address VARCHAR(45),
  user_agent TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Notification Logs
CREATE TABLE notification_logs (
  log_id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(user_id),
  school_id INTEGER REFERENCES schools(school_id),
  notification_type VARCHAR(100),
  channel VARCHAR(50),
  recipient VARCHAR(255),
  message TEXT,
  status VARCHAR(50),
  delivery_timestamp TIMESTAMP,
  error_message TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Support Tickets
CREATE TABLE support_tickets (
  ticket_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id),
  user_id INTEGER REFERENCES users(user_id),
  category VARCHAR(100),
  priority VARCHAR(20),
  status VARCHAR(50) DEFAULT 'open',
  title VARCHAR(255),
  description TEXT,
  assigned_to INTEGER REFERENCES users(user_id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Flagged Content
CREATE TABLE flagged_content (
  flag_id SERIAL PRIMARY KEY,
  content_type VARCHAR(100),
  content_id INTEGER,
  reason VARCHAR(255),
  flagged_by INTEGER REFERENCES users(user_id),
  status VARCHAR(50) DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT NOW()
);

-- Celery Tasks (for monitoring)
CREATE TABLE celery_tasks (
  task_id UUID PRIMARY KEY,
  task_name VARCHAR(255),
  status VARCHAR(50),
  result JSONB,
  error_message TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  completed_at TIMESTAMP
);
```

### Database Indexes for Performance
```sql
CREATE INDEX idx_users_school ON users(school_id);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_whatsapp ON users(whatsapp_number);
CREATE INDEX idx_lesson_plans_school ON lesson_plans(school_id);
CREATE INDEX idx_lesson_plans_teacher ON lesson_plans(teacher_id);
CREATE INDEX idx_lesson_plans_status ON lesson_plans(status);
CREATE INDEX idx_lesson_plans_created ON lesson_plans(created_at DESC);
CREATE INDEX idx_curriculum_school ON curriculum(school_id);
CREATE INDEX idx_subscriptions_school ON subscriptions(school_id);
CREATE INDEX idx_audit_logs_school ON audit_logs(school_id);
CREATE INDEX idx_ai_gen_logs_user ON ai_generation_logs(user_id);
CREATE INDEX idx_ai_gen_logs_created ON ai_generation_logs(created_at DESC);
CREATE INDEX idx_notification_logs_user ON notification_logs(user_id);
CREATE INDEX idx_notification_logs_status ON notification_logs(status);

-- Full-text search indexes
CREATE INDEX idx_lesson_plans_content_fts ON lesson_plans USING gin(to_tsvector('english', title || ' ' || COALESCE(topic, '')));
CREATE INDEX idx_curriculum_topic_fts ON curriculum USING gin(to_tsvector('english', topic || ' ' || COALESCE(standards, '')));
```

---

## Supabase Configuration

### Storage Buckets
```
/lesson-plans
  ├── pdfs/
  ├── images/
  └── attachments/

/user-uploads
  ├── bulk-imports/
  └── exports/

/curriculum-files
  ├── documents/
  └── resources/
```

### Storage Policies (RLS)
```sql
-- Allow users to upload to their school's folder
CREATE POLICY "Users can upload to school folder"
ON storage.objects FOR INSERT
WITH CHECK (bucket_id = 'lesson-plans' AND auth.uid()::text = (storage.foldername(name))[1]);

-- Allow users to read public files
CREATE POLICY "Anyone can read public files"
ON storage.objects FOR SELECT
USING (bucket_id = 'lesson-plans' AND (storage.foldername(name))[2] = 'public');

-- CDN caching headers
-- Files are cached for 1 year if immutable, 1 hour if not
```

---

## n8n Workflow Automation

### n8n Setup Options
1. **n8n Cloud** (Free tier): 
   - 600 executions/month
   - Basic automation workflows
2. **Self-hosted n8n**:
   - Docker container
   - Unlimited executions
   - Full control

### Key Workflows

#### Workflow 1: AI Lesson Plan Generation (Multi-Provider Fallback)
```
Trigger: Webhook (from Django API)
  ↓
Input: Lesson details (subject, class, topic)
  ↓
Try Primary (Google Gemini API): Generate content
  ↓
If Rate Limited → Try Secondary (HuggingFace)
  ↓
If Failed → Try Tertiary (Groq)
  ↓
If Failed → Try Quaternary (Together.ai)
  ↓
Process: Format response, log provider used, measure response time
  ↓
Update Database: POST to Django API (with provider info)
  ↓
Send Notification: Email + WhatsApp
  ↓
Response: Webhook back to Django
```

#### Workflow 2: WhatsApp Notifications
```
Trigger: Webhook (from Django - approval, expiry alerts, etc)
  ↓
Extract: User WhatsApp number, notification type
  ↓
If Type = "approval":
  Format: "Your lesson [title] has been approved ✅"
  ↓
If Type = "expiry":
  Format: "Your subscription expires in 7 days ⏰"
  ↓
If Type = "coverage":
  Format: "Curriculum coverage below 50% ⚠️"
  ↓
Send via WAWP: WhatsAppClient.send_message() with retry
  ↓
Log: Save to notification_logs table with delivery status
  ↓
Response: Webhook back to Django with delivery status
```

#### Workflow 3: Scheduled Email Notifications
```
Trigger: Cron (Daily at 8 AM)
  ↓
Query: Get expiring subscriptions (7 days)
  ↓
For Each: Build email + WhatsApp message
  ↓
Send Email: Via SendGrid
  ↓
Send WhatsApp: Via WAWP
  ↓
Log: Update notification_logs via API
```

#### Workflow 4: Bulk User Import Processing
```
Trigger: Webhook (File uploaded)
  ↓
Read File: Parse CSV
  ↓
Validate: Check data format
  ↓
For Each Row: Create user via Django API
  ↓
Send Email: Invite users
  ↓
Send WhatsApp: Invite link (optional)
  ↓
Log: Record import summary
```

### n8n Nodes Used
- **HTTP Request** - Call Django API endpoints
- **Google Generative AI** - Gemini integration
- **HuggingFace Inference** - Alternative AI model
- **Groq** - Fast inference
- **Together.ai** - Alternative AI provider
- **SendGrid / Email** - Send notifications
- **WAWP / WhatsApp** - Send WhatsApp messages
- **If/Else** - Conditional logic
- **Loop** - Process collections
- **Set** - Data transformation
- **Webhook** - Receive triggers from Django
- **Merge** - Combine data from multiple sources
- **Code** - Custom JavaScript execution

---

## API Architecture (Django REST Framework)

### Authentication
```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle'
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/hour',
        'user': '1000/hour',
    },
}

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=5),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=1),
    'ROTATE_REFRESH_TOKENS': True,
    'ALGORITHM': 'HS256',
    'SIGNING_KEY': settings.SECRET_KEY,
}
```

### Token Security Best Practices
```python
# Use httpOnly cookies for JWT tokens (most secure)
SIMPLE_JWT['AUTH_COOKIE'] = 'access_token'
SIMPLE_JWT['AUTH_COOKIE_SECURE'] = True  # HTTPS only
SIMPLE_JWT['AUTH_COOKIE_HTTP_ONLY'] = True
SIMPLE_JWT['AUTH_COOKIE_SAMESITE'] = 'Strict'
```

### API Endpoints

**Authentication:**
```
POST   /api/auth/register/              # School registration
POST   /api/auth/login/                 # Login with email/password
POST   /api/auth/refresh/               # Refresh JWT token
POST   /api/auth/logout/                # Logout
POST   /api/auth/password-reset/        # Request password reset
POST   /api/auth/password-reset-confirm/ # Confirm reset with token
POST   /api/auth/verify-whatsapp/       # Verify WhatsApp number
GET    /api/auth/me/                    # Get current user
PATCH  /api/auth/profile/               # Update profile
```

**Lesson Plans (Teachers):**
```
GET    /api/lesson-plans/               # List user's lesson plans (paginated)
POST   /api/lesson-plans/               # Create new lesson plan
GET    /api/lesson-plans/{id}/          # Get lesson plan detail
PATCH  /api/lesson-plans/{id}/          # Update lesson plan
DELETE /api/lesson-plans/{id}/          # Delete lesson plan
POST   /api/lesson-plans/{id}/submit/   # Submit for approval
POST   /api/lesson-plans/{id}/ai-generate/  # Generate with AI (calls n8n)
GET    /api/lesson-plans/{id}/versions/ # Get version history
POST   /api/lesson-plans/{id}/duplicate/ # Duplicate a lesson plan
GET    /api/lesson-plans/search/?q=     # Full-text search
```

**Approvals (Supervisors):**
```
GET    /api/approvals/                  # Pending approvals queue
GET    /api/approvals/{id}/             # Approval detail
POST   /api/approvals/{id}/approve/     # Approve lesson plan
POST   /api/approvals/{id}/reject/      # Reject with feedback
```

**Curriculum Management (Admin):**
```
GET    /api/curriculum/                 # List curriculum
POST   /api/curriculum/                 # Create curriculum topic
GET    /api/curriculum/{id}/            # Get curriculum detail
PATCH  /api/curriculum/{id}/            # Update curriculum
DELETE /api/curriculum/{id}/            # Delete curriculum
POST   /api/curriculum/bulk-upload/     # Bulk upload via CSV
GET    /api/curriculum/search/?q=       # Search curriculum
```

**Users Management (School Admin):**
```
GET    /api/users/                      # List school users (paginated)
POST   /api/users/                      # Create new user
GET    /api/users/{id}/                 # Get user detail
PATCH  /api/users/{id}/                 # Update user
DELETE /api/users/{id}/                 # Delete user
POST   /api/users/bulk-upload/          # Bulk import CSV
POST   /api/users/{id}/reset-password/  # Reset password
PATCH  /api/users/{id}/whatsapp/        # Update WhatsApp settings
POST   /api/users/{id}/deactivate/      # Deactivate user
POST   /api/users/{id}/reactivate/      # Reactivate user
```

**Subscriptions (Admin):**
```
GET    /api/subscriptions/              # View subscription
POST   /api/subscriptions/upgrade/      # Upgrade plan
GET    /api/billing/invoices/           # List invoices
POST   /api/billing/invoices/download/  # Download invoice PDF
```

**Analytics (Admin):**
```
GET    /api/analytics/dashboard/        # Dashboard stats
GET    /api/analytics/lesson-trends/    # Lesson creation trends
GET    /api/analytics/curriculum-coverage/  # Coverage metrics
GET    /api/analytics/export/           # Export analytics report
```

**File Upload:**
```
POST   /api/upload/                     # Upload file
GET    /api/files/{id}/                 # Download file (signed URL)
DELETE /api/files/{id}/                 # Delete file
GET    /api/files/{id}/preview/         # Get file preview
```

**Notifications:**
```
GET    /api/notifications/              # Get notification history
POST   /api/notifications/send-whatsapp/ # Manual WhatsApp send (admin)
PATCH  /api/notifications/preferences/  # Update notification settings
GET    /api/notifications/unread/       # Get unread count
```

**Health & Status:**
```
GET    /api/health/                     # API health check
GET    /api/status/                     # System status
GET    /api/version/                    # API version
```

---

## Frontend - Axios Configuration

### HTTP Client Setup
```javascript
// api/axiosClient.js
import axios from 'axios';
import { useAuthStore } from '../store/authStore';

const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:8000/api';

const axiosClient = axios.create({
  baseURL: API_BASE_URL,
  timeout: 30000,  // 30 second timeout
  headers: {
    'Content-Type': 'application/json',
  },
  withCredentials: true,  // Send cookies with requests
});

// Request interceptor - Add auth token
axiosClient.interceptors.request.use(
  (config) => {
    const { accessToken } = useAuthStore.getState();
    if (accessToken) {
      config.headers.Authorization = `Bearer ${accessToken}`;
    }
    // Add CSRF token if available
    const csrfToken = document.querySelector('meta[name="csrf-token"]')?.content;
    if (csrfToken) {
      config.headers['X-CSRFToken'] = csrfToken;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor - Handle token refresh and errors
axiosClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;

    // Handle 401 with token refresh
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      try {
        const { refreshToken, setAuth } = useAuthStore.getState();
        const response = await axios.post(`${API_BASE_URL}/auth/refresh/`, {
          refresh: refreshToken,
        }, {
          withCredentials: true,
        });
        
        setAuth(response.data);
        return axiosClient(originalRequest);
      } catch (refreshError) {
        useAuthStore.getState().logout();
        window.location.href = '/login';
      }
    }

    // Handle rate limiting
    if (error.response?.status === 429) {
      console.warn('Rate limited, please try again later');
    }

    return Promise.reject(error);
  }
);

export default axiosClient;
```

### Axios Retry Configuration
```javascript
// api/axiosClient.js (continued)
import axiosRetry from 'axios-retry';

// Configure retry for network failures (not 4xx/5xx)
axiosRetry(axiosClient, {
  retries: 3,
  retryDelay: (retryCount) => {
    // Exponential backoff: 1s, 2s, 4s
    return retryCount * 1000 * Math.pow(2, retryCount);
  },
  retryCondition: (error) => {
    // Retry on network errors, 5xx, and specific 4xx codes
    return (
      !error.response || // Network error
      (error.response.status >= 500) || // 5xx
      error.response.status === 408 || // Request timeout
      error.response.status === 429    // Too many requests
    );
  },
});
```

### API Service Layer
```javascript
// services/lessonPlanService.js
import axiosClient from './axiosClient';

export const lessonPlanService = {
  // Get all lesson plans with pagination
  async getLessonPlans(params = {}) {
    const { data } = await axiosClient.get('/lesson-plans/', { params });
    return data;
  },

  // Search lesson plans
  async searchLessonPlans(query) {
    const { data } = await axiosClient.get('/lesson-plans/search/', {
      params: { q: query },
    });
    return data;
  },

  // Get single lesson plan with error handling
  async getLessonPlan(id) {
    try {
      const { data } = await axiosClient.get(`/lesson-plans/${id}/`);
      return data;
    } catch (error) {
      if (error.response?.status === 404) {
        throw new Error('Lesson plan not found');
      }
      throw error;
    }
  },

  // Create lesson plan with auto-save
  async createLessonPlan(payload) {
    const { data } = await axiosClient.post('/lesson-plans/', payload);
    return data;
  },

  // Update with conflict detection
  async updateLessonPlan(id, payload) {
    try {
      const { data } = await axiosClient.patch(`/lesson-plans/${id}/`, payload);
      return data;
    } catch (error) {
      if (error.response?.status === 409) {
        throw new Error('This lesson plan has been modified by someone else');
      }
      throw error;
    }
  },

  // Generate with AI (long-running, with polling)
  async generateWithAI(id, options = {}) {
    const { data } = await axiosClient.post(
      `/lesson-plans/${id}/ai-generate/`,
      options,
      {
        timeout: 60000, // 60 second timeout for generation
      }
    );
    return data; // Returns { task_id, status }
  },

  // Poll for AI generation status
  async pollAIGeneration(taskId) {
    const { data } = await axiosClient.get(
      `/lesson-plans/ai-generate/${taskId}/`
    );
    return data;
  },

  // Submit for approval
  async submitForApproval(id) {
    const { data } = await axiosClient.post(`/lesson-plans/${id}/submit/`);
    return data;
  },

  // Other methods...
};
```

### File Upload with Progress
```javascript
// services/uploadService.js
import axiosClient from './axiosClient';

export const uploadService = {
  async uploadFile(file, onProgress) {
    const formData = new FormData();
    formData.append('file', file);

    return axiosClient.post('/upload/', formData, {
      headers: {
        'Content-Type': 'multipart/form-data',
      },
      onUploadProgress: (progressEvent) => {
        const percentCompleted = Math.round(
          (progressEvent.loaded * 100) / progressEvent.total
        );
        onProgress?.(percentCompleted);
      },
      timeout: 120000, // 2 minutes for large files
    });
  },

  // Cancel upload
  cancelUpload(cancelToken) {
    cancelToken.cancel('Upload cancelled');
  },
};
```

---

## Error Handling & Resilience

### Backend Error Handling
```python
# api/exceptions.py
from rest_framework.exceptions import APIException

class AIGenerationFailedError(APIException):
    status_code = 503
    default_detail = 'AI generation service temporarily unavailable'

class RateLimitError(APIException):
    status_code = 429
    default_detail = 'Rate limit exceeded'

# Custom exception handler
def custom_exception_handler(exc, context):
    response = exception_handler(exc, context)
    
    if response is not None:
        response.data['timestamp'] = timezone.now()
        response.data['path'] = context['request'].path
    
    # Log to Sentry
    if status.is_server_error(response.status_code):
        sentry_sdk.capture_exception(exc)
    
    return response
```

### Frontend Error Boundaries
```javascript
// components/ErrorBoundary.js
import React from 'react';
import * as Sentry from '@sentry/react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    // Log to Sentry
    Sentry.captureException(error, { contexts: { react: errorInfo } });
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="p-4 bg-red-100 border border-red-400 rounded">
          <p>Something went wrong. Please try refreshing the page.</p>
          <button onClick={() => window.location.reload()}>
            Refresh
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

export default Sentry.withErrorBoundary(ErrorBoundary, {
  fallback: <p>An error occurred</p>,
});
```

### Offline Detection & Retry
```javascript
// hooks/useNetworkStatus.js
import { useState, useEffect } from 'react';

export const useNetworkStatus = () => {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  return isOnline;
};

// Usage
function MyComponent() {
  const isOnline = useNetworkStatus();

  if (!isOnline) {
    return <div>You are offline. Some features are unavailable.</div>;
  }

  return <div>Online</div>;
}
```

---

## Performance Optimization

### Frontend Code Splitting
```javascript
// routes/index.js
import { lazy, Suspense } from 'react';
import LoadingSpinner from '@/components/LoadingSpinner';

const LessonPlans = lazy(() => import('./pages/LessonPlans'));
const LessonDetail = lazy(() => import('./pages/LessonDetail'));

export const routes = [
  {
    path: '/lesson-plans',
    element: (
      <Suspense fallback={<LoadingSpinner />}>
        <LessonPlans />
      </Suspense>
    ),
  },
];
```

### React Optimization Patterns
```javascript
// Use memoization for expensive computations
import { useMemo, useCallback, memo } from 'react';

const LessonList = memo(({ lessons, onSelect }) => {
  const sortedLessons = useMemo(
    () => lessons.sort((a, b) => b.created_at - a.created_at),
    [lessons]
  );

  const handleSelect = useCallback((lesson) => {
    onSelect(lesson);
  }, [onSelect]);

  return (
    <div>
      {sortedLessons.map((lesson) => (
        <div key={lesson.id} onClick={() => handleSelect(lesson)}>
          {lesson.title}
        </div>
      ))}
    </div>
  );
});
```

### HTTP Caching Headers
```python
# views.py
from django.views.decorators.http import cache_page, vary_on_headers
from rest_framework.decorators import api_view, cache_control

@api_view(['GET'])
@cache_control(max_age=3600)  # Cache for 1 hour
def get_curriculum(request):
    # ...
    pass

@api_view(['GET'])
@vary_on_headers('Authorization')  # Cache per user
@cache_page(3600)
def get_lesson_plans(request):
    # ...
    pass
```

---

## Security Best Practices

### Frontend Security
```javascript
// Token Management
const storeTokens = (accessToken, refreshToken) => {
  // Store in httpOnly cookie (not localStorage)
  // This requires backend to set cookie
  document.cookie = `accessToken=${accessToken};HttpOnly;Secure;SameSite=Strict;Path=/`;
  document.cookie = `refreshToken=${refreshToken};HttpOnly;Secure;SameSite=Strict;Path=/`;
};

// CSRF Protection
const getCsrfToken = () => {
  return document.querySelector('meta[name="csrf-token"]')?.content;
};

// Headers
const headers = {
  'X-CSRFToken': getCsrfToken(),
  'X-Requested-With': 'XMLHttpRequest',
};

// Content Security Policy
const cspMeta = document.createElement('meta');
cspMeta.httpEquiv = 'Content-Security-Policy';
cspMeta.content = "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'";
document.head.appendChild(cspMeta);
```

### Backend Security
```python
# settings.py
CSRF_COOKIE_SECURE = True
CSRF_COOKIE_HTTPONLY = True
CSRF_COOKIE_SAMESITE = 'Strict'

SESSION_COOKIE_SECURE = True
SESSION_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Strict'

SECURE_HSTS_SECONDS = 31536000  # 1 year
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True

X_FRAME_OPTIONS = 'DENY'
SECURE_CONTENT_SECURITY_POLICY = {
    'default-src': "'self'",
    'script-src': ["'self'"],
    'style-src': ["'self'", "'unsafe-inline'"],
}
```

---

## Monitoring & Telemetry

### Frontend Monitoring
```javascript
// Sentry Configuration
import * as Sentry from '@sentry/react';

Sentry.init({
  dsn: process.env.REACT_APP_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1,
  integrations: [
    new Sentry.Replay({
      maskAllText: true,
      blockAllMedia: true,
    }),
  ],
  replaysSessionSampleRate: 0.1,
});

// Core Web Vitals
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getFCP(console.log);
getLCP(console.log);
getTTFB(console.log);
```

### Backend Monitoring
```python
# settings.py
import sentry_sdk
from sentry_sdk.integrations.django import DjangoIntegration

sentry_sdk.init(
    dsn=os.getenv('SENTRY_DSN'),
    integrations=[DjangoIntegration()],
    traces_sample_rate=0.1,
    environment=os.getenv('ENVIRONMENT', 'development'),
)
```

---

## Accessibility (WCAG 2.1 AA)

### Frontend a11y Implementation
```javascript
// Semantic HTML
<main role="main" aria-label="Main content">
  <section aria-labelledby="lessons-heading">
    <h1 id="lessons-heading">My Lessons</h1>
    {/* Content */}
  </section>
</main>

// ARIA labels
<button aria-label="Add new lesson plan" onClick={handleAdd}>
  <PlusIcon />
</button>

// Focus management
import { useRef, useEffect } from 'react';

function Modal({ isOpen, onClose }) {
  const modalRef = useRef(null);

  useEffect(() => {
    if (isOpen) {
      modalRef.current?.focus();
    }
  }, [isOpen]);

  return (
    <dialog
      ref={modalRef}
      onKeyDown={(e) => e.key === 'Escape' && onClose()}
    >
      {/* Modal content */}
    </dialog>
  );
}

// Skip links
<header>
  <a href="#main-content" className="sr-only">
    Skip to main content
  </a>
  {/* Navigation */}
</header>
```

---

## Deployment & Infrastructure

### Docker Configuration
```dockerfile
# Dockerfile (Backend)
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["gunicorn", "config.wsgi", "--bind", "0.0.0.0:8000", "--workers", "4"]
```

### Environment Validation at Startup
```python
# apps/core/apps.py
from django.apps import AppConfig
import os

class CoreConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'apps.core'
    
    def ready(self):
        # Validate required environment variables
        required_env_vars = [
            'DATABASE_URL',
            'SUPABASE_URL',
            'GOOGLE_GEMINI_API_KEY',
            'SECRET_KEY',
        ]
        
        missing_vars = [var for var in required_env_vars if not os.getenv(var)]
        
        if missing_vars:
            raise EnvironmentError(
                f"Missing required environment variables: {', '.join(missing_vars)}"
            )
```

---

## Testing

### Frontend Testing
```javascript
// Example test with React Testing Library
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import LessonForm from '@/components/LessonForm';

describe('LessonForm', () => {
  it('submits form data', async () => {
    const user = userEvent.setup();
    const handleSubmit = vi.fn();
    
    render(<LessonForm onSubmit={handleSubmit} />);
    
    const titleInput = screen.getByLabelText(/title/i);
    await user.type(titleInput, 'New Lesson');
    
    const submitButton = screen.getByRole('button', { name: /submit/i });
    await user.click(submitButton);
    
    expect(handleSubmit).toHaveBeenCalled();
  });
});
```

### Backend Testing
```python
# Example test with pytest
import pytest
from django.test import TestCase
from apps.lessons.models import LessonPlan

@pytest.mark.django_db
def test_create_lesson_plan():
    lesson = LessonPlan.objects.create(
        title='Test Lesson',
        subject='Math',
    )
    assert lesson.id is not None
    assert lesson.status == 'draft'
```

---

## Documentation

- **API Documentation**: Auto-generated with drf-spectacular
  - Swagger UI at `/api/schema/swagger-ui/`
  - ReDoc at `/api/schema/redoc/`
- **Backend Docs**: Docstrings on all views and models
- **Frontend Docs**: Storybook for components
- **Architecture Decision Records (ADR)**: In `/docs/adr/`
- **Deployment Guide**: In `/docs/deployment.md`

---

## Free Tier Limits & Considerations

### Supabase Free Tier
- **Database**: 500 MB storage
- **File Storage**: 1 GB
- **Realtime**: Limited connections
- **Auth**: Unlimited users
- **API Rate**: Generous limits

### AI Providers Free Tiers
- **Gemini**: 60 calls/minute, unlimited
- **HuggingFace**: 30,000 calls/month
- **Groq**: 1,440 calls/day
- **Together.ai**: Free tier available

### Other Services
- **WAWP**: Pay-per-message ($0.001-0.01 per message in Nigeria)
- **SendGrid**: 100 emails/day
- **Brevo**: 300 emails/day
- **n8n Cloud**: 600 executions/month
- **Sentry**: 5,000 events/month free tier

---

## Scaling Considerations

When scaling beyond MVP:
- Migrate to managed services (AWS RDS, S3)
- Implement caching layer (Redis)
- Use Celery + Redis for background jobs
- Consider GraphQL for complex data fetching
- Implement WebSockets for real-time features (Channels)
- Set up CDN for static assets
- Database replication for high availability
- Add more AI providers or fine-tuned models
- Implement analytics service (Mixpanel, Amplitude)
- Message queue for inter-service communication
- Containerization and orchestration (Kubernetes)

---

## Version History & Changelog

**Last Updated**: 2026-06-15  
**Next Review**: Weekly sprint planning  
**Maintainers**: @dennisukpong

### v2.0 (2026-06-15) - Comprehensive update with omissions
- Added async/concurrency support (Daphne, async views)
- Added comprehensive error handling and resilience
- Added performance optimization patterns
- Added accessibility (a11y) implementation guide
- Added offline detection and retry logic
- Added cross-tab communication
- Added rich text editor support
- Added comprehensive monitoring and telemetry
- Added token security best practices
- Added file upload with progress tracking
- Added search and filtering infrastructure
- Added database connection pooling info
- Added Celery Flower monitoring
- Added structured logging
- Added frontend and backend testing details

### v1.0 (2026-06-15) - Initial stack documentation

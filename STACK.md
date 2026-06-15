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

### HTTP Client
- **Axios** - Promise-based HTTP client for API communication
  - Interceptors for auth token management
  - Request/response transformers
  - Error handling middleware

### State Management & Data Fetching
- **Zustand** - Lightweight state management (open-source)
- **Fetch API** + Custom hooks - For caching layer
- **LocalStorage/SessionStorage** - Client-side data persistence

### Form Management
- **React Hook Form** - Lightweight form state management
- **Zod** - Schema validation library (open-source, TypeScript-first)

### UI Components & Styling
- **Shadcn/ui** - Headless React components (open-source)
- **Recharts** - Data visualization library (open-source)
- **Lucide React** - Icon library (open-source)
- **Sonner** - Toast notifications (open-source)

### Tables & Data Display
- **TanStack Table (React Table)** - Headless table library (open-source)
- **Framer Motion** - Animation library (free tier available)

### File Handling
- **React Dropzone** - File upload component (open-source)
- **xlsx** or **papaparse** - CSV/Excel parsing for bulk uploads (open-source)
- **JSZip** - ZIP file handling for bulk exports (open-source)

### Development Tools
- **ESLint** - Code linting (open-source)
- **Prettier** - Code formatting (open-source)
- **Vitest** - Unit testing (open-source, faster than Jest)
- **Playwright** - E2E testing (open-source)
- **Husky** - Git hooks (open-source)

### Deployment
- **Vercel** - Free tier for frontend deployment
- **GitHub Pages** - Alternative free option
- **Netlify** - Free tier available

---

## Backend Stack

### Core Framework
- **Django 4.2+** - Python web framework (open-source)
- **Django REST Framework** - Building REST APIs (open-source)
- **Python 3.10+** - Runtime environment

### API Development
- **DRF (Django REST Framework)** - REST API toolkit
  - Serializers for data validation
  - ViewSets and Routers for auto-routing
  - Authentication (Token-based, JWT)
  - Permissions and throttling
- **drf-spectacular** - OpenAPI 3.0 schema generation (open-source)
- **django-cors-headers** - CORS configuration (open-source)

### Database & ORM
- **Supabase** (PostgreSQL 14+) - Managed database
  - Free tier: 500 MB storage
  - 2 million monthly egress
  - Realtime capabilities
- **PostgreSQL Adapter**: psycopg2-binary or psycopg
- **Django ORM** - Built-in object-relational mapping

### Authentication & Security
- **Django Allauth** - User authentication and social auth (open-source)
  - Email verification
  - Password reset
  - Multi-backend support
- **djangorestframework-simplejwt** - JWT authentication (open-source)
- **django-ratelimit** - Rate limiting (open-source)
- **django-cors-headers** - CORS handling (open-source)

### File Storage & Management
- **Supabase Storage** - Cloud object storage (free tier included)
  - File upload/download
  - CDN delivery
  - Built-in security policies
- **django-storages** - Storage backend abstraction (open-source)
- **Pillow** - Image processing and optimization (open-source)

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
        'priority': 1
    },
    'secondary': {
        'name': 'huggingface',
        'rate_limit': 30000,  # calls/month
        'model': 'mistralai/Mistral-7B-Instruct-v0.1',
        'priority': 2
    },
    'tertiary': {
        'name': 'groq',
        'rate_limit': 1440,  # calls/day
        'model': 'mixtral-8x7b-32768',
        'priority': 3
    },
    'quaternary': {
        'name': 'together',
        'rate_limit': 'free_tier',
        'model': 'togethercomputer/llama-2-70b-chat',
        'priority': 4
    }
}

async def generate_lesson_plan(prompt, fallback=True):
    """
    Try primary provider, fallback to others if needed
    """
    for provider in sorted(AI_PROVIDERS.values(), key=lambda x: x['priority']):
        try:
            result = await call_provider(provider, prompt)
            return result
        except RateLimitError:
            if not fallback:
                raise
            continue
        except Exception as e:
            logger.error(f"Provider {provider['name']} failed: {e}")
            continue
    
    raise Exception("All AI providers exhausted")
```

### Email & Notifications
- **Django's built-in email backend** - SMTP configuration
  - Gmail SMTP (free tier)
  - SendGrid (12,000 emails/month free)
  - Brevo (formerly Sendinblue) - 300 emails/day free
- **Celery** - Async task queue (open-source)
- **Redis** - Message broker for Celery (free self-hosted)
- **django-celery-beat** - Scheduled tasks (open-source)

### WhatsApp Notifications (WAWP)
- **WAWP (WhatsApp Webhook Provider)** - Open-source WhatsApp integration
  - Send messages via official WhatsApp Business API
  - Webhook support for incoming messages
  - Template messages
  - Document/image/media support

#### **WAWP Setup**
```python
# settings.py
WAWP_CONFIG = {
    'phone_number': '234XXXXXXXXXX',  # Nigeria number
    'api_key': os.getenv('WAWP_API_KEY'),
    'webhook_url': os.getenv('WAWP_WEBHOOK_URL'),
}

# Use case: Notify teachers about approvals
from wawp import WhatsAppClient

client = WhatsAppClient(
    phone_number=settings.WAWP_CONFIG['phone_number'],
    api_key=settings.WAWP_CONFIG['api_key']
)

# Send lesson approval notification
message = f"""
Hello {teacher_name},

Your lesson plan "{lesson_title}" has been approved!

View: {lesson_url}
"""

client.send_message(
    to=teacher_phone,
    message=message,
    template='lesson_approved'  # Pre-approved template
)
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
```

### Logging & Monitoring
- **Python logging** - Built-in logging module
- **Sentry** (open-source) - Error tracking and monitoring
  - Self-hosted version available
  - Free tier on SaaS: 5,000 events/month
- **Django Debug Toolbar** - Development debugging (open-source)

### Testing
- **pytest** - Testing framework (open-source, more flexible than unittest)
- **pytest-django** - Django integration (open-source)
- **factory-boy** - Test data factories (open-source)
- **coverage** - Code coverage measurement (open-source)

### Development & DevOps
- **Django-environ** - Environment configuration (open-source)
- **Gunicorn** - WSGI application server (open-source)
- **Whitenoise** - Static file serving (open-source)
- **Black** - Code formatter (open-source)
- **Flake8** - Linting (open-source)
- **isort** - Import sorting (open-source)

### Performance Optimization
- **Django Caching Framework** - Built-in caching
  - Redis cache backend (free self-hosted)
  - Database caching
- **django-ratelimit** - API rate limiting (open-source)
- **drf-spectacular** - API schema caching (open-source)
- **select_related()** / **prefetch_related()** - Query optimization

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
  whatsapp_number VARCHAR(20),  -- NEW: For WhatsApp notifications
  logo_url TEXT,
  status VARCHAR(20) DEFAULT 'active', -- active, trial, suspended, inactive
  subscription_plan VARCHAR(50), -- basic, standard, premium
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
  whatsapp_number VARCHAR(20),  -- NEW: For WhatsApp notifications
  role VARCHAR(50), -- teacher, supervisor, school_admin, platform_admin, super_admin
  status VARCHAR(20) DEFAULT 'active',
  is_email_verified BOOLEAN DEFAULT FALSE,
  is_whatsapp_verified BOOLEAN DEFAULT FALSE,  -- NEW
  notifications_enabled BOOLEAN DEFAULT TRUE,  -- NEW
  last_login TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Curriculum
CREATE TABLE curriculum (
  curriculum_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id) ON DELETE CASCADE,
  subject VARCHAR(100) NOT NULL,
  class_level VARCHAR(50), -- JSS 1, JSS 2, JSS 3, SS 1, SS 2, SS 3
  topic VARCHAR(255) NOT NULL,
  standards TEXT, -- Learning outcomes/standards
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
  term VARCHAR(20), -- Term 1, 2, 3
  week INTEGER,
  objectives TEXT,
  introduction TEXT,
  main_content JSONB, -- Rich content structure
  activities JSONB,
  assessment TEXT,
  resources TEXT,
  duration_minutes INTEGER,
  status VARCHAR(50) DEFAULT 'draft', -- draft, submitted, approved, rejected, published
  ai_generated BOOLEAN DEFAULT FALSE,
  ai_provider_used VARCHAR(100), -- gemini, huggingface, groq, together
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
  approval_status VARCHAR(50), -- pending, approved, rejected
  feedback TEXT,
  submitted_at TIMESTAMP DEFAULT NOW(),
  reviewed_at TIMESTAMP
);

-- Subscriptions
CREATE TABLE subscriptions (
  subscription_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id) ON DELETE CASCADE,
  plan VARCHAR(50), -- basic, standard, premium
  status VARCHAR(50) DEFAULT 'active', -- active, pending, expired, suspended, cancelled
  amount_paid DECIMAL(10, 2),
  payment_reference VARCHAR(255),
  billing_cycle VARCHAR(20), -- monthly, annual
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
  ai_provider VARCHAR(100),  -- gemini, huggingface, groq, together
  ai_model VARCHAR(100),
  tokens_used INTEGER,
  cost_estimated DECIMAL(8, 4),
  status VARCHAR(50), -- success, failed, throttled
  error_message TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Audit Logs
CREATE TABLE audit_logs (
  log_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id),
  user_id INTEGER REFERENCES users(user_id),
  action VARCHAR(255),
  resource_type VARCHAR(100), -- lesson_plan, user, curriculum, subscription
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
  notification_type VARCHAR(100), -- email, whatsapp, in_app
  channel VARCHAR(50), -- whatsapp, email
  recipient VARCHAR(255),
  message TEXT,
  status VARCHAR(50), -- sent, failed, pending
  error_message TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Support Tickets
CREATE TABLE support_tickets (
  ticket_id SERIAL PRIMARY KEY,
  school_id INTEGER REFERENCES schools(school_id),
  user_id INTEGER REFERENCES users(user_id),
  category VARCHAR(100), -- billing, technical, feature_request, bug
  priority VARCHAR(20), -- low, medium, high, critical
  status VARCHAR(50) DEFAULT 'open', -- open, in_progress, resolved, closed
  title VARCHAR(255),
  description TEXT,
  assigned_to INTEGER REFERENCES users(user_id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Flagged Content
CREATE TABLE flagged_content (
  flag_id SERIAL PRIMARY KEY,
  content_type VARCHAR(100), -- lesson_plan, comment
  content_id INTEGER,
  reason VARCHAR(255), -- inappropriate, plagiarism, spam
  flagged_by INTEGER REFERENCES users(user_id),
  status VARCHAR(50) DEFAULT 'pending', -- pending, approved, rejected, false_alarm
  created_at TIMESTAMP DEFAULT NOW()
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
CREATE INDEX idx_curriculum_school ON curriculum(school_id);
CREATE INDEX idx_subscriptions_school ON subscriptions(school_id);
CREATE INDEX idx_audit_logs_school ON audit_logs(school_id);
CREATE INDEX idx_ai_gen_logs_user ON ai_generation_logs(user_id);
CREATE INDEX idx_notification_logs_user ON notification_logs(user_id);
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
Process: Format response, log provider used
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
Send via WAWP: WhatsAppClient.send_message()
  ↓
Log: Save to notification_logs table
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
}

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=5),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=1),
    'ROTATE_REFRESH_TOKENS': True,
}
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
```

**Lesson Plans (Teachers):**
```
GET    /api/lesson-plans/               # List user's lesson plans
POST   /api/lesson-plans/               # Create new lesson plan
GET    /api/lesson-plans/{id}/          # Get lesson plan detail
PATCH  /api/lesson-plans/{id}/          # Update lesson plan
DELETE /api/lesson-plans/{id}/          # Delete lesson plan
POST   /api/lesson-plans/{id}/submit/   # Submit for approval
POST   /api/lesson-plans/{id}/ai-generate/  # Generate with AI (calls n8n)
GET    /api/lesson-plans/{id}/versions/ # Get version history
```

**Approvals (Supervisors):**
```
GET    /api/approvals/                  # Pending approvals queue
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
```

**Users Management (School Admin):**
```
GET    /api/users/                      # List school users
POST   /api/users/                      # Create new user
GET    /api/users/{id}/                 # Get user detail
PATCH  /api/users/{id}/                 # Update user
DELETE /api/users/{id}/                 # Delete user
POST   /api/users/bulk-upload/          # Bulk import CSV
POST   /api/users/{id}/reset-password/  # Reset password
PATCH  /api/users/{id}/whatsapp/        # Update WhatsApp settings
```

**Subscriptions (Admin):**
```
GET    /api/subscriptions/              # View subscription
POST   /api/subscriptions/upgrade/      # Upgrade plan
GET    /api/billing/invoices/           # List invoices
```

**Analytics (Admin):**
```
GET    /api/analytics/dashboard/        # Dashboard stats
GET    /api/analytics/lesson-trends/    # Lesson creation trends
GET    /api/analytics/curriculum-coverage/  # Coverage metrics
```

**File Upload:**
```
POST   /api/upload/                     # Upload file
GET    /api/files/{id}/                 # Download file
DELETE /api/files/{id}/                 # Delete file
```

**Notifications:**
```
GET    /api/notifications/              # Get notification history
POST   /api/notifications/send-whatsapp/ # Manual WhatsApp send (admin)
PATCH  /api/notifications/preferences/  # Update notification settings
```

---

## Frontend - Axios Configuration

### HTTP Client Setup
```javascript
// api/axiosClient.js
import axios from 'axios';
import store from '../store'; // Zustand store

const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:8000/api';

const axiosClient = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - Add auth token
axiosClient.interceptors.request.use(
  (config) => {
    const token = store.getState().auth.accessToken;
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor - Handle token refresh
axiosClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;

    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      try {
        const refreshToken = store.getState().auth.refreshToken;
        const response = await axios.post(`${API_BASE_URL}/auth/refresh/`, {
          refresh: refreshToken,
        });
        store.setState({ auth: response.data });
        return axiosClient(originalRequest);
      } catch {
        store.setState({ auth: null }); // Clear auth
        window.location.href = '/login';
      }
    }
    return Promise.reject(error);
  }
);

export default axiosClient;
```

### API Service Layer
```javascript
// services/lessonPlanService.js
import axiosClient from './axiosClient';

export const lessonPlanService = {
  // Get all lesson plans
  async getLessonPlans(params = {}) {
    const { data } = await axiosClient.get('/lesson-plans/', { params });
    return data;
  },

  // Generate with AI (triggers n8n workflow)
  async generateWithAI(payload) {
    const { data } = await axiosClient.post('/lesson-plans/ai-generate/', payload);
    return data;
  },

  // Submit for approval (triggers WhatsApp notification)
  async submitForApproval(id) {
    const { data } = await axiosClient.post(`/lesson-plans/${id}/submit/`);
    return data;
  },

  // Other methods...
};
```

---

## AI Inference Integration Details

### Multi-Provider AI Service Implementation

```python
# backends/ai_service.py
import asyncio
from typing import Optional
import google.generativeai as genai
import requests
import httpx

class AIService:
    """Multi-provider AI inference service with fallback logic"""
    
    def __init__(self):
        self.providers = {
            'gemini': self.call_gemini,
            'huggingface': self.call_huggingface,
            'groq': self.call_groq,
            'together': self.call_together,
        }
        self.rate_limits = {
            'gemini': {'calls': 60, 'window': 60},  # 60/minute
            'huggingface': {'calls': 30000, 'window': 2592000},  # 30k/month
            'groq': {'calls': 1440, 'window': 86400},  # 1440/day
            'together': {'calls': 'unlimited', 'window': None},
        }
    
    async def generate_lesson_plan(self, prompt: str, retry_fallbacks=True) -> dict:
        """Generate lesson plan with fallback providers"""
        providers_to_try = ['gemini', 'huggingface', 'groq', 'together']
        
        for provider_name in providers_to_try:
            try:
                logger.info(f"Attempting {provider_name} for lesson generation")
                result = await self.providers[provider_name](prompt)
                
                # Log successful generation
                AIGenerationLog.objects.create(
                    ai_provider=provider_name,
                    ai_model=result.get('model'),
                    tokens_used=result.get('tokens', 0),
                    status='success'
                )
                
                return {
                    'content': result['content'],
                    'provider': provider_name,
                    'model': result.get('model'),
                    'tokens': result.get('tokens', 0)
                }
            
            except RateLimitError:
                logger.warning(f"{provider_name} rate limited, trying next")
                if not retry_fallbacks:
                    raise
                continue
            
            except Exception as e:
                logger.error(f"{provider_name} failed: {str(e)}")
                continue
        
        raise AIGenerationFailedError("All AI providers exhausted")
    
    async def call_gemini(self, prompt: str) -> dict:
        """Google Generative AI (Gemini)"""
        genai.configure(api_key=settings.GOOGLE_GEMINI_API_KEY)
        model = genai.GenerativeModel('gemini-1.5-flash')
        
        response = model.generate_content(prompt)
        
        return {
            'content': response.text,
            'model': 'gemini-1.5-flash',
            'tokens': len(prompt.split()) + len(response.text.split())
        }
    
    async def call_huggingface(self, prompt: str) -> dict:
        """HuggingFace Inference API"""
        url = "https://api-inference.huggingface.co/models/mistralai/Mistral-7B-Instruct-v0.1"
        headers = {"Authorization": f"Bearer {settings.HUGGINGFACE_API_KEY}"}
        
        payload = {
            "inputs": prompt,
            "parameters": {
                "max_new_tokens": 1000,
                "temperature": 0.7
            }
        }
        
        async with httpx.AsyncClient() as client:
            response = await client.post(url, json=payload, headers=headers)
            result = response.json()
        
        return {
            'content': result[0]['generated_text'],
            'model': 'mistral-7b-instruct',
            'tokens': len(prompt.split()) + 1000
        }
    
    async def call_groq(self, prompt: str) -> dict:
        """Groq Cloud API"""
        from groq import Groq
        
        client = Groq(api_key=settings.GROQ_API_KEY)
        
        message = client.chat.completions.create(
            model="mixtral-8x7b-32768",
            messages=[
                {"role": "user", "content": prompt}
            ],
            max_tokens=1000,
        )
        
        return {
            'content': message.choices[0].message.content,
            'model': 'mixtral-8x7b-32768',
            'tokens': message.usage.total_tokens
        }
    
    async def call_together(self, prompt: str) -> dict:
        """Together.ai API"""
        import together
        
        together.api_key = settings.TOGETHER_API_KEY
        
        response = together.Complete.create(
            prompt=prompt,
            model="togethercomputer/llama-2-70b-chat",
            max_tokens=1000,
            temperature=0.7,
        )
        
        return {
            'content': response['output']['choices'][0]['text'],
            'model': 'llama-2-70b-chat',
            'tokens': len(prompt.split()) + 1000
        }

# Usage in views
@api_view(['POST'])
@permission_classes([IsAuthenticated])
async def generate_lesson_plan(request):
    """API endpoint to trigger AI lesson plan generation"""
    prompt = request.data.get('prompt')
    
    ai_service = AIService()
    result = await ai_service.generate_lesson_plan(prompt)
    
    # Trigger n8n webhook with result
    await trigger_n8n_webhook('lesson_generated', result)
    
    return Response(result)
```

### Environment Variables
```bash
# .env
GOOGLE_GEMINI_API_KEY=your_gemini_key
HUGGINGFACE_API_KEY=your_hf_key
GROQ_API_KEY=your_groq_key
TOGETHER_API_KEY=your_together_key

WAWP_API_KEY=your_wawp_key
WAWP_PHONE_NUMBER=234XXXXXXXXXX

N8N_WEBHOOK_URL=https://your-n8n-instance.com/webhook/
```

---

## WhatsApp Integration Details

### WAWP Setup & Configuration

```python
# settings.py
WAWP_CONFIG = {
    'api_key': os.getenv('WAWP_API_KEY'),
    'phone_number': os.getenv('WAWP_PHONE_NUMBER'),  # Business account number
    'webhook_url': os.getenv('WAWP_WEBHOOK_URL'),
}

# services/whatsapp_service.py
from wawp import WhatsAppClient

class WhatsAppService:
    def __init__(self):
        self.client = WhatsAppClient(
            api_key=settings.WAWP_CONFIG['api_key'],
            phone_number=settings.WAWP_CONFIG['phone_number']
        )
    
    async def send_lesson_approval(self, user, lesson):
        """Send WhatsApp notification when lesson is approved"""
        message = f"""
👋 Hello {user.first_name},

Your lesson plan has been approved! ✅

📚 Lesson: {lesson.title}
👨‍🏫 Subject: {lesson.subject}
📅 Class: {lesson.class_level}

👉 View: {settings.FRONTEND_URL}/lesson/{lesson.id}

Questions? Reply to this message.

Best regards,
EduPlan AI Team
        """
        
        await self.client.send_message(
            to=user.whatsapp_number,
            message=message,
            template='lesson_approved'
        )
        
        # Log notification
        NotificationLog.objects.create(
            user=user,
            notification_type='lesson_approved',
            channel='whatsapp',
            recipient=user.whatsapp_number,
            message=message,
            status='sent'
        )
    
    async def send_bulk_notification(self, users, message_template, context):
        """Send to multiple users"""
        for user in users:
            if user.whatsapp_number and user.notifications_enabled:
                message = message_template.format(**context)
                await self.send_message(user.whatsapp_number, message)

# signals.py - Auto-send on events
from django.db.models.signals import post_save
from django.dispatch import receiver

@receiver(post_save, sender=LessonPlanApproval)
def send_approval_notification(sender, instance, created, **kwargs):
    if created:
        whatsapp_service = WhatsAppService()
        asyncio.run(
            whatsapp_service.send_lesson_approval(
                instance.submitted_by,
                instance.lesson_id
            )
        )
```

### WhatsApp Message Templates
```
1. Lesson Approved
   "Your lesson [title] has been approved ✅"

2. Lesson Rejected
   "Your lesson [title] needs revision"

3. Subscription Expiring
   "Your subscription expires in 7 days ⏰"

4. Curriculum Alert
   "Curriculum coverage below 50% ⚠️"

5. New User Invitation
   "You've been invited to EduPlan AI 🎓"

6. AI Generation Ready
   "Your lesson plan is ready for review 🚀"
```

---

## Free Tier Limits & Considerations

### Supabase Free Tier
- **Database**: 500 MB storage
- **File Storage**: 1 GB
- **Realtime**: Limited connections
- **Auth**: Unlimited users
- **API Rate**: Generous limits

### AI Providers Free Tiers
- **Gemini**: 60 calls/minute
- **HuggingFace**: 30,000 calls/month
- **Groq**: 1,440 calls/day
- **Together.ai**: Free tier available

### WAWP
- **Messages**: Pay-per-message model (typically $0.001-0.01 per message in Nigeria)
- **Alternative**: Self-hosted Twilio integration

### Email (Free Options)
- **SendGrid**: 100 emails/day
- **Brevo**: 300 emails/day
- **Gmail SMTP**: 500 emails/day

### Deployment (Free Tier)
- **Frontend**: Vercel (free tier) or Netlify
- **Backend**: Railway (5 GB bandwidth/month), Render, or self-hosted
- **Database**: Supabase (included)
- **Storage**: Supabase (included)
- **Automation**: n8n Cloud (600 executions/month) or self-hosted

---

## Local Development Setup

### Prerequisites
```bash
# Install Python 3.10+
python --version

# Install Node.js 16+
node --version
```

### Backend Setup
```bash
# Clone repository
git clone <repo-url>
cd lesson-plan-automation

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env file
cp .env.example .env

# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Start Django server
python manage.py runserver
```

### Frontend Setup
```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Create .env file
cp .env.example .env.local

# Start development server
npm run dev
```

### Environment Variables

**Backend (.env):**
```
DEBUG=True
SECRET_KEY=your-secret-key
DATABASE_URL=postgresql://user:password@localhost/lesson_plan_db
SUPABASE_URL=https://xxxxx.supabase.co
SUPABASE_KEY=xxxxx

# AI Providers
GOOGLE_GEMINI_API_KEY=xxxxx
HUGGINGFACE_API_KEY=xxxxx
GROQ_API_KEY=xxxxx
TOGETHER_API_KEY=xxxxx

# WhatsApp
WAWP_API_KEY=xxxxx
WAWP_PHONE_NUMBER=234XXXXXXXXXX

# Email
SENDGRID_API_KEY=xxxxx

# n8n
N8N_WEBHOOK_URL=https://n8n.example.com/webhook/

CORS_ALLOWED_ORIGINS=http://localhost:3000
JWT_SECRET=your-jwt-secret
```

**Frontend (.env.local):**
```
REACT_APP_API_URL=http://localhost:8000/api
REACT_APP_SUPABASE_URL=https://xxxxx.supabase.co
REACT_APP_SUPABASE_KEY=xxxxx
```

---

## Deployment Guide

### Backend Deployment (Railway/Render)

**1. Prepare Django for Production:**
```python
# settings.py
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']

# HTTPS
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

**2. Create Procfile:**
```
web: gunicorn config.wsgi --log-file -
release: python manage.py migrate
```

**3. Deploy to Railway/Render:**
- Connect GitHub repository
- Set environment variables
- Deploy

### Frontend Deployment (Vercel)

**1. Connect GitHub to Vercel**
**2. Set environment variables**
**3. Deploy (automatic on push to main)**

---

## Performance Optimization Tips

### Backend
- Use `select_related()` and `prefetch_related()` for queries
- Enable database connection pooling
- Cache API responses with Redis
- Use pagination for large datasets
- Add database indexes on frequently queried fields
- Implement async with Celery for AI tasks

### Frontend
- Code splitting with React.lazy()
- Image optimization with Supabase CDN
- Debounce API calls
- Implement request caching with Axios
- Use virtual scrolling for large lists

---

## Security Best Practices

### Backend
- Use environment variables for secrets
- Enable CORS properly
- Implement rate limiting
- Hash passwords with Django's built-in tools
- Use HTTPS in production
- Implement audit logging
- Validate WhatsApp numbers before sending

### Frontend
- Store tokens securely (httpOnly cookies preferred)
- Validate user input
- Implement CSRF protection
- Use Content Security Policy headers
- Sanitize user-generated content

---

## Testing Strategy

### Backend Testing
```bash
pytest --cov=apps tests/
```

### Frontend Testing
```bash
npm run test
```

### E2E Testing
```bash
npx playwright test
```

---

## Monitoring & Logging

- **Django Logging**: Built-in Python logging to files
- **Sentry** (optional): Free self-hosted or 5k events/month SaaS
- **Server Monitoring**: Railway/Render built-in metrics
- **Frontend Errors**: Implement custom error boundary
- **WhatsApp Delivery**: Track via notification_logs table

---

## Future Scaling Considerations

When scaling beyond MVP:
- Migrate to managed services (AWS RDS, S3)
- Implement caching layer (Redis)
- Use Celery + Redis for background jobs
- Consider GraphQL for complex data fetching
- Implement WebSockets for real-time features
- Set up CDN for static assets
- Database replication for high availability
- Add more AI providers or fine-tuned models
- Implement analytics service (Mixpanel, Amplitude)


# PRD.md
# AI-Powered Lesson Planning & Curriculum Management System (EduPlan AI)

## 1. Product Overview
EduPlan AI is a SaaS platform that automates lesson planning using AI, ensures curriculum alignment, manages academic workflows, and provides centralized educational intelligence for schools.

## 2. Vision
To become the operating system for teaching and learning in schools by combining AI, curriculum management, and workflow automation.

## 3. Monetization Model

### 3.1 Subscription Model (Primary Revenue)
- Schools must pay subscription fees before activation
- Platform admin approves school onboarding after payment confirmation
- Subscription tiers: Basic, Standard, Premium

### 3.2 Advertisement Model (Secondary Revenue)
- School-related vendors can advertise products/services
- Ad placements:
  - Dashboard banners
  - Sidebar ads
  - Resource pages
- Ads are contextual and non-intrusive

## 4. School Lifecycle
- Pending → Payment Confirmed → Active → Expired → Suspended
- Only ACTIVE schools can use full platform features
- Expired schools enter read-only mode

## 5. Core Modules

### 5.1 User Management
- Role-based access: Teacher, Head Teacher, Admin
- Bulk user upload via CSV/XLSX

### 5.2 Curriculum Management
- Structured curriculum storage
- Weekly scheme of work
- Versioning support

### 5.3 AI Lesson Planning Engine
- AI-generated lesson plans
- Curriculum alignment (RAG system)
- Editable outputs

### 5.4 Workflow System
- Draft → Submitted → Reviewed → Approved → Archived
- Supervisor feedback loop

### 5.5 Document System
- PDF/DOCX generation
- Worksheets and assessments
- Secure storage (Supabase/S3)

### 5.6 Analytics
- Curriculum coverage tracking
- Teacher productivity metrics
- Academic reports

### 5.7 Notifications
- WhatsApp, Email, In-app alerts
- n8n automation integration

### 5.8 AI System
- Hybrid AI: Gemini, Groq, Ollama
- Embedding-based curriculum search (pgvector)

## 6. Architecture

Frontend:
- React + Vite

Backend:
- Django + DRF
- Celery for background tasks

Database:
- PostgreSQL (Supabase)

Storage:
- Supabase Storage (MVP)

AI Layer:
- Gemini / Groq / Ollama hybrid

Automation:
- n8n workflows

## 7. Non-Functional Requirements
- Multi-tenant architecture
- Secure school data isolation
- Scalable to 10,000+ schools
- 99.5% uptime target

## 8. Success Metrics
- 80% reduction in lesson planning time
- 90% curriculum coverage accuracy
- High school adoption rate
- Monetization via subscriptions + ads

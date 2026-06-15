# User Flow Documentation: EduPlan AI

This document outlines all user journeys across the EduPlan AI system, covering onboarding, core workflows, subscription management, and administrative functions.

---

## Table of Contents

1. [User Roles & Permissions](#user-roles--permissions)
2. [Platform Admin Workflows](#platform-admin-workflows)
3. [School Admin Workflows](#school-admin-workflows)
4. [Authentication & Onboarding](#authentication--onboarding)
5. [Teacher Workflows](#teacher-workflows)
6. [Supervisor/Admin Workflows](#supervisoradmin-workflows)
7. [Bulk Upload Workflows](#bulk-upload-workflows)
8. [Curriculum Management](#curriculum-management)
9. [Subscription & Billing](#subscription--billing)
10. [AI Features & Integration](#ai-features--integration)
11. [Notifications & Communication](#notifications--communication)
12. [System States & Transitions](#system-states--transitions)

---

## User Roles & Permissions

### Role Hierarchy

```
Super Admin (Platform Level)
    ├── Platform Admin (Support/Operations)
    └── School Level
        ├── School Admin
        │   ├── Supervisor
        │   └── Teacher
```

### Complete Permission Matrix

| Action | Teacher | Supervisor | School Admin | Platform Admin | Super Admin |
|--------|---------|-----------|--------------|----------------|------------|
| **Lesson Management** |
| Create lesson plan | ✅ | ✅ | ✅ | ❌ | ✅ |
| Submit for approval | ✅ | ✅ | ✅ | ❌ | ✅ |
| Approve/Reject lessons | ❌ | ✅ | ✅ | ❌ | ✅ |
| View school dashboard | ✅ | ✅ | ✅ | ❌ | ✅ |
| **User Management** |
| Manage school users | ❌ | ❌ | ✅ | ✅ | ✅ |
| Bulk upload users | ❌ | ❌ | ✅ | ✅ | ✅ |
| Reset user passwords | ❌ | ❌ | ✅ | ✅ | ✅ |
| **Curriculum Management** |
| View curriculum | ✅ | ✅ | ✅ | ✅ | ✅ |
| Manage school curriculum | ❌ | ❌ | ✅ | ❌ | ✅ |
| Bulk upload curriculum | ❌ | ❌ | ✅ | ❌ | ✅ |
| Track coverage | ❌ | ✅ | ✅ | ❌ | ✅ |
| **Subscription & Billing** |
| View subscription (own school) | ❌ | ❌ | ✅ | ✅ | ✅ |
| Manage subscription (own school) | ❌ | ❌ | ✅ | ✅ | ✅ |
| View all subscriptions | ❌ | ❌ | ❌ | ✅ | ✅ |
| Manage all subscriptions | ❌ | ❌ | ❌ | ✅ | ✅ |
| Process refunds | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Analytics & Reporting** |
| School analytics (own school) | ❌ | ✅ | ✅ | ❌ | ✅ |
| Platform analytics | ❌ | ❌ | ❌ | ✅ | ✅ |
| View audit logs (own school) | ❌ | ❌ | ✅ | ✅ | ✅ |
| View platform audit logs | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Platform Management** |
| Manage schools | ❌ | ❌ | ❌ | ✅ | ✅ |
| Suspend/Activate schools | ❌ | ❌ | ❌ | ✅ | ✅ |
| View platform settings | ❌ | ❌ | ❌ | ❌ | ✅ |
| Manage feature flags | ❌ | ❌ | ❌ | ❌ | ✅ |
| Manage system users | ❌ | ❌ | ❌ | ❌ | ✅ |

---

## Platform Admin Workflows

### 1. Dashboard & Overview

```
Platform Admin Login
    ↓
[PLATFORM ADMIN DASHBOARD]
    ├─ Global Statistics (Top Section)
    │   ├─ Total Active Schools: X
    │   ├─ Total Users: Y
    │   ├─ Total Lesson Plans: Z
    │   ├─ Monthly Revenue: ₦XXX,XXX
    │   ├─ Churn Rate: X%
    │   └─ System Health: 99.9% uptime
    ├─ Sections (Left Sidebar)
    │   ├─ Dashboard (current)
    │   ├─ Schools Management
    │   ├─ Subscriptions & Billing
    │   ├─ Users (Platform Users only)
    │   ├─ Support Tickets
    │   ├─ Analytics & Reports
    │   ├─ System Logs & Monitoring
    │   ├─ Content Moderation
    │   └─ Settings & Configuration
    └─ Quick Actions (Right Panel)
        ├─ View Recent Activity
        ├─ Check System Status
        └─ Alert Notifications
    ↓
[DASHBOARD LOADED]
```

---

### 2. Schools Management

```
Platform Admin → Schools Management
    ↓
[SCHOOLS LIST VIEW]
    ├─ Search & Filter
    │   ├─ Search by school name
    │   ├─ Filter by status (active, trial, expired, suspended)
    │   ├─ Filter by region/state
    │   ├─ Filter by subscription plan
    │   ├─ Filter by date range (created_at)
    │   └─ Sort by (name, created_at, users, revenue)
    ├─ Bulk Actions
    │   ├─ Select multiple schools
    │   ├─ [Suspend Selected]
    │   ├─ [Activate Selected]
    │   └─ [Export as CSV]
    └─ Schools Table
        ├─ School Name
        ├─ Status (badge color)
        ├─ Plan (basic/standard/premium)
        ├─ Users Count
        ├─ Lesson Plans Count
        ├─ Subscription End Date
        ├─ Created Date
        └─ Actions Column
    ↓
Click on School Name
    ↓
[SCHOOL DETAIL VIEW]
    ├─ School Info Section
    │   ├─ Name, Address, State, Country
    │   ├─ Contact Email, Phone
    │   ├─ Registration Date
    │   ├─ Status Badge (with change option)
    │   └─ [Edit School] [Suspend] [Archive] buttons
    ├─ Subscription Section
    │   ├─ Current Plan
    │   ├─ Status
    │   ├─ Start/End Dates
    │   ├─ Amount Paid
    │   ├─ Payment Reference
    │   ├─ [View All Subscriptions]
    │   └─ [Manage Subscription] button
    ├─ Users Section
    │   ├─ Total Users: X
    │   ├─ Breakdown (Teachers/Supervisors/Admins)
    │   ├─ Active Users (last 30 days)
    │   └─ [View All Users]
    ├─ Activity Section
    │   ├─ Lesson Plans Created
    │   ├─ AI Generations Used
    │   ├─ Last Activity Date
    │   └─ [View Activity Log]
    ├─ Analytics Section (Quick Stats)
    │   ├─ Curriculum Coverage %
    │   ├─ Avg. Approval Time
    │   ├─ Approval Rate
    │   └─ [View Full Analytics]
    └─ Actions
        ├─ [Send Email to School]
        ├─ [Generate Invoice]
        ├─ [Export School Data]
        ├─ [Suspend School]
        ├─ [Archive School]
        └─ [Back to List]
    ↓
[SCHOOL DETAIL VIEWED]
```

**Data Flow:**
```
GET /api/admin/schools
├─ Query schools table
├─ Apply filters & search
├─ Calculate aggregates (user count, lesson count)
├─ Join with subscriptions table
└─ Return paginated list

GET /api/admin/schools/:school_id
├─ Get school record
├─ Get subscription(s) - last 5
├─ Get user stats
├─ Get activity (from audit_logs)
├─ Calculate analytics
└─ Return detailed view

PATCH /api/admin/schools/:school_id
├─ Validate changes
├─ Update schools record
├─ Log action in audit_logs
├─ Send notification to school admin
└─ Return updated school
```

---

### 3. Subscription Management

```
Platform Admin → Subscriptions & Billing
    ↓
[SUBSCRIPTIONS VIEW]
    ├─ Search & Filter
    │   ├─ Search by school name
    │   ├─ Filter by status (active, pending, expired, suspended)
    │   ├─ Filter by plan (basic/standard/premium)
    │   ├─ Filter by date range (end_date)
    │   └─ Sort by (amount, end_date, status)
    ├─ Bulk Actions
    │   ├─ Select multiple subscriptions
    │   └─ [Send Renewal Reminder] button
    └─ Subscriptions Table
        ├─ School Name
        ├─ Plan
        ├─ Status (badge)
        ├─ Monthly Amount (₦)
        ├─ Start Date
        ├─ End Date
        ├─ Days Until Expiry (color coded)
        ├─ Payment Ref
        └─ Actions
    ↓
Click on Subscription
    ↓
[SUBSCRIPTION DETAIL VIEW]
    ├─ Subscription Info
    │   ├─ School: [name] (link)
    │   ├─ Plan: [plan name]
    │   ├─ Status: [status badge]
    │   ├─ Billing Cycle: Monthly/Annual
    │   ├─ Amount Paid: ₦XXXX
    │   ├─ Payment Reference: [ref]
    │   ├─ Start Date: [date]
    │   ├─ End Date: [date]
    │   ├─ Days Remaining: X
    │   └─ Auto-Renewal: Yes/No
    ├─ Billing History
    │   ├─ List all subscription records for this school
    │   ├─ Show date, plan, amount, status
    │   └─ [View Payment Receipts]
    ├─ Feature Usage (Current Plan)
    │   ├─ Users: 25/100
    │   ├─ Lesson Plans/Month: 350/500
    │   ├─ AI Generations/Month: 450/500
    │   ├─ Curriculum Topics: 890/1000
    │   └─ Storage: 8.5GB/25GB
    ├─ Actions
    │   ├─ [Upgrade Plan] (if not premium)
    │   ├─ [Downgrade Plan] (if not basic)
    │   ├─ [Issue Refund]
    │   ├─ [Extend Expiry Date]
    │   ├─ [Generate Invoice]
    │   ├─ [Resend Confirmation Email]
    │   ├─ [Suspend Subscription]
    │   └─ [Back to List]
    └─ Notes
        └─ Admin notes field
    ↓
[SUBSCRIPTION DETAIL VIEWED]
```

**Data Flow:**
```
GET /api/admin/subscriptions
├─ Query subscriptions table
├─ Join with schools
├─ Apply filters & search
├─ Calculate days until expiry
└─ Return paginated list

GET /api/admin/subscriptions/:subscription_id
├─ Get subscription record
├─ Get school info
├─ Get billing history
├─ Calculate feature usage
├─ Get payment details
└─ Return detail view

POST /api/admin/subscriptions/:id/refund
├─ Validate refund amount
├─ Process refund (call payment gateway)
├─ Create refund record
├─ Update subscription status
├─ Log action in audit_logs
├─ Send receipt email
└─ Return confirmation

POST /api/admin/subscriptions/:id/extend
├─ Update end_date
├─ Log action
├─ Send notification
└─ Return updated subscription
```

---

### 4. Billing & Revenue

```
Platform Admin → Subscriptions & Billing → Invoicing
    ↓
[REVENUE DASHBOARD]
    ├─ Summary Cards (Top)
    │   ├─ Monthly Revenue: ₦XXX,XXX
    │   ├─ Expected Revenue (this month): ₦YYY,YYY
    │   ├─ Pending Payments: ₦ZZZ,ZZZ
    │   ├─ Refunds (this month): ₦AAA,AAA
    │   └─ Churn Loss: ₦BBB,BBB
    ├─ Charts
    │   ├─ Revenue trend (last 12 months)
    │   ├─ Revenue by plan type (pie chart)
    │   ├─ Payment method breakdown
    │   └─ Churn rate trend
    ├─ Tables
    │   ├─ Recent Payments
    │   ├─ Pending Payments (overdue)
    │   ├─ Refunds
    │   └─ Failed Transactions
    └─ Export
        ├─ [Export Monthly Report]
        ├─ [Export Tax Report]
        └─ [Send to Accountant]
    ↓
[REVENUE VIEWED]
```

---

### 5. Support Tickets Management

```
Platform Admin → Support Tickets
    ↓
[TICKETS LIST VIEW]
    ├─ Filter & Status
    │   ├─ Status: Open, In Progress, Resolved, Closed
    │   ├─ Priority: Low, Medium, High, Critical
    │   ├─ Category: Billing, Technical, Feature Request, Bug
    │   ├─ Search by ticket ID or school name
    │   └─ Sort by (created_at, priority)
    ├─ Queue Stats
    │   ├─ Open Tickets: X
    │   ├─ Avg Resolution Time: X hours
    │   ├─ Oldest Ticket: X days old
    │   └─ Assigned to Admin
    └─ Tickets Table
        ├─ Ticket ID
        ├─ School Name (link)
        ├─ Reported By (user name)
        ├─ Category (badge)
        ├─ Priority (badge)
        ├─ Status
        ├─ Created Date
        ├─ Last Updated
        └─ Actions
    ↓
Click on Ticket
    ↓
[TICKET DETAIL VIEW]
    ├─ Ticket Header
    │   ├─ ID, Status, Priority, Category
    │   ├─ Created by: [user] from [school]
    │   ├─ Created Date, Last Updated
    │   └─ Assigned To: [admin name] (change option)
    ├─ Ticket Description
    │   ├─ Full issue description
    │   ├─ Screenshots (if attached)
    │   └─ System info (browser, OS)
    ├─ Conversation Thread
    │   ├─ Show all messages (chronological)
    │   ├─ Admin responses
    │   ├─ Timestamps
    │   └─ Reply field
    ├─ Actions Panel
    │   ├─ [Reply]
    │   ├─ [Resolve Ticket]
    │   ├─ [Assign to Another Admin]
    │   ├─ [Change Priority]
    │   ├─ [Add Internal Note]
    │   ├─ [Send to Developer (if technical)]
    │   └─ [Close Ticket]
    └─ Related Info
        ├─ School Info (link)
        ├─ School Subscription Status
        ├─ Recent School Activity
        └─ [View School Dashboard]
    ↓
Admin Types Response
    ↓
[TICKET RESPONSE SENT]
    └─ Send email notification to school
    ↓
[TICKET STATUS UPDATED]
```

---

### 6. User Management (Platform Users Only)

```
Platform Admin → Users (Platform Admin/Super Admin accounts)
    ↓
[PLATFORM USERS LIST]
    ├─ Search & Filter
    │   ├─ Search by name or email
    │   ├─ Filter by role (super_admin, platform_admin)
    │   ├─ Filter by status (active, inactive, suspended)
    │   ├─ Filter by last login date
    │   └─ Sort by (name, created_at)
    ├─ Table
    │   ├─ Name
    │   ├─ Email
    │   ├─ Role
    │   ├─ Status (badge)
    │   ├─ Last Login
    │   ├─ Created Date
    │   └─ Actions
    └─ [Add New Admin] Button
    ↓
Click on User
    ↓
[ADMIN USER DETAIL]
    ├─ Personal Info
    │   ├─ Name
    │   ├─ Email
    │   ├─ Role (with change option)
    │   ├─ Status
    │   ├─ Last Login
    │   └─ Created Date
    ├─ Permissions
    │   ├─ Display all permissions for this role
    │   └─ Read-only
    ├─ Activity Log
    │   ├─ Recent actions
    │   ├─ Login history
    │   └─ Changes made
    ├─ Actions
    │   ├─ [Change Role]
    │   ├─ [Reset Password]
    │   ├─ [Suspend User]
    │   ├─ [Activate User]
    │   ├─ [Send Email]
    │   └─ [Deactivate]
    └─ [Back to List]
    ↓
[USER DETAIL VIEWED]
```

---

### 7. System Logs & Monitoring

```
Platform Admin → System Logs & Monitoring
    ↓
[SYSTEM HEALTH DASHBOARD]
    ├─ Status Indicators
    │   ├─ API Server: 🟢 Up (Response time: 45ms)
    │   ├─ Database: 🟢 Connected (CPU: 32%, Memory: 64%)
    │   ├─ Email Service: 🟢 Operational (100 sent in last hour)
    │   ├─ AI Services: 🟢 Connected (Model: Gemini)
    │   ├─ Storage (S3/Supabase): 🟢 Operational
    │   └─ Vector DB (pgvector): 🟢 Operational
    ├─ Performance Metrics
    │   ├─ API Uptime: 99.95% (this month)
    │   ├─ Avg Response Time: 145ms
    │   ├─ Peak Load: 2,500 requests/min
    │   ├─ Error Rate: 0.02%
    │   └─ Database Query Time: 85ms avg
    ├─ Recent Alerts
    │   ├─ Last 10 alerts (if any)
    │   ├─ Alert type (Error, Warning, Info)
    │   ├─ Timestamp
    │   └─ [View All Alerts]
    └─ [Advanced Logs]
    ↓
Click on Logs
    ↓
[DETAILED SYSTEM LOGS]
    ├─ Filter
    │   ├─ Log type (error, warning, info, debug)
    │   ├─ Service (api, worker, scheduler, email)
    │   ├─ Date range
    │   └─ Search by message
    ├─ Logs Table
    │   ├─ Timestamp
    │   ├─ Level (severity badge)
    │   ├─ Service
    │   ├─ Message
    │   ├─ Error stack (if error)
    │   └─ Expand icon
    └─ Export
        ├─ [Download Logs (CSV)]
        └─ [Email Report]
    ↓
[LOGS VIEWED]
```

---

### 8. Analytics & Reports

```
Platform Admin → Analytics & Reports
    ↓
[PLATFORM ANALYTICS DASHBOARD]
    ├─ Date Range Selector
    │   ├─ Last 7 days / 30 days / 90 days / Custom
    │   └─ Compare with previous period
    ├─ Key Metrics (Top Cards)
    │   ├─ Active Schools: X
    │   ├─ Total Users: Y
    │   ├─ Total Lesson Plans: Z
    │   ├─ Total AI Generations: A
    │   ├─ Monthly Revenue: ₦B
    │   └─ Churn Rate: C%
    ├─ Growth Metrics
    │   ├─ New Schools (this period)
    │   ├─ New Users (this period)
    │   ├─ New Lesson Plans (this period)
    │   ├─ Revenue Growth %
    │   └─ Churn Change %
    ├─ Charts
    │   ├─ Active Schools Trend (line chart)
    │   ├─ Revenue Trend (line chart)
    │   ├─ Lesson Plans by Subject (bar chart)
    │   ├─ Plan Distribution (pie chart)
    │   ├─ User Distribution by Role (pie chart)
    │   ├─ AI Model Usage (pie chart)
    │   ├─ Approval Rate Trend (line chart)
    │   └─ Curriculum Coverage (bar chart)
    ├─ Tables
    │   ├─ Top 10 Schools (by activity)
    │   ├─ Top 10 Teachers (by lesson plans)
    │   ├─ Plan Breakdown (basic/standard/premium count)
    │   ├─ Regional Distribution (by state)
    │   ├─ Feature Usage Breakdown
    │   └─ Churn Analysis (inactive schools)
    └─ Export
        ├─ [Generate PDF Report]
        ├─ [Export as Excel]
        ├─ [Schedule Email Report]
        └─ [Share Report Link]
    ↓
[ANALYTICS VIEWED]
```

**Data Flow:**
```
GET /api/admin/analytics/overview
├─ Count active schools
├─ Count total users
├─ Sum lesson plans
├─ Sum AI generations
├─ Calculate revenue (sum subscription amounts)
├─ Calculate churn rate
└─ Return metrics

GET /api/admin/analytics/trends
├─ Query audit_logs (date grouped)
├─ Calculate growth metrics
├─ Query subscriptions (revenue trend)
├─ Query lesson_plans (creation trend)
└─ Return time series data
```

---

### 9. Content Moderation

```
Platform Admin → Content Moderation
    ↓
[FLAGGED CONTENT VIEW]
    ├─ Filter & Status
    │   ├─ Status: Pending Review, Approved, Rejected, False Alarm
    │   ├─ Content Type: Lesson Plan, Curriculum, User Report
    │   ├─ Reason: Inappropriate Content, Plagiarism, Spam, Other
    │   ├─ Date Range
    │   └─ Sort by (created_at, priority)
    ├─ Flagged Items List
    │   ├─ Flagged By (user/system)
    │   ├─ Content Preview
    │   ├─ Reason
    │   ├─ Date Flagged
    │   ├─ Status (badge)
    │   └─ Actions
    └─ [View Details]
    ↓
Click on Flagged Item
    ↓
[MODERATION DETAIL]
    ├─ Content Preview
    │   ├─ Full content of flagged item
    │   ├─ Highlight flagged sections
    │   ├─ Metadata (author, date, school)
    │   └─ View history (if edited)
    ├─ Flag Information
    │   ├─ Flagged By: [user/system]
    │   ├─ Reason: [reason]
    │   ├─ Date Flagged: [date]
    │   ├─ Additional Info: [details]
    │   └─ Reporter Comments: [text]
    ├─ Actions
    │   ├─ [Approve Content] (Remove flag)
    │   ├─ [Reject Content] (Delete item)
    │   ├─ [Request Revision] (Send to author)
    │   ├─ [False Alarm] (Mark as system error)
    │   ├─ [Notify Author] (Send warning/explanation)
    │   └─ [Add Admin Note]
    └─ Decision Log
        ├─ Previous moderation decisions (if any)
        └─ Historical context
    ↓
Admin Makes Decision
    ↓
[ACTION EXECUTED]
    ├─ Update content status
    ├─ Log action in audit_logs
    ├─ Send notification to content creator
    ├─ Update flag status
    └─ Alert school admin (if necessary)
    ↓
[MODERATION COMPLETE]
```

---

### 10. Settings & Configuration

```
Platform Admin → Settings & Configuration
    ↓
[SETTINGS PAGE]
    ├─ Left Menu
    │   ├─ General Settings
    │   ├─ Feature Flags
    │   ├─ Email Templates
    │   ├─ Payment Configuration
    │   ├─ AI Models Configuration
    │   ├─ Rate Limits
    │   ├─ System Maintenance
    │   └─ Audit Settings
    └─ [Edit Settings] (Admin Only)
    ↓
Select: General Settings
    ↓
[GENERAL SETTINGS]
    ├─ Platform Name
    ├─ Support Email
    ├─ Help Center URL
    ├─ Terms & Conditions (link to edit)
    ├─ Privacy Policy (link to edit)
    ├─ Default Subscription Plans
    │   ├─ Trial Duration (days)
    │   ├─ Basic Plan Price
    │   ├─ Standard Plan Price
    │   ├─ Premium Plan Price
    │   └─ Annual Discount %
    ├─ Feature Limits
    │   ├─ Min users per school
    │   ├─ Max users per school
    │   └─ Max curriculum topics per school
    ├─ [Save Changes] [Cancel]
    └─ [View Change History]
    ↓
Select: Feature Flags
    ↓
[FEATURE FLAGS]
    ├─ List of all features
    ├─ Toggle each feature on/off
    │   ├─ AI Generation (ON/OFF)
    │   ├─ RAG Search (ON/OFF)
    │   ├─ Bulk Upload (ON/OFF)
    │   ├─ Curriculum Versioning (ON/OFF)
    │   ├─ WhatsApp Notifications (ON/OFF)
    │   ├─ Video Support (ON/OFF)
    │   └─ Mobile App (ON/OFF - beta)
    ├─ Gradual Rollout %
    │   └─ e.g., AI Generation: enabled for 50% of schools
    ├─ Exceptions
    │   ├─ Force enable for: [specific schools]
    │   └─ Force disable for: [specific schools]
    └─ [Save Changes]
    ↓
Select: Payment Configuration
    ↓
[PAYMENT SETTINGS]
    ├─ Stripe Configuration
    │   ├─ API Key (masked)
    │   ├─ Webhook URL
    │   └─ [Test Connection]
    ├─ Paystack Configuration
    │   ├─ Public Key
    │   ├─ Secret Key (masked)
    │   └─ [Test Connection]
    ├─ Bank Transfer
    │   ├─ Account Name
    │   ├─ Account Number
    │   └─ Bank Details
    ├─ [Save Changes]
    └─ [Verify Settings]
    ↓
Select: AI Models Configuration
    ↓
[AI MODELS SETTINGS]
    ├─ Primary Model
    │   ├─ Model: Gemini / Groq / Custom
    │   ├─ API Key (masked)
    │   ├─ Max Tokens per request
    │   ├─ Temperature (0.0 - 1.0)
    │   └─ [Test Model]
    ├─ Fallback Model
    │   ├─ Model: [model]
    │   ├─ API Key (masked)
    │   └─ Enable Fallback on error (yes/no)
    ├─ Embedding Model
    │   ├─ Model: OpenAI / Custom
    │   ├─ API Key (masked)
    │   ├─ Dimensions: 1536 (for OpenAI)
    │   └─ [Test Embedding]
    ├─ [Save Changes]
    └─ [View Usage Statistics]
    ↓
[SETTINGS UPDATED]
```

---

## School Admin Workflows

### Overview

School admins manage everything within their school but cannot access platform-level operations. They have complete control over their school's users, curriculum, subscriptions, and analytics.

---

### 1. School Admin Dashboard

```
School Admin Login
    ↓
[SCHOOL ADMIN DASHBOARD]
    ├─ Welcome Section
    │   ├─ "Welcome back, [Admin Name]"
    │   ├─ School Name: [name]
    │   ├─ Current Subscription: [plan]
    │   ├─ Subscription Expires: [date] (X days remaining)
    │   └─ [Manage Subscription] button
    ├─ Quick Stats (Key Cards)
    │   ├─ Active Users: X
    │   ├─ Lesson Plans (this term): Y
    │   ├─ Curriculum Coverage: Z%
    │   ├─ AI Generations Used: A/B
    │   └─ Storage Used: C GB/D GB
    ├─ Sidebar Menu
    │   ├─ Dashboard (current)
    │   ├─ Users Management
    │   ├─ Curriculum Management
    │   ├─ Lesson Plans (view all)
    │   ├─ Approvals Queue
    │   ├─ Subscription & Billing
    │   ├─ Analytics & Reports
    │   ├─ Notifications
    │   ├─ Audit Logs
    │   ├─ School Settings
    │   └─ Support
    ├─ Center Panel
    │   ├─ Recent Activities
    │   ├─ Pending Approvals (count)
    │   ├─ Upcoming Renewals
    │   ├─ Feature Usage Alerts
    │   └─ System Notifications
    └─ [View Full Analytics] [Contact Support]
    ↓
[DASHBOARD LOADED]
```

---

### 2. Users Management (School-level)

See **Bulk Upload Workflows** section below for detailed user management and bulk import flows.

---

### 3. Curriculum Management (School-level)

See **Curriculum Management** section below for detailed curriculum workflows.

---

### 4. School Settings

```
School Admin → School Settings
    ↓
[SCHOOL SETTINGS PAGE]
    ├─ Left Menu
    │   ├─ General Settings
    │   ├─ Users & Roles
    │   ├─ Notification Preferences
    │   ├─ Billing & Subscription
    │   ├─ Data & Privacy
    │   └─ Support
    └─ Select: General Settings
    ↓
[GENERAL SETTINGS]
    ├─ School Information
    │   ├─ School Name
    │   ├─ Address
    │   ├─ State
    │   ├─ Country
    │   ├─ Phone Number
    │   ├─ Contact Email
    │   ├─ Logo (upload)
    │   └─ School Website (optional)
    ├─ Academic Settings
    │   ├─ School Type (Primary/Secondary)
    │   ├─ Academic Calendar (terms start/end dates)
    │   ├─ Curriculum Version (active)
    │   └─ Classes Offered (JSS 1-3, SS 1-3)
    ├─ [Save Changes] [Cancel]
    └─ [View Change History]
```

---

## Authentication & Onboarding

### 1. School Registration Flow

```
New School
    ↓
Landing Page
    ↓
Sign Up Form (School Details)
    ├─ School Name
    ├─ State/Region
    ├─ Address
    └─ Contact Person Email
    ↓
Email Verification
    ↓
Subscription Plan Selection
    ├─ Free Trial (14 days)
    ├─ Basic Plan
    ├─ Standard Plan
    └─ Premium Plan
    ↓
Account Created
    ├─ school_id generated
    ├─ Subscription created (status: trial/pending)
    ├─ Audit log entry created
    └─ Confirmation email sent
    ↓
[ONBOARDING COMPLETE]
```

**Data Flow:**
```
POST /api/auth/register
├─ Validate school details
├─ Create schools record
├─ Create subscriptions record
├─ Create audit_logs record
├─ Send verification email
└─ Return auth token + school_id
```

---

### 2. User Invitation & Onboarding

```
School Admin
    ↓
Admin Dashboard → Invite User
    ├─ Select Role (teacher/supervisor)
    ├─ Enter Email
    └─ Send Invitation
    ↓
Invitation Email Sent
    ├─ Link to accept invitation
    └─ Expires in 7 days
    ↓
User Clicks Link
    ↓
Sign Up / Set Password
    ├─ Full Name
    ├─ Password
    └─ Accept Terms
    ↓
User Account Created
    ├─ users record created
    ├─ Role assigned (teacher/supervisor/admin)
    ├─ school_id linked
    └─ audit_logs entry
    ↓
[USER ONBOARDING COMPLETE]
    ↓
User Redirected to Dashboard
```

---

### 3. Login Flow

```
User Lands on App
    ↓
Login Page
    ├─ Email
    └─ Password
    ↓
Validate Credentials
    ├─ User exists?
    ├─ Password correct?
    └─ Account active?
    ↓
Check School Status
    ├─ Is school.status = 'active'?
    └─ Is subscription valid?
    ↓
Issue JWT Token + Refresh Token
    ├─ Store in cookies/localStorage
    └─ Set CurrentSchoolMiddleware context
    ↓
Redirect to Dashboard
    ├─ Load user's lesson plans
    ├─ Load curriculum standards
    └─ Load pending approvals (if supervisor)
    ↓
[LOGIN COMPLETE]
```

---

## Teacher Workflows

### 1. Create & Edit Lesson Plan

```
Teacher Dashboard
    ↓
Click "New Lesson Plan"
    ↓
Step 1: Basic Info
    ├─ Subject
    ├─ Class Level (JSS 1, JSS 2, ..., SS 3)
    ├─ Topic
    ├─ Term & Week
    └─ Objectives (text)
    ↓
Step 2: Lesson Content
    ├─ Introduction
    ├─ Main Content (JSONB)
    ├─ Activities
    ├─ Assessment
    ├─ Resources
    └─ Duration (minutes)
    ↓
Step 3: AI Assistance (Optional)
    ├─ [USE AI] Button
    │   ├─ Call AI generation endpoint
    │   ├─ Log request to ai_generation_logs
    │   ├─ Receive generated content
    │   └─ Populate fields
    └─ Manual entry (skip AI)
    ↓
Save as Draft
    ├─ Create lesson_plans record (status: draft)
    ├─ ai_generated: true/false
    ├─ Store content as JSONB
    ├─ Create initial version in lesson_plan_versions
    └─ Redirect to Edit View
    ↓
[DRAFT SAVED]
```

---

### 2. Submit for Approval

```
Teacher in Draft Lesson Plan
    ↓
Click "Submit for Approval"
    ↓
Validation
    ├─ Check all required fields filled
    ├─ Check minimum content length
    └─ Verify no errors
    ↓
Submission Modal
    ├─ Add submission comment (optional)
    └─ Confirm submission
    ↓
Backend Processing
    ├─ Update lesson_plans (status: "submitted")
    ├─ Create lesson_approvals record
    ├─ Create version snapshot
    ├─ Log action in audit_logs
    └─ Trigger notification (supervisors)
    ↓
[SUBMISSION COMPLETE]
```

---

## Supervisor/Admin Workflows

### 1. Approval Queue & Review

```
Supervisor Dashboard
    ↓
Approval Queue Section
    ├─ Pending Approvals (sorted by date)
    ├─ Filter by Subject, Teacher, Priority
    └─ Search function
    ↓
Click on Lesson
    ↓
Review Screen
    ├─ Lesson Content
    ├─ Metadata
    ├─ Comparison (Optional)
    └─ Action Panel ([APPROVE] [REQUEST CHANGES] [REJECT])
    ↓
Supervisor Action
    ├─ Option A: Approve
    ├─ Option B: Request Changes
    └─ Option C: Reject
    ↓
Backend Processing
    ├─ Update lesson_approvals
    ├─ Update lesson_plans (if approved)
    ├─ Create version snapshot
    ├─ Log action
    └─ Trigger notifications
    ↓
[REVIEW COMPLETE]
```

---

## Bulk Upload Workflows

### 1. Bulk User Upload (CSV/XLSX)

```
School Admin Dashboard
    ↓
User Management → Import Users
    ↓
Upload Modal
    ├─ File selector (.csv, .xlsx)
    ├─ Template option (download CSV template)
    └─ [UPLOAD] Button
    ↓
File Validation (Frontend)
    ├─ Check file type
    ├─ Check file size (<5MB)
    └─ Parse first row (headers)
    ↓
Frontend Preview
    ├─ Show parsed data (first 5 rows)
    ├─ Display detected columns
    └─ [CONFIRM IMPORT] or [CANCEL]
    ↓
Backend: File Processing
    ├─ Parse uploaded file
    ├─ Validate each row
    │   ├─ Required fields: full_name, email, role
    │   ├─ Email format valid?
    │   ├─ Role valid? (teacher/supervisor/admin)
    │   ├─ Duplicate email check
    │   └─ Subscription limit check
    └─ Return validation report
    ↓
Backend: Import Execution
    ├─ Create transaction
    ├─ For each valid row:
    │   ├─ Create users record
    │   ├─ Generate temp password
    │   ├─ Log in audit_logs
    │   └─ Queue invitation email
    ├─ Commit transaction
    └─ Return import summary
    ↓
Import Summary Screen
    ├─ "Successfully imported X users"
    ├─ Breakdown by role
    └─ [DONE]
    ↓
[BULK IMPORT COMPLETE]
```

**CSV Template Format:**
```
full_name,email,role
John Doe,john.doe@school.com,teacher
Jane Smith,jane.smith@school.com,supervisor
```

---

### 2. Bulk Curriculum Upload (CSV/XLSX)

```
School Admin Dashboard
    ↓
Curriculum Management → Import Curriculum
    ↓
Upload Modal
    ├─ File selector (.csv, .xlsx)
    ├─ Template option
    ├─ Curriculum version selector
    └─ [UPLOAD] Button
    ↓
File Validation (Frontend)
    ├─ Check file type
    ├─ Check file size (<10MB)
    └─ Parse and preview
    ↓
Backend: File Processing
    ├─ Parse uploaded file
    ├─ Validate each row
    │   ├─ Required fields: subject, class_level, term, week, topic, objectives
    │   ├─ Valid values check
    │   ├─ Duplicate detection
    │   └─ Subscription limit check
    └─ Return validation report
    ↓
Backend: Import Execution
    ├─ Determine version number
    ├─ Create curriculum_standards (batch)
    ├─ Create curriculum_coverage (batch)
    ├─ Create schemes_of_work (if provided)
    ├─ Log bulk import action
    └─ Return import summary
    ↓
Import Summary Screen
    ├─ "Successfully imported X topics"
    ├─ Breakdown by subject
    ├─ Curriculum version created
    └─ [VIEW CURRICULUM]
    ↓
[BULK IMPORT COMPLETE]
```

**CSV Template Format:**
```
subject,class_level,term,week,topic,objectives,sub_topics,activities
Mathematics,JSS 1,1,1,Whole Numbers,"Students will: 1) Identify whole numbers. 2) Perform basic operations.","1) Natural numbers 2) Whole numbers","1) Classify 2) Addition"
```

---

## Curriculum Management

### 1. View Curriculum Standards

```
Admin/Supervisor Dashboard
    ↓
Curriculum Management
    ↓
Curriculum Overview
    ├─ Current version (active)
    ├─ Total topics: X
    ├─ Subjects (with counts)
    └─ Class levels (grid view)
    ↓
[VIEW COMPLETE]
```

---

### 2. Curriculum Linked to Lesson Plans

```
Supervisor Dashboard → Curriculum Coverage
    ↓
Link Lesson Plan to Curriculum
    ├─ Click on uncovered topic
    ├─ Modal: Search lesson plans
    ├─ Select lesson
    ├─ Create link in curriculum_coverage
    └─ Update coverage map
    ↓
[LINK COMPLETE]
```

---

## Subscription & Billing

### 1. Feature Usage Tracking

```
[On Every Feature Usage Action]
    ↓
Endpoint Hit (e.g., POST /api/lessons)
    ↓
Check Feature Limits
    ├─ Query subscriptions.plan for school
    ├─ Query feature_usage (today's date)
    ├─ Compare against tier limit
    ↓
If Usage < Limit:
    ├─ Process action
    ├─ Increment feature_usage.usage_count
    └─ Return success
    ↓
If Usage >= Limit:
    ├─ Deny action
    ├─ Return 403 + upgrade prompt
    └─ Log denial attempt
```

**Feature Limits by Tier:**

| Feature | Free | Basic | Standard | Premium |
|---------|------|-------|----------|---------|
| Users | 5 | 25 | 100 | Unlimited |
| Lesson Plans/Month | 20 | 100 | 500 | Unlimited |
| Curriculum Topics | 50 | 200 | 1000 | Unlimited |
| AI Generations/Month | 5 | 50 | 500 | Unlimited |
| Storage | 1GB | 5GB | 25GB | 100GB |
| Bulk Uploads/Month | 2 | 10 | 50 | Unlimited |
| Support | Community | Email | Priority | Dedicated |

---

## AI Features & Integration

### AI Generation Request Flow

```
Teacher Initiates Generation
    ↓
Frontend: Build prompt context
    ↓
Backend: Validate request
    ├─ Check subscription active
    ├─ Check feature_usage limit
    └─ Check school not suspended
    ↓
Backend: Route to AI service
    ├─ Determine model (based on plan)
    ├─ Build structured prompt
    └─ Call AI API
    ↓
Backend: Log request
    ├─ Insert ai_generation_logs
    ├─ Create vector embedding
    ├─ Increment feature_usage
    └─ Return generated content
    ↓
Frontend: Display & Actions
    ├─ Show generated content
    ├─ Options: Accept, Edit, Regenerate
    └─ Accept → Populate fields
    ↓
[GENERATION COMPLETE]
```

---

## Notifications & Communication

### Notification Types & Triggers

| Event | Recipient | Channels |
|-------|-----------|----------|
| Lesson submitted | Supervisor | In-app, Email |
| Lesson approved | Teacher | In-app, Email |
| Lesson rejected | Teacher | In-app, Email |
| User invited | New User | Email |
| Users bulk imported | Admin | In-app, Email |
| Curriculum bulk imported | Admin | In-app, Email |
| Subscription expiring | Admin | In-app, Email |

---

## System States & Transitions

### 1. School Lifecycle

```
[REGISTRATION]
    ↓
school.status = 'pending'
    ↓
[ACTIVE TRIAL]
    ↓
Trial ends (day 14)
    ├─ Payment succeeds → [ACTIVE SUBSCRIPTION]
    └─ Payment fails → [EXPIRED/SUSPENDED]
    ↓
[ACTIVE SUBSCRIPTION]
    ↓
Renewal date approaches
    ├─ Payment succeeds → Continue service
    └─ Payment fails → [SUSPENDED]
    ↓
[CANCELLED]
```

---

## Summary Diagram: Complete Admin Hierarchy

```
Platform Level
├─ Super Admin
│   └─ Full system control
│       ├─ Manage all schools
│       ├─ Manage platform admins
│       ├─ View all analytics
│       ├─ System configuration
│       └─ Feature flags
├─ Platform Admin
│   ├─ School management
│   ├─ Subscription management
│   ├─ Support tickets
│   ├─ Revenue analytics
│   ├─ System monitoring
│   └─ Content moderation

School Level
├─ School Admin
│   ├─ User management (own school)
│   ├─ Curriculum management (own school)
│   ├─ Subscription management (own school)
│   ├─ School analytics
│   └─ School settings
├─ Supervisor
│   ├─ Lesson approval
│   ├─ Curriculum coverage tracking
│   └─ Teacher performance analytics
└─ Teacher
    ├─ Lesson creation
    ├─ AI assistance
    └─ Lesson submission
```

---

## Key Metrics & Success Criteria

### Platform Admin KPIs
- **Active Schools**: Target 500+
- **Monthly Revenue**: Track by region
- **Churn Rate**: Target <5%
- **Support Ticket Resolution**: <24 hours
- **System Uptime**: Target 99.9%

### School Admin KPIs
- **User Onboarding**: <5 minutes
- **Curriculum Import Success**: >99%
- **Bulk Upload Processing**: <5 seconds
- **Feature Usage Compliance**: 100%

---

**Document Last Updated**: June 2026  
**Version**: 2.0  
**Status**: Approved for Development

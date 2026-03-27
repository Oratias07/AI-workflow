# Fitness Trainer MVP App - Claude Code Prompt

*Complete session logging and billing platform for fitness trainers to track workouts, manage trainees, and handle month-end invoicing and payroll*

## Overview

A mobile-first application designed for fitness trainers to streamline their daily operations. Trainers can quickly log training sessions with biometric login, track multiple trainees across different training tracks, and automate month-end billing and salary calculations. Group managers can oversee multiple trainers, set rates, and generate payment reports. Built with security and data integrity as top priorities.

## Core Features

### 1. Authentication & Access
* Facial recognition login for trainers using iPhone camera (FaceID/face detection API)
* Secure session management with automatic timeouts
* Multi-user support with role-based access (Trainer, Group Manager, Admin)
* Passwordless biometric authentication for ease of use

### 2. Trainer Dashboard
* Quick-access interface to log training sessions (3-4 steps max)
* Form inputs for: trainee name, training track (dropdown), duration/amount per workout
* Real-time session logging with timestamps
* Edit/delete previous session logs within current month
* At-a-glance daily summary and session count

### 3. Training Tracks System
* Predefined training tracks (Strength, Cardio, Flexibility, Sports-Specific)
* Create custom tracks specific to trainer or group
* Group tracks with designated group managers
* Per-track pricing/rate configuration by managers
* Track-specific notes and requirements

### 4. Monthly Session Closing
* Month-end finalization workflow with warning system
* "Summarize and Send" button generates:
  * Total training hours/sessions per trainee
  * Amount owed by trainee (calculated from track rates × hours)
  * Debt notification message
  * Invoice/summary document
* Prevents modification of closed months
* Archive previous months for reference

### 5. Group Manager Features
* Dashboard showing all trainers in their group
* Automatic notifications of trainer deliverables
* Set and adjust salary rates per trainee
* View total salary owed to trainers based on delivered training
* Generate trainer payment reports
* Performance metrics and group analytics

### 6. Analytics & Statistics
* Monthly statistics per track (total hours, sessions, trainees)
* Trainer performance metrics and comparisons
* Group performance insights
* Exportable reports (PDF/CSV)
* Charts/visualizations of training trends over time

### 7. Notifications System
* Push notifications for month-end reminders
* Debt notifications to trainees
* Salary notifications to group managers
* Email summaries of monthly activity
* Configurable notification preferences

## Technical Requirements

### Technology Stack
* Frontend: React Native (iOS-first) or React web with mobile responsiveness
* Backend: Node.js/Express or Python
* Database: PostgreSQL with encryption
* Authentication: Biometric (FaceID) + JWT token-based auth
* Notifications: Firebase Cloud Messaging or Twilio
* File Generation: PDF libraries for invoices and reports

### Security Requirements
* Encrypt sensitive data at rest (trainer IDs, trainee info, financial data)
* Implement role-based access control (RBAC) at API level
* Secure API endpoints with authentication middleware
* Session timeouts for biometric login (15-30 min idle)
* Audit logging for all transactions and access
* GDPR-compliant data handling and retention policies
* Secure password reset mechanisms for secondary auth
* Financial transaction logging and reconciliation

### Project Structure
```
├── src/
│   ├── components/
│   │   ├── Dashboard/
│   │   ├── SessionLogger/
│   │   ├── TrackManager/
│   │   └── Reports/
│   ├── screens/
│   ├── services/
│   ├── utils/
│   ├── config/
│   └── hooks/
├── server/
│   ├── routes/
│   ├── controllers/
│   ├── models/
│   ├── middleware/
│   ├── utils/
│   └── config/
├── database/
│   ├── migrations/
│   └── seeds/
├── tests/
├── docs/
└── README.md
```

## Deliverables

### 1. Complete Application Code
* Full frontend implementation (mobile-responsive)
* Backend API with all endpoints
* Database schema and migrations
* Authentication system with biometric support
* Session logging and billing engines

### 2. Configuration Files
* `.env.example` with all required variables
* `database.config.js` with connection settings
* `app.config.js` with app-wide settings (rates, notification settings)
* `auth.config.js` with security and session settings
* `notifications.config.js` for push notification setup

### 3. Documentation
* API documentation (list all endpoints)
* Database schema diagram
* Setup and deployment instructions
* User guide for trainers and managers
* Security best practices document
* Integration guide for payment systems

### 4. Database Schema
* Users - Account info with roles (trainer, manager, admin)
* Trainees - Trainee profiles and contact info
* TrainingTracks - Track names, types, and pricing
* TrainingSessions - Session logs with timestamps, durations, amounts
* Groups - Group management and hierarchy
* Invoices/Bills - Generated billing documents
* Notifications - Notification history and status
* AuditLogs - All transactions and access logs

## UI/UX Specifications
* Clean, modern, trainer-friendly interface
* Minimal clicks to log a session (target: 3-4 steps max)
* Large, readable text and buttons for quick use
* Dark mode support for low-light gym environments
* Offline capability for session logging (sync when online)
* Responsive design for iPhone, iPad, and desktop
* High contrast for accessibility (WCAG AA)
* Intuitive navigation and clear call-to-action buttons

## Testing & Quality Assurance
* Unit tests for critical functions (calculations, auth)
* Integration tests for API endpoints
* Test data seeds for development with realistic scenarios
* Error scenarios well-handled with user-friendly messages
* Performance testing for offline sync and large datasets
* Security penetration testing for authentication
* Load testing for concurrent users

## Setup & Deployment
* Prerequisites: Node.js 16+, PostgreSQL 12+
* Step-by-step setup guide with environment configuration
* Docker Compose setup for local development
* Database initialization and migration scripts
* Production deployment instructions
* CI/CD pipeline configuration

---

**Start with the core features (sessions logging, dashboard, month-end closing). Prioritize security, data integrity, and user experience.**

This prompt is production-ready for fitness training businesses needing secure, scalable session and billing management.

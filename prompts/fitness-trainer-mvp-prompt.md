# Prompt: Fitness Trainer MVP App

**Use case:** Building a complete, production-ready mobile/web application for fitness trainers to log training sessions, track trainees, and automate month-end billing and payroll. Designed for trainers managing multiple clients across different training types with group manager oversight.

---

## Prompt

```
You are a full-stack application architect. Build a complete, production-ready MVP application with the following specifications:

## Core Features

1. **Authentication & Access**
   - Facial recognition login for trainers using iPhone camera (FaceID/face detection API)
   - Secure session management with automatic timeouts
   - Multi-user support with role-based access (Trainer, Group Manager, Admin)
   - Passwordless biometric authentication

2. **Trainer Dashboard**
   - Quick-access interface to log training sessions (3-4 steps max)
   - Form inputs for: trainee name, training track (dropdown), duration/amount per workout
   - Real-time session logging with timestamps
   - Edit/delete previous session logs within current month
   - At-a-glance daily summary and session count

3. **Training Tracks System**
   - Predefined training tracks (Strength, Cardio, Flexibility, Sports-Specific)
   - Create custom tracks specific to trainer or group
   - Group tracks with designated group managers
   - Per-track pricing/rate configuration by managers
   - Track-specific notes and requirements

4. **Monthly Session Closing**
   - Month-end finalization workflow with warning system
   - "Summarize and Send" button generates: total training hours/sessions per trainee, amount owed by trainee (calculated from track rates × hours), debt notification message, invoice/summary document
   - Prevents modification of closed months
   - Archive previous months for reference

5. **Group Manager Features**
   - Dashboard showing all trainers in their group
   - Automatic notifications of trainer deliverables
   - Set and adjust salary rates per trainee
   - View total salary owed to trainers based on delivered training
   - Generate trainer payment reports
   - Performance metrics and group analytics

6. **Analytics & Statistics**
   - Monthly statistics per track (total hours, sessions, trainees)
   - Trainer performance metrics and comparisons
   - Group performance insights
   - Exportable reports (PDF/CSV)
   - Charts/visualizations of training trends over time

7. **Notifications System**
   - Push notifications for month-end reminders
   - Debt notifications to trainees
   - Salary notifications to group managers
   - Email summaries of monthly activity
   - Configurable notification preferences

## Technical Requirements

**Technology Stack:**
- Frontend: React Native (iOS-first) or React web with mobile responsiveness
- Backend: Node.js/Express or Python
- Database: PostgreSQL with encryption
- Authentication: Biometric (FaceID) + JWT token-based auth
- Notifications: Firebase Cloud Messaging or Twilio
- File Generation: PDF libraries for invoices and reports

**Security Requirements:**
- Encrypt sensitive data at rest (trainer IDs, trainee info, financial data)
- Implement role-based access control (RBAC) at API level
- Secure API endpoints with authentication middleware
- Session timeouts for biometric login (15-30 min idle)
- Audit logging for all transactions and access
- GDPR-compliant data handling and retention policies
- Secure password reset mechanisms for secondary auth
- Financial transaction logging and reconciliation

**Project Structure:**
- src/ (components, screens, services, utils, config, hooks)
- server/ (routes, controllers, models, middleware, utils, config)
- database/ (migrations, seeds)
- tests/
- docs/

## Deliverables

1. Complete Application Code (frontend, backend, database schema, migrations, authentication system)
2. Configuration Files (.env.example, database.config.js, app.config.js, auth.config.js, notifications.config.js)
3. Database Schema (Users, Trainees, TrainingTracks, TrainingSessions, Groups, Invoices/Bills, Notifications, AuditLogs)
4. Documentation (API documentation, database schema diagram, setup and deployment instructions, user guide, security best practices)

## UI/UX Specifications
- Clean, modern, trainer-friendly interface
- Minimal clicks to log a session (target: 3-4 steps max)
- Large, readable text and buttons for quick use
- Dark mode support for low-light gym environments
- Offline capability for session logging (sync when online)
- Responsive design for iPhone, iPad, and desktop
- High contrast for accessibility (WCAG AA)
- Intuitive navigation and clear call-to-action buttons

## Testing & Quality Assurance
- Unit tests for critical functions (calculations, auth)
- Integration tests for API endpoints
- Test data seeds for development with realistic scenarios
- Error scenarios well-handled with user-friendly messages
- Performance testing for offline sync and large datasets
- Security penetration testing for authentication
- Load testing for concurrent users

## Setup & Deployment
- Prerequisites: Node.js 16+, PostgreSQL 12+
- Step-by-step setup guide with environment configuration
- Docker Compose setup for local development
- Database initialization and migration scripts
- Production deployment instructions
- CI/CD pipeline configuration

---

Start with core features (sessions logging, dashboard, month-end closing). Prioritize security, data integrity, and user experience.
```

---

**When to use this prompt:**
- Building a complete fitness trainer management application from scratch
- Creating a session logging system with billing automation
- Designing a multi-user platform with role-based access (trainer, manager, admin)
- Developing a mobile-first application for gym or fitness business management
- Implementing biometric authentication for trainer login systems

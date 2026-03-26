# Fitness Trainer MVP App - Claude Code Prompt

Create a complete, production-ready MVP application with the following specifications:

## Core Features

### 1. Authentication & Access
* Facial recognition login for trainers using iPhone camera (use FaceID/face detection API)
* Secure session management
* Multi-user support with role-based access (Trainer, Group Manager, Admin)

### 2. Trainer Dashboard
* Quick-access interface to log training sessions
* Form inputs for: trainee name, training track (select from list), duration/amount per workout
* Real-time session logging with timestamps
* Edit/delete previous session logs within current month

### 3. Training Tracks System
* Predefined training tracks (e.g., Strength, Cardio, Flexibility, Sports-Specific)
* Ability to create custom tracks
* Group tracks with designated group managers
* Per-track pricing/rate configuration by managers

### 4. Monthly Session Closing
* Month-end finalization workflow
* "Summarize and Send" button that generates:
  * Total training hours/sessions per trainee
  * Amount owed by trainee (calculated from track rates × hours)
  * Debt notification message
  * Invoice/summary document
* Prevents modification of closed months

### 5. Group Manager Features
* Dashboard showing all trainers in their group
* Automatic notifications of trainer deliverables
* Set salary rates per trainee
* View total salary owed to trainers based on delivered training
* Generate trainer payment reports

### 6. Analytics & Statistics
* Monthly statistics per track (total hours, sessions, trainees)
* Trainer performance metrics
* Group performance insights
* Exportable reports (PDF/CSV)
* Charts/visualizations of training trends

### 7. Notifications System
* Push notifications for month-end reminders
* Debt notifications to trainees
* Salary notifications to group managers
* Email summaries of monthly activity

## Technical Requirements

### Technology Stack:
* Frontend: React Native (for iOS-first mobile experience) or React web with mobile responsiveness
* Backend: Node.js/Express or Python (your choice)
* Database: PostgreSQL (with secure encryption for sensitive data)
* Authentication: Biometric (FaceID) + token-based auth (JWT)
* Notifications: Firebase Cloud Messaging or similar

### Security Requirements:
* Encrypt sensitive data (trainer IDs, trainee information, financial data)
* Implement role-based access control (RBAC)
* Secure API endpoints with authentication middleware
* Session timeouts for biometric login
* Audit logging for all transactions
* GDPR-compliant data handling
* Secure password reset mechanisms

### Project Structure:

```
├── src/
│   ├── components/
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
├── config/
│   ├── env.example
│   ├── database.config.js
│   └── app.config.js
├── tests/
├── docs/
└── README.md
```

## Deliverables

### 1. Complete Application Code
* Full frontend implementation (mobile-ready)
* Backend API with all endpoints
* Database schema and migrations
* Authentication system with biometric support

### 2. Configuration Files
* `.env.example` with all required variables
* `database.config.js` with connection settings
* `app.config.js` with app-wide settings (rates, notification settings, etc.)
* `auth.config.js` for security settings

### 3. Skill Files (if using Claude Code modules)
* Create reusable skill files for:
  * Biometric authentication
  * Financial calculations (invoicing, salary)
  * Report generation
  * Notification dispatch

### 4. Documentation
* API documentation (list all endpoints)
* Database schema diagram
* Setup and deployment instructions
* User guide for trainers and managers
* Security best practices document

### 5. Database Schema (include migrations)
* Users table (with roles: trainer, manager, admin)
* Trainees table
* TrainingTracks table
* TrainingSessions table (with timestamps, amounts)
* Groups table (for group management)
* Invoices/Bills table
* Notifications table
* AuditLogs table

### 6. Additional Features
* Dashboard with at-a-glance metrics
* Search and filter functionality
* Bulk actions (e.g., send multiple invoices)
* Settings page for admins/managers
* Help/FAQ section
* Error handling and user-friendly error messages

## UI/UX Specifications
* Clean, modern, trainer-friendly interface
* Minimal clicks to log a session (aim for 3-4 steps max)
* Large, readable text and buttons (for quick use)
* Dark mode support
* Offline capability for session logging (sync when online)
* Responsive design that works on iPhone, iPad, and desktop

## Testing & Quality Assurance
* Unit tests for critical functions (calculations, auth)
* Integration tests for API endpoints
* Test data seeds for development
* Error scenarios well-handled

## Installation & Setup Instructions
* Provide step-by-step setup guide
* Include Docker configuration (if applicable)
* Provide database initialization scripts

---

**Start with the core features and ensure each is fully functional before adding enhancements. Prioritize security, data integrity, and user experience.**

This prompt is ready to paste directly into Claude Code and will guide it to create a comprehensive, production-grade application with all the structure and security considerations needed for a real-world fitness trainer management system.

# CV Optimization Platform - Claude Code Prompt

*AI-powered resume analysis and optimization tool that helps job seekers improve their CVs for ATS compatibility and recruiter appeal*

## Overview

A comprehensive CV optimization platform that uses AI to analyze resumes, identify weaknesses, and provide actionable recommendations. The tool helps job seekers tailor their CVs for specific job descriptions, improve ATS compatibility, and enhance their professional impact. Features real-time feedback, keyword optimization, and competitive analysis against job requirements.

## Core Features

### 1. CV Upload & Parsing
* Support multiple file formats (PDF, DOCX, TXT)
* Intelligent text extraction and structure recognition
* Section identification (education, experience, skills, etc.)
* Data validation and quality checks
* Version control with upload history

### 2. ATS Compatibility Analysis
* Scan for ATS-blocking formatting issues
* Flag non-compliant layouts, tables, images, fonts
* Generate ATS compatibility score
* Provide specific fix recommendations
* Show before/after comparison

### 3. Keyword Optimization
* Extract keywords from job descriptions
* Identify missing high-impact keywords in CV
* Suggest keyword placement and frequency
* Avoid keyword stuffing warnings
* Track keyword relevance scores

### 4. Content Enhancement
* Identify weak action verbs and suggest stronger alternatives
* Flag missing metrics and quantifiable achievements
* Suggest content restructuring for better impact
* Grammar and spelling checks
* Tone analysis and professional language review

### 5. Job Match Analysis
* Compare CV against specific job descriptions
* Calculate match percentage for each position
* Highlight gaps between CV and requirements
* Recommend section improvements per job
* Create tailored CV suggestions

### 6. Competitive Benchmarking
* Compare CV against industry standards
* Identify common keywords in similar roles
* Show skill gaps vs. typical candidates
* Provide market context and salary insights
* Highlight unique strengths

### 7. Personalized Recommendations
* Generate priority-ranked improvement suggestions
* Provide rewrite examples for weak sections
* Suggest certifications and skills to add
* Career path recommendations
* Timeline for improvements

## Technical Requirements

### Technology Stack
* Frontend: React with TypeScript
* Backend: Node.js/Express
* Database: PostgreSQL
* File Processing: pdf-parse, docx-parser, sharp
* AI Integration: OpenAI API for content analysis
* Authentication: Auth0 or similar
* Cloud Storage: AWS S3 for file management

### Security Requirements
* Encrypt all uploaded CV data at rest and in transit
* Implement GDPR compliance for data retention
* Secure file deletion after analysis period
* Role-based access control
* Audit logging for all user actions
* API rate limiting and DDoS protection
* PCI compliance for potential payment processing
* Regular security audits

### Project Structure
```
├── src/
│   ├── components/
│   │   ├── CVUpload/
│   │   ├── AnalysisResults/
│   │   ├── JobMatcher/
│   │   └── Recommendations/
│   ├── pages/
│   ├── services/
│   │   ├── cvParser/
│   │   ├── atsAnalyzer/
│   │   ├── keywordOptimizer/
│   │   └── aiIntegration/
│   ├── hooks/
│   └── utils/
├── server/
│   ├── routes/
│   ├── controllers/
│   ├── models/
│   ├── middleware/
│   ├── services/
│   └── config/
├── database/
│   ├── migrations/
│   └── seeds/
├── tests/
├── docs/
└── README.md
```

## Deliverables

### 1. Application Code
* Complete React frontend with file upload and analysis views
* Express backend with file processing and analysis pipelines
* CV parsing and text extraction service
* ATS compatibility analyzer
* Keyword extraction and optimization engine
* Job match comparison service
* Recommendation generation system

### 2. Configuration
* `.env.example` with API keys, storage settings
* `database.config.js` with connection settings
* `app.config.js` with feature flags, analysis settings
* `auth.config.js` with OAuth configuration
* `storage.config.js` with S3 and file handling settings

### 3. Documentation
* API documentation with all endpoints
* File format specifications and parsing logic
* ATS rules and compatibility guide
* Setup and deployment guide
* User guide with best practices
* Architecture overview and data flow

### 4. Database Schema
* Users - Account information and preferences
* CVs - Uploaded files and metadata
* Analyses - Analysis results and recommendations
* JobPostings - Saved job descriptions for matching
* Keywords - Extracted and tracked keywords
* AuditLogs - Track all user actions

## UI/UX Specifications
* Clean, professional dashboard with drag-and-drop file upload
* Real-time analysis feedback with progress indicators
* Color-coded scoring system (red/yellow/green)
* Side-by-side comparison views
* Interactive recommendation cards with "apply" actions
* Mobile-responsive design for on-the-go access
* Dark mode support
* Accessibility compliant (WCAG 2.1 AA)

## Testing & Quality Assurance
* Unit tests for CV parsing logic
* Integration tests for ATS analysis
* Test CVs with various formats and edge cases
* Performance testing with large files
* Security testing for file uploads
* Accessibility testing across browsers
* E2E tests for user workflows

## Setup & Deployment
* Requires Node.js 16+, PostgreSQL 12+
* Install dependencies: `npm install`
* Configure environment variables
* Run database migrations
* Start backend and frontend servers
* Docker Compose setup for local development
* Deploy to AWS/Heroku with CI/CD pipeline

---

**Start with CV parsing and ATS analysis. Prioritize accuracy of file processing and clarity of recommendations.**

This prompt is production-ready for job seekers, career coaches, and recruitment platforms.

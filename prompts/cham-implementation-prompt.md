# Prompt: CHAM Model Implementation for ST-System

**Use case:** Implementing a sophisticated three-layer code assessment system that combines automated unit testing, LLM-based quality analysis, and intelligent human review routing. Designed for educational platforms that need accurate, fair, and scalable code evaluation with minimal false positives/negatives.

---

## Prompt

```
# Implementing CHAM Model in ST-System

## System Context
You are working on ST-System - an AI-powered student assessment system built with Next.js 14, MongoDB Atlas, and Google OAuth. The system currently runs assessments using Gemini API and is hosted on Vercel.

## Mission
Implement the "Contextual Hybrid Assessment Model" (CHAM) - a three-layer assessment architecture with smart routing mechanism.

---

## Model Architecture

### Layer 1: Objective-Dynamic Assessment Layer
**Role:** Execute code against unit tests - ground truth for functional correctness

**Implementation Requirements:**
1. **Isolated Execution Environment (Sandbox):**
   - Create Docker container or isolated VM for each code execution
   - Set timeout (e.g., 30 seconds) and resource limits
   - Support for languages: Python, JavaScript, Java, C, C++
   
2. **Unit Test System:**
   - JSON format for each question: `{ "question_id": "...", "tests": [...] }`
   - Each test includes: input, expected_output, test_type (equality/range/exception)
   - Create test database in MongoDB (`unit_tests` collection)
   
3. **Execution Mechanism:**
   - Receive student code + test list
   - Run each test and record: passed/failed, execution_time, error_message
   - Return: `{ "total_tests": N, "passed": M, "failed_tests": [...] }`

**New API Endpoint:**
```
POST /api/assessment/execute-tests
Body: {
  "submission_id": "...",
  "code": "...",
  "language": "python",
  "question_id": "..."
}
Response: {
  "layer1_score": 0-100,
  "test_results": {...},
  "execution_errors": [...]
}
```

---

### Layer 2: Semantic-Static Assessment Layer
**Role:** LLM analysis of code quality beyond functionality

**Assessment Criteria:**
1. **Code Quality (25%):**
   - Readability and consistent style
   - Meaningful variable/function names
   - Proper organizational structure

2. **Documentation (20%):**
   - Appropriate comments
   - Function docstrings
   - README if required

3. **Algorithmic Complexity (25%):**
   - Big-O analysis
   - Solution optimality
   - Proper use of data structures

4. **Error Handling (15%):**
   - try-catch blocks
   - Edge case handling
   - Input validation

5. **Best Practices (15%):**
   - SOLID/DRY principles
   - Security considerations
   - Code smells

**Technical Implementation:**
```javascript
// services/semanticAssessment.js
async function analyzeCodeQuality(code, language, question_context) {
  const prompt = `
    Analyze the following ${language} code submission:
    
    CODE:
    ${code}
    
    QUESTION CONTEXT:
    ${question_context}
    
    Provide a JSON response with:
    {
      "code_quality": { "score": 0-100, "feedback": "..." },
      "documentation": { "score": 0-100, "feedback": "..." },
      "complexity": { "score": 0-100, "big_o": "...", "feedback": "..." },
      "error_handling": { "score": 0-100, "feedback": "..." },
      "best_practices": { "score": 0-100, "feedback": "..." },
      "overall_score": 0-100,
      "confidence": 0-100,
      "flags_for_human_review": []
    }
  `;
  
  const response = await callLLM(prompt); // Gemini/Claude API
  return response;
}
```

**New MongoDB Model:**
```javascript
const SemanticAssessmentSchema = new Schema({
  submission_id: ObjectId,
  timestamp: Date,
  criteria_scores: {
    code_quality: Number,
    documentation: Number,
    complexity: Number,
    error_handling: Number,
    best_practices: Number
  },
  overall_score: Number,
  confidence_score: Number,
  detailed_feedback: String,
  llm_model_used: String
});
```

---

### Layer 3: Contextual-Human Assessment Layer
**Role:** Selective human review for aspects LLM cannot assess

**Teacher Interface:**
1. **Review Queue:**
   - Dedicated page: `/teacher/review-queue`
   - List of submissions awaiting human review
   - Info per submission: routing reason, layer 1+2 scores, wait time

2. **Review Interface:**
   - Code display with syntax highlighting
   - Test results and automated feedback
   - Form for adding comments and adjusting score
   - Option for student dialogue (chat/comments)

3. **Performance Metrics:**
   - Average review time
   - Percentage of submissions routed
   - Correlation between automated and human scores

**API Endpoints:**
```
GET /api/teacher/review-queue
POST /api/teacher/submit-review
  Body: {
    "submission_id": "...",
    "human_score": 0-100,
    "comments": "...",
    "override_auto_score": boolean
  }
```

---

## Smart Routing Mechanism

### Trigger 1: Confidence Score
```javascript
function checkConfidenceTrigger(semantic_result) {
  const CONFIDENCE_THRESHOLD = 70;
  return semantic_result.confidence < CONFIDENCE_THRESHOLD;
}
```

### Trigger 2: Border Zone
```javascript
function checkBorderZoneTrigger(final_score) {
  const PASS_THRESHOLD = 56;
  const BORDER_RANGE = 10; // ±10 points
  
  return Math.abs(final_score - PASS_THRESHOLD) <= BORDER_RANGE;
}
```

### Trigger 3: Question Type
```javascript
// Add field to Question schema:
const QuestionSchema = new Schema({
  // ... existing fields
  question_type: {
    type: String,
    enum: ['objective', 'creative', 'open-ended', 'algorithmic'],
    required: true
  },
  requires_human_review: {
    type: Boolean,
    default: false
  }
});

function checkQuestionTypeTrigger(question) {
  return ['creative', 'open-ended'].includes(question.question_type) 
    || question.requires_human_review;
}
```

### Trigger 4: Student History
```javascript
async function checkStudentHistoryTrigger(student_id, current_score) {
  const previousSubmissions = await Submission.find({
    student_id,
    status: 'graded'
  }).sort({ submitted_at: -1 }).limit(5);
  
  // First submission
  if (previousSubmissions.length === 0) {
    return { triggered: true, reason: 'first_submission' };
  }
  
  // Calculate mean and std
  const scores = previousSubmissions.map(s => s.final_score);
  const avg = scores.reduce((a, b) => a + b, 0) / scores.length;
  const std = Math.sqrt(
    scores.reduce((sq, n) => sq + Math.pow(n - avg, 2), 0) / scores.length
  );
  
  // Anomalous deviation: more than 2 standard deviations
  const deviation = Math.abs(current_score - avg);
  if (deviation > 2 * std) {
    return { triggered: true, reason: 'anomalous_deviation', deviation, avg, std };
  }
  
  return { triggered: false };
}
```

### Combined Routing Logic
```javascript
async function evaluateRoutingDecision(submission, question, semantic_result, layer1_score) {
  const triggers = [];
  
  // Trigger 1
  if (checkConfidenceTrigger(semantic_result)) {
    triggers.push({ type: 'low_confidence', value: semantic_result.confidence });
  }
  
  // Trigger 2
  const combined_score = (layer1_score * 0.6) + (semantic_result.overall_score * 0.4);
  if (checkBorderZoneTrigger(combined_score)) {
    triggers.push({ type: 'border_zone', score: combined_score });
  }
  
  // Trigger 3
  if (checkQuestionTypeTrigger(question)) {
    triggers.push({ type: 'question_type', question_type: question.question_type });
  }
  
  // Trigger 4
  const historyCheck = await checkStudentHistoryTrigger(submission.student_id, combined_score);
  if (historyCheck.triggered) {
    triggers.push({ type: 'student_history', ...historyCheck });
  }
  
  return {
    requires_human_review: triggers.length > 0,
    triggers: triggers,
    auto_score: combined_score,
    timestamp: new Date()
  };
}
```

---

## Workflow

1. **Student submits code:**
   - POST `/api/submissions/submit`
   - Save to DB with status: 'pending'

2. **Layer 1 - Run tests:**
   - Pass to sandbox execution service
   - Save results + calculate layer1_score

3. **Layer 2 - Semantic analysis:**
   - Call LLM API (Gemini/Claude)
   - Extract scores + confidence + feedback

4. **Activate routing mechanism:**
   - Run 4 triggers
   - Decide: auto-grade or human-review

5. **If auto-grade:**
   - Calculate weighted score: `layer1 * 0.6 + layer2 * 0.4`
   - Update status: 'graded'
   - Send feedback to student

6. **If human-review:**
   - Update status: 'awaiting_review'
   - Add to table: `human_review_queue`
   - Notify teacher

7. **Teacher performs review:**
   - Review interface with all info
   - Enter human score + comments
   - Update status: 'graded'

---

## Required Database Structure

### New Table: `assessment_layers`
```javascript
{
  submission_id: ObjectId,
  layer1: {
    score: Number,
    test_results: Object,
    execution_time: Number,
    errors: Array
  },
  layer2: {
    score: Number,
    criteria_breakdown: Object,
    confidence: Number,
    feedback: String,
    model_used: String
  },
  layer3: {
    required: Boolean,
    triggers: Array,
    human_score: Number,
    reviewer_id: ObjectId,
    reviewed_at: Date,
    comments: String
  },
  final_score: Number,
  score_calculation: {
    formula: String,
    weights: Object
  },
  created_at: Date
}
```

### Update Table: `submissions`
```javascript
// Add fields:
{
  // ... existing fields
  assessment_status: {
    type: String,
    enum: ['pending', 'testing', 'semantic_analysis', 'awaiting_review', 'graded'],
    default: 'pending'
  },
  routing_decision: {
    requires_human: Boolean,
    triggers: Array,
    decided_at: Date
  }
}
```

### New Table: `human_review_queue`
```javascript
{
  submission_id: ObjectId,
  student_id: ObjectId,
  question_id: ObjectId,
  added_at: Date,
  priority: Number, // calculated from triggers
  auto_score: Number,
  triggers: Array,
  reviewed: Boolean,
  reviewer_id: ObjectId,
  reviewed_at: Date
}
```

---

## Implementation Steps

### Phase 1: Sandbox Infrastructure (1-2 work days)
- [ ] Install Docker SDK in backend
- [ ] Create Dockerfile for Python/JS execution environment
- [ ] Build API endpoint for code execution: `/api/sandbox/execute`
- [ ] Add timeout and resource limits
- [ ] Test with sample code

### Phase 2: Unit Test System (1 work day)
- [ ] Create MongoDB collection: `unit_tests`
- [ ] Build teacher interface for adding tests to questions
- [ ] Write matching function between submission and tests
- [ ] Add UI for displaying test results

### Phase 3: Semantic Analysis Layer (2 work days)
- [ ] Write service: `semanticAssessment.js`
- [ ] Build appropriate prompt engineering for Gemini
- [ ] Create schema: `semantic_assessments`
- [ ] Add JSON response parsing from LLM
- [ ] Add fallback for invalid responses

### Phase 4: Smart Routing Mechanism (1-2 work days)
- [ ] Implement 4 triggers as separate functions
- [ ] Build combined decision logic
- [ ] Create collection: `human_review_queue`
- [ ] Add required fields to Question and Submission schemas

### Phase 5: Teacher Review Interface (2-3 work days)
- [ ] Page: `/teacher/review-queue`
- [ ] ReviewCard component with all info
- [ ] Comments and score form
- [ ] API: POST `/api/teacher/submit-review`
- [ ] Teacher notifications for new submissions

### Phase 6: Full Integration (1 work day)
- [ ] Connect all layers in one flow
- [ ] Add detailed logging
- [ ] Build system monitoring dashboard
- [ ] End-to-end testing

### Phase 7: Optimization and Monitoring (ongoing)
- [ ] Measure response times
- [ ] Calculate percentage of human review referrals
- [ ] Collect accuracy metrics (correlation between scores)
- [ ] Tune weights and thresholds

---

## Important Technical Considerations

### 1. Performance
- Layer 1 (sandbox) can take 5-30 seconds - run async
- Layer 2 (LLM) depends on API rate limits - consider queuing
- Cache results for re-grading cases

### 2. Security
- Sandbox must be **completely isolated** from the system
- No student code access to network/filesystem
- Filter input to prevent code injection in LLM prompts

### 3. Costs
- LLM calls cost money - consider:
  - Caching identical assessments
  - Daily call ceiling
  - Using cheaper model for Layer 2

### 4. Flexibility
- Each layer should be pluggable
- Allow LLM provider switching (Gemini ↔ Claude ↔ GPT)
- Layer weights (`layer1 * 0.6 + layer2 * 0.4`) should be configurable

---

## Starter Code Example

```javascript
// services/chamAssessment.js
const { executeSandbox } = require('./sandbox');
const { analyzeCodeQuality } = require('./semanticAssessment');
const { evaluateRoutingDecision } = require('./smartRouting');

async function assessSubmission(submission_id) {
  const submission = await Submission.findById(submission_id)
    .populate('question_id student_id');
  
  // Layer 1
  console.log(`[CHAM] Starting Layer 1 for ${submission_id}`);
  const layer1Result = await executeSandbox({
    code: submission.code,
    language: submission.language,
    tests: submission.question_id.unit_tests
  });
  
  // Layer 2
  console.log(`[CHAM] Starting Layer 2 for ${submission_id}`);
  const layer2Result = await analyzeCodeQuality(
    submission.code,
    submission.language,
    submission.question_id.description
  );
  
  // Save results
  const assessment = await AssessmentLayers.create({
    submission_id,
    layer1: layer1Result,
    layer2: layer2Result,
    created_at: new Date()
  });
  
  // Routing mechanism
  console.log(`[CHAM] Evaluating routing for ${submission_id}`);
  const routing = await evaluateRoutingDecision(
    submission,
    submission.question_id,
    layer2Result,
    layer1Result.score
  );
  
  if (routing.requires_human_review) {
    // Add to review queue
    await HumanReviewQueue.create({
      submission_id,
      student_id: submission.student_id,
      question_id: submission.question_id,
      added_at: new Date(),
      triggers: routing.triggers,
      auto_score: routing.auto_score,
      reviewed: false
    });
    
    await submission.update({
      assessment_status: 'awaiting_review',
      routing_decision: routing
    });
    
    console.log(`[CHAM] ${submission_id} routed to human review`);
    return { status: 'awaiting_review', routing };
    
  } else {
    // Calculate automatic score
    const final_score = (layer1Result.score * 0.6) + (layer2Result.overall_score * 0.4);
    
    await assessment.update({
      final_score,
      score_calculation: {
        formula: 'layer1 * 0.6 + layer2 * 0.4',
        weights: { layer1: 0.6, layer2: 0.4 }
      }
    });
    
    await submission.update({
      assessment_status: 'graded',
      final_score,
      graded_at: new Date()
    });
    
    console.log(`[CHAM] ${submission_id} auto-graded: ${final_score}`);
    return { status: 'graded', final_score, assessment };
  }
}

module.exports = { assessSubmission };
```

---

## Execution Instructions for Claude Code

1. **Start with existing system analysis:**
   - Scan the ST-System codebase
   - Identify project structure, schemas, API routes
   - Find where current assessment is performed
   - Make changes incrementally - if the current session stops, changes persist and tokens aren't wasted

2. **Plan the integration:**
   - Create flow diagram of new model
   - Identify files to modify and new files to create
   - Prepare database migration plan

3. **Execute in phases:**
   - Don't try to do everything at once
   - Start with Phase 1 (sandbox), verify it works
   - Continue to Phase 2, etc.
   - After each phase - run tests

4. **Maintain documentation:**
   - Update README with detailed CHAM explanation
   - Document each new API endpoint
   - Add code comments explaining the logic

5. **Testing:**
   - Create test cases for each layer
   - Test edge cases (failing tests, low confidence, etc.)
   - Ensure system handles errors gracefully

---

## Summary
CHAM model is an advanced architecture that combines:
- ✅ Functional validation (unit tests)
- ✅ Quality analysis (LLM)
- ✅ Intelligent human judgment (smart routing)

**Goal:** Accurate, fair, and efficient assessment that combines the strengths of each method and reduces false positives/negatives.

**Expected Result:** An assessment system that students trust more, teachers work less on, and grades better reflect actual performance.
```

---

**When to use this prompt:**
- Building or upgrading an automated code assessment system for educational platforms
- Implementing multi-layer evaluation that goes beyond simple unit testing
- Creating a system that needs to balance automation with human oversight
- Developing assessment tools for programming courses with diverse question types
- Migrating from purely manual or purely automated grading to a hybrid approach
- Reducing teacher workload while maintaining high assessment quality
- Minimizing false positives (good code scored low) and false negatives (bad code scored high)

# Calculator Workflow & Data Flow

## High-Level User Workflow

```
┌─────────────────────────────────────────────────────────┐
│ User Visits Calculator URL                              │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│ Landing Page Loads                                      │
│ - Company info form                                     │
│ - Assessment type selection                             │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
         ┌──────────────┐
         │ Form Valid?  │
         └──────┬───┬──┘
            No  │   │ Yes
         ┌──────┘   └──────┐
         ▼                  ▼
    [Show Error]    ┌─────────────────────────┐
         │          │ Save Assessment Session│
         │          │ Generate Session ID    │
         │          └────────────┬───────────┘
         │                       │
         └───────────┬───────────┘
                     ▼
        ┌──────────────────────────┐
        │ Assessment Wizard Start  │
        │ Load Dimension 1         │
        └────────────┬─────────────┘
                     │
                     ▼
        ┌──────────────────────────┐
        │ Display Question          │
        │ with Options             │
        └────────────┬─────────────┘
                     │
                     ▼
        ┌──────────────────────────┐
        │ User Selects Answer       │
        │ Save to Session           │
        └────────────┬─────────────┘
                     │
                     ▼
          ┌─────────────────┐
          │ More Questions? │
          └────────┬────┬───┘
               Yes │    │ No
          ┌────────┘    └────────┐
          ▼                      ▼
     (Loop)           ┌──────────────────────┐
                      │ Calculate Scores     │
                      │ Analyze Results      │
                      │ Generate Report      │
                      └────────────┬─────────┘
                                   │
                                   ▼
                      ┌──────────────────────┐
                      │ Results Dashboard    │
                      │ - Overall score      │
                      │ - Dimension scores   │
                      │ - Risk assessment    │
                      │ - Gap analysis       │
                      └────────────┬─────────┘
                                   │
                                   ▼
                      ┌──────────────────────┐
                      │ Recommendations      │
                      │ - AMC services       │
                      │ - Roadmap            │
                      └────────────┬─────────┘
                                   │
                                   ▼
                      ┌──────────────────────┐
                      │ Lead Capture Form    │
                      │ - Email              │
                      │ - Phone              │
                      │ - Company contact    │
                      └────────────┬─────────┘
                                   │
                                   ▼
                      ┌──────────────────────┐
                      │ Report Download      │
                      │ Email Confirmation   │
                      │ Sales Notification   │
                      └──────────────────────┘
```

## Data Collection Flow

### Phase 1: Initial Information
```json
{
  "sessionId": "uuid",
  "createdAt": "timestamp",
  "companyInfo": {
    "name": "string",
    "industry": "string",
    "companySize": "enum",
    "annualRevenue": "string",
    "primaryContact": "string",
    "email": "string",
    "phone": "string"
  },
  "assessmentConfig": {
    "type": "quick|comprehensive|custom",
    "startedAt": "timestamp",
    "estimatedDuration": "minutes"
  }
}
```

### Phase 2: Assessment Responses
```json
{
  "sessionId": "uuid",
  "responses": [
    {
      "questionId": "G1",
      "dimension": "Governance",
      "answer": "4",  // Scale 0-4
      "answeredAt": "timestamp",
      "timeSpent": "seconds"
    },
    // ... more responses
  ]
}
```

### Phase 3: Calculated Scores
```json
{
  "sessionId": "uuid",
  "scores": {
    "overallScore": 72,
    "dimensions": {
      "governance": {
        "score": 78,
        "weight": 1.0,
        "weightedScore": 78
      },
      "riskManagement": {
        "score": 65,
        "weight": 1.0,
        "weightedScore": 65
      },
      // ... more dimensions
    },
    "riskAssessment": {
      "overallRisk": 45,
      "regulatoryRisk": 52,
      "operationalRisk": 38,
      "securityRisk": 42,
      "reputationalRisk": 35
    }
  }
}
```

### Phase 4: Gap Analysis
```json
{
  "sessionId": "uuid",
  "gaps": [
    {
      "id": "G1-gap",
      "dimension": "Governance",
      "currentState": 2,
      "targetState": 4,
      "gap": 2,
      "priority": "HIGH",
      "effort": "MEDIUM",
      "estimatedCost": "45000-65000",
      "timelineMonths": 3,
      "description": "Documentation of governance charter"
    },
    // ... more gaps
  ]
}
```

### Phase 5: Recommendations
```json
{
  "sessionId": "uuid",
  "recommendations": [
    {
      "id": "rec-001",
      "serviceName": "Governance Audit & Enhancement",
      "relevance": 95,
      "priority": "HIGH",
      "estimatedCost": "45000-65000",
      "timelineMonths": 3,
      "description": "...",
      "relatedGaps": ["G1-gap", "G3-gap"]
    },
    // ... more recommendations
  ]
}
```

## Backend API Endpoints

### 1. Session Management
```
POST /api/v1/assessment/start
- Create new assessment session
- Input: companyInfo
- Output: sessionId, assessmentConfig

GET /api/v1/assessment/{sessionId}
- Retrieve assessment progress
- Output: Current session state, completed questions

POST /api/v1/assessment/{sessionId}/save
- Save current progress
- Input: responses array
- Output: Confirmation, next question
```

### 2. Question Retrieval
```
GET /api/v1/questions?dimension={dimension}&index={index}
- Get next question
- Output: Question object with options

GET /api/v1/questions/all
- Get all questions (for offline mode)
- Output: Complete question bank
```

### 3. Scoring & Analysis
```
POST /api/v1/assessment/{sessionId}/calculate
- Calculate scores and analysis
- Input: All responses
- Output: Scores, gaps, recommendations

GET /api/v1/assessment/{sessionId}/results
- Retrieve assessment results
- Output: All calculated scores and analysis
```

### 4. Report Generation
```
GET /api/v1/assessment/{sessionId}/report/pdf
- Generate PDF report
- Output: PDF file

GET /api/v1/assessment/{sessionId}/report/excel
- Generate Excel workbook
- Output: Excel file

POST /api/v1/assessment/{sessionId}/report/email
- Email report to recipient
- Input: Email address
- Output: Confirmation
```

### 5. Lead Management
```
POST /api/v1/assessment/{sessionId}/lead
- Capture lead information
- Input: Contact details
- Output: Lead ID, confirmation

GET /api/v1/leads/{leadId}
- Retrieve lead information
- Output: Lead details

POST /api/v1/leads/{leadId}/notify
- Notify sales team
- Input: Notification preference
- Output: Confirmation
```

## Branching Logic

### Conditional Questions
Some questions appear only based on previous answers:

```javascript
// Example: If company size is "enterprise", show additional questions
if (response['companySize'] === 'enterprise') {
  showQuestion('G1-enterprise-specific');
}

// Example: If already have security program, skip basic questions
if (response['S1'] >= 3) {
  skipToQuestion('S5');
}
```

## Data Validation

- All responses must be valid against question type
- Required questions must be answered before proceeding
- Response time limits to detect session abandonment
- Invalid data rejected with clear error messages

## Session Management

- Sessions expire after 24 hours of inactivity
- Users can save and resume assessments
- Session data encrypted in transit and at rest
- Assessment history retained for 2 years

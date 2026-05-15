# Phase 02: UI/UX Design & Calculator Interface

## Overview
Phase 02 focuses on designing the user interface and experience for the BOOKAMC Compliance Calculator, ensuring ease of use while maintaining assessment accuracy.

## Design Goals

1. **User-Centric Design** - Intuitive navigation for enterprise users
2. **Progressive Disclosure** - Complex information presented incrementally
3. **Visual Clarity** - Clear progress indicators and assessment flow
4. **Accessibility** - WCAG 2.1 AA compliance
5. **Mobile Responsive** - Works seamlessly across devices

## Calculator Interface Architecture

### 1. Entry Page / Landing
**Purpose**: Capture initial user information and assess readiness

**Components**:
- Company information form (name, industry, size)
- Assessment type selection
- Estimated time indicator
- Start assessment button

**Data Collection**:
```json
{
  "companyName": "string",
  "industry": "string",
  "companySize": "enum: small|medium|large|enterprise",
  "annualRevenue": "string",
  "assessmentType": "enum: quick|comprehensive|custom"
}
```

### 2. Assessment Wizard
**Purpose**: Guide users through compliance assessment across 6 dimensions

**Dimensions**:
1. **Governance** (8-10 questions)
2. **Risk Management** (8-10 questions)
3. **Compliance** (10-12 questions)
4. **Security** (10-12 questions)
5. **Operations** (8-10 questions)
6. **Technology** (8-10 questions)

**Wizard Features**:
- Step-by-step progression
- Progress bar showing current section
- Save and resume functionality
- Skip optional questions
- Branching logic based on answers

**Question Structure**:
```json
{
  "id": "string",
  "dimension": "string",
  "category": "string",
  "question": "string",
  "description": "string",
  "type": "enum: radio|checkbox|scale|text|select",
  "options": [],
  "required": "boolean",
  "weight": "number",
  "conditional": {
    "parentId": "string",
    "parentValue": "string|number"
  }
}
```

### 3. Assessment Results Dashboard
**Purpose**: Display compliance scores and recommendations

**Key Sections**:

#### Overall Compliance Score
- Large, prominent score display (0-100)
- Maturity level indicator (Initial/Developing/Managed/Optimized/Advanced)
- Comparison to industry benchmark
- Trend indicator (improving/stable/declining)

#### Dimension Breakdown
- Individual scores for each dimension
- Radar chart showing dimension comparison
- Strengths and weaknesses highlighted

#### Risk Assessment
- Overall risk score (0-100)
- Risk level badge (Low/Moderate/High/Critical)
- Risk breakdown by category:
  - Regulatory risk
  - Operational risk
  - Security risk
  - Reputational risk

#### Gap Analysis
- Priority ranking of identified gaps
- Impact/effort matrix for each gap
- Estimated remediation timeline
- Resource requirements

### 4. Recommendations Engine
**Purpose**: Suggest targeted AMC services based on assessment results

**Display Format**:

```
┌─────────────────────────────────────┐
│ Recommended Services                │
├─────────────────────────────────────┤
│ 1. Governance Audit & Enhancement   │
│    Priority: HIGH                   │
│    Estimated Cost: $45K-65K         │
│    Timeline: 3-4 months             │
│    Relevance: 95%                   │
│    [Learn More] [Request Demo]      │
├─────────────────────────────────────┤
│ 2. Risk Management Framework        │
│    Priority: HIGH                   │
│    Estimated Cost: $35K-50K         │
│    Timeline: 2-3 months             │
│    Relevance: 92%                   │
│    [Learn More] [Request Demo]      │
└─────────────────────────────────────┘
```

### 5. Report Generation
**Purpose**: Provide downloadable assessment reports

**Report Contents**:
- Executive summary
- Detailed scoring breakdown
- Risk assessment analysis
- Gap analysis with priorities
- Compliance roadmap
- AMC service recommendations
- Industry benchmarking
- Timeline and budget estimates

**Export Formats**:
- PDF report
- Excel workbook with data
- JSON data export

### 6. Lead Capture & Follow-up
**Purpose**: Qualify leads and initiate sales process

**Components**:
- Company contact information form
- Preferred contact method
- Demo/consultation scheduling
- Document download options
- Sales team notification

**Follow-up Actions**:
- Automated email with report
- Sales team assignment
- CRM integration
- Calendar invite for demo

## User Flow Diagram

```
┌─────────────┐
│   Landing   │
│    Page     │
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│ Company Info    │
│ & Assessment    │
│  Type Selection │
└────────┬────────┘
         │
         ▼
    ┌─────────────┐
    │ Assessment  │
    │  Wizard     │
    │ (6 Dims)    │
    └──────┬──────┘
           │
           ▼
    ┌─────────────┐
    │  Results    │
    │ Dashboard   │
    └──────┬──────┘
           │
    ┌──────┴──────┐
    │             │
    ▼             ▼
┌────────┐  ┌─────────────┐
│ Reports│  │Recomm.      │
│        │  │Engine       │
└────┬───┘  └────┬────────┘
     │           │
     └─────┬─────┘
           ▼
     ┌──────────────┐
     │ Lead Capture │
     │   & Follow   │
     └──────────────┘
```

## Wireframes

### Entry Page Layout
```
┌──────────────────────────────────────┐
│  BOOKAMC Compliance Calculator       │
├──────────────────────────────────────┤
│                                      │
│  Evaluate Your Compliance Readiness  │
│                                      │
│  ┌────────────────────────────────┐  │
│  │ Company Name                   │  │
│  └────────────────────────────────┘  │
│                                      │
│  ┌────────────────────────────────┐  │
│  │ Industry (Dropdown)            │  │
│  └────────────────────────────────┘  │
│                                      │
│  ┌────────────────────────────────┐  │
│  │ Company Size (Radio)           │  │
│  │ ○ Small  ○ Medium ○ Large      │  │
│  └────────────────────────────────┘  │
│                                      │
│  ┌────────────────────────────────┐  │
│  │ Estimated Time: 15-20 minutes  │  │
│  └────────────────────────────────┘  │
│                                      │
│        [Start Assessment →]          │
│                                      │
└──────────────────────────────────────┘
```

### Assessment Wizard Layout
```
┌──────────────────────────────────────┐
│  BOOKAMC Compliance Calculator       │
├──────────────────────────────────────┤
│ Progress: [█████░░░░░░░░░░░] 35%    │
│ Current: Governance (3 of 6)        │
├──────────────────────────────────────┤
│                                      │
│ Question 3 of 8                      │
│                                      │
│ Does your organization have a       │
│ documented governance charter?      │
│                                      │
│ ○ Not applicable                     │
│ ○ Under development                  │
│ ● Exists but not fully documented    │
│ ○ Fully documented                   │
│ ○ Documented and regularly reviewed  │
│                                      │
│  [← Back]  [Next →]  [Save & Exit]  │
│                                      │
└──────────────────────────────────────┘
```

### Results Dashboard Layout
```
┌──────────────────────────────────────────────┐
│ BOOKAMC Compliance Assessment Results        │
├──────────────────────────────────────────────┤
│                                              │
│  Overall Score        Risk Level             │
│  ┌──────────┐        ┌──────────┐            │
│  │    72    │        │  MODERATE│            │
│  │ Managed  │        │  Risk    │            │
│  └──────────┘        └──────────┘            │
│  vs Industry: 68               Status: ▲    │
│                                              │
├──────────────────────────────────────────────┤
│                                              │
│  Dimension Scores                            │
│  ┌─────────────────────────────────┐         │
│  │ Governance:      78  [████████░]│         │
│  │ Risk Mgmt:       65  [██████░░░]│         │
│  │ Compliance:      82  [████████░]│         │
│  │ Security:        70  [███████░░]│         │
│  │ Operations:      68  [███████░░]│         │
│  │ Technology:      60  [██████░░░]│         │
│  └─────────────────────────────────┘         │
│                                              │
├──────────────────────────────────────────────┤
│ Risk Analysis  │  Gap Analysis  │  Reports   │
│ [Details]      │  [Details]     │ [Download] │
└──────────────────────────────────────────────┘
```

## Design System

### Color Palette
- **Primary**: #0066CC (Professional Blue)
- **Secondary**: #FF6B35 (Action Orange)
- **Success**: #22C55E (Green)
- **Warning**: #F59E0B (Amber)
- **Danger**: #EF4444 (Red)
- **Neutral**: #6B7280 (Gray)

### Typography
- **Headings**: Montserrat, Bold
- **Body**: Inter, Regular
- **Code**: Courier New, Regular

### Spacing
- Base unit: 8px
- Margins: 8px, 16px, 24px, 32px
- Padding: 8px, 12px, 16px, 20px

### Responsive Breakpoints
- Mobile: < 640px
- Tablet: 640px - 1024px
- Desktop: > 1024px

## Accessibility Requirements

- WCAG 2.1 Level AA compliance
- Keyboard navigation support
- Screen reader friendly
- Color contrast minimum 4.5:1
- Focus indicators clearly visible
- Form error messages linked to inputs

## Performance Targets

- Initial load: < 2 seconds
- Assessment step transitions: < 500ms
- Report generation: < 5 seconds
- Mobile performance: Lighthouse score > 85

## Next Steps

1. Create detailed wireframes in Figma/Adobe XD
2. Develop interactive prototypes
3. Conduct user testing with enterprise users
4. Refine design based on feedback
5. Create style guide and component library
6. Hand off to development team for Phase 03

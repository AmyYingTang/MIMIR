# Phase 1: Requirements Analysis

> **Phase Goal**: Clarify what to build, for whom, and to what extent  
> **Deliverables**: PRD document, priority definition, MVP scope  
> **Key Principle**: Asking the right questions matters more than giving answers

---

## 1. Agent Questioning Checklist

### 1.1 Business Understanding

```markdown
Let me first understand the business context:

**Core Questions**
1. What business problem does this system solve? (What's the pain point?)
2. How is this problem currently being solved? (What's the status quo?)
3. Why is a new system needed? (What's the motivation?)

**Users & Value**
4. Who are the target users? (Describe the user persona specifically)
5. What tasks can users accomplish through this system?
6. What are the success criteria? (Measurable metrics)

**Business Process**
7. What is the core business process? (Steps from start to completion)
8. Which roles are involved? What are their responsibilities?
9. Are there any steps requiring manual approval/intervention?
```

### 1.2 Scale & Performance

```markdown
Let me understand the scale expectations:

**User Scale**
1. How many users are expected to use this system?
   - [ ] < 50 people (small team)
   - [ ] 50-500 people (medium organization)
   - [ ] 500-5000 people (large organization)
   - [ ] > 5000 people (large scale)

2. How many concurrent online users expected?

3. User growth expectations? (Next 1-2 years)

**Data Scale**
4. How much data is expected to be generated?
   - How many new records per day?
   - Expected size of core data tables?

**Performance Requirements**
5. Any special requirements for response speed?
6. Any scenarios requiring real-time processing?
```

### 1.3 Users & Permissions

```markdown
About users and permissions:

**User Types**
1. How many types of users? Please list them
2. What are the main responsibilities of each user type?
3. Are there hierarchical/ownership relationships between users?

**Permission Requirements**
4. Do different users see the same data? (Data isolation)
5. Do different users perform the same operations? (Functional permissions)
6. Are permissions assigned by role or by individual?

**Authentication Requirements**
7. How do users log in? (Username/password / Phone / Email)
8. Need to support third-party login? (WeChat/DingTalk/Enterprise SSO)
9. Any password strength requirements?
```

### 1.4 Integration & Extension

```markdown
About integration with other systems:

**Existing Systems**
1. Which existing systems need to be integrated?
2. What's the integration method? (API / Database / File)
3. Does data need bidirectional or unidirectional sync?

**Future Extension**
4. Which systems might need to be integrated in the future? (Reserve interfaces in advance)
5. Might need to expose API to third parties in the future?
6. Considering a mobile App?

**Data Migration**
7. Any historical data that needs to be migrated?
8. What's the data format? How's the quality?
```

### 1.5 Technical Constraints

```markdown
Let me understand the technical environment and constraints:

**Existing Tech Stack**
1. What tech stack does the company/team mainly use?
2. Any technologies that must be used?
3. Any technologies that are forbidden?

**Infrastructure**
4. Where will the system be deployed? (Public cloud / Private cloud / On-premise servers)
5. Any existing database available?
6. Any CI/CD process in place?

**Compliance Requirements**
7. Any data storage location requirements? (Must be domestic, etc.)
8. Any security certification requirements? (SOC2, ISO, etc.)
9. Any audit requirements? (Operation logs, access records, etc.)

**Resource Constraints**
10. Project budget range?
11. Expected development timeline?
12. Development team configuration?
```

---

## 2. Information Organization & Confirmation

After collecting information, organize and confirm with the user in this format:

```markdown
## Project Overview Confirmation

### Basic Project Information
- **Project Name**: [Name]
- **Project Type**: Enterprise Web Application
- **Core Value**: [One sentence description]

### Users & Scale
- **Target Users**: [User type list]
- **User Volume**: [Estimated number]
- **Concurrency Estimate**: [Estimated number]

### User Roles
| Role | Description | Main Functions |
|------|-------------|----------------|
| [Role 1] | [Description] | [Function list] |
| [Role 2] | [Description] | [Function list] |

### Core Functions
1. [Function 1] - [Brief description]
2. [Function 2] - [Brief description]
3. [Function 3] - [Brief description]

### Technical Constraints
- **Deployment Environment**: [Description]
- **Tech Preferences**: [Description]
- **Compliance Requirements**: [Description]

### Timeline & Resources
- **Expected Timeline**: [Time]
- **Team Size**: [Number]

---
Is the above understanding correct? Anything to add or correct?
```

---

## 3. Feature Priority Classification

### Priority Definitions

| Priority | Definition | Criteria |
|:--------:|------------|----------|
| **P0** | Core Feature | MVP must-have, can't work without it |
| **P1** | Necessary Feature | Must have at launch, otherwise affects usage |
| **P2** | Enhancement Feature | Nice to have, can iterate later |
| **P3** | Future Feature | Long-term consideration, just reserve in architecture |

### Classification Principles

1. **Start from user value**: Can users complete core tasks without this feature?
2. **Consider dependencies**: Features that other features depend on have higher priority
3. **Evaluate implementation cost**: Low cost, high value features first
4. **Reserve extension space**: Reserve interfaces for P3 features in architecture

### Agent Guidance Script

```markdown
Now let's classify feature priorities. I'll list all features, please confirm:

**P0 - Core Features (MVP Must-Have)**
These features, without which the system is unusable:
- [ ] [Feature 1]
- [ ] [Feature 2]

**P1 - Necessary Features (Launch Must-Have)**
These features need to be ready at launch, otherwise affects normal usage:
- [ ] [Feature 3]
- [ ] [Feature 4]

**P2 - Enhancement Features (Can Iterate Later)**
Nice to have, but can skip for now:
- [ ] [Feature 5]
- [ ] [Feature 6]

**P3 - Future Features (Reserve in Architecture)**
Might be needed long-term, just need design reservation now:
- [ ] [Feature 7]
- [ ] [Feature 8]

Is this priority classification reasonable?
```

---

## 4. MVP Scope Definition

### MVP Principles

| Principle | Description |
|-----------|-------------|
| **Minimal** | Only include minimum features to validate core value |
| **Viable** | Must be able to run through complete flow |
| **Testable** | Able to collect user feedback |

### MVP Checklist

- [ ] Includes all P0 features
- [ ] Core business flow can run completely
- [ ] Basic user authentication
- [ ] Basic permission control
- [ ] Basic error handling
- [ ] Does not include any P2/P3 feature implementations (can reserve interfaces)

---

## 5. PRD Document Structure

The final PRD should include:

```markdown
# [Project Name] - Product Requirements Document (PRD)

## 1. Project Overview
### 1.1 Project Background
### 1.2 Project Goals
### 1.3 Terminology Definitions

## 2. User Analysis
### 2.1 Target Users
### 2.2 User Roles & Permissions
### 2.3 User Journey

## 3. Functional Requirements
### 3.1 Feature Module Overview
### 3.2 Detailed Module Requirements
(Each module includes: Feature description, User stories, Acceptance criteria)

## 4. Non-Functional Requirements
### 4.1 Performance Requirements
### 4.2 Security Requirements
### 4.3 Availability Requirements
### 4.4 Compatibility Requirements

## 5. System Constraints
### 5.1 Technical Constraints
### 5.2 Business Constraints
### 5.3 Compliance Constraints

## 6. Priority & Milestones
### 6.1 Priority Classification
### 6.2 MVP Scope
### 6.3 Iteration Plan

## 7. Appendix
### 7.1 Prototype/Wireframes (if any)
### 7.2 Business Process Diagrams
### 7.3 Pending Confirmation Items
```

---

## 6. Common Problems & Pitfalls

### Pitfall 1: Scope Creep

**Symptoms**: Users or developers keep adding "might as well do this" features

**Response**: 
- Strictly follow priority classification
- Ask "What priority is this?" for every new feature
- Don't expand MVP scope after it's set

### Pitfall 2: False Requirements

**Symptoms**: "I think users might need..."

**Response**:
- Ask "Is there specific user feedback?"
- Ask "What happens if users don't have this feature?"
- Put uncertain ones in P2/P3

### Pitfall 3: Over-Engineering

**Symptoms**: "Although we don't need it now, we might in the future, let's implement it"

**Response**:
- Distinguish between "architecture reservation" and "feature implementation"
- Reserve interfaces in architecture is fine, but don't implement
- YAGNI principle: You Ain't Gonna Need It

### Pitfall 4: Ignoring Non-Functional Requirements

**Symptoms**: Only focusing on features, ignoring performance, security, maintainability

**Response**:
- Mandatory non-functional requirements section
- Use checklists to ensure nothing is missed
- Validate feasibility during tech selection phase

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |

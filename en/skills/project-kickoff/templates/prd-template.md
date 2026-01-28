# [Project Name] - Product Requirements Document (PRD)

> **Document Version**: v1.0  
> **Created Date**: [Date]  
> **Last Updated**: [Date]  
> **Document Status**: Draft / Under Review / Confirmed

---

## 1. Project Overview

### 1.1 Project Background

[Describe the project background, why this project is being done, what problem it solves]

### 1.2 Project Goals

[Describe the goals the project aims to achieve, preferably measurable]

- Goal 1: ...
- Goal 2: ...

### 1.3 Terminology Definitions

| Term | Definition |
|------|------------|
| [Term 1] | [Definition] |
| [Term 2] | [Definition] |

---

## 2. User Analysis

### 2.1 Target Users

[Describe the target user groups]

### 2.2 User Roles & Permissions

| Role | Description | Main Functions | Data Permissions |
|------|-------------|----------------|------------------|
| [Role 1] | [Description] | [Function list] | [Data scope] |
| [Role 2] | [Description] | [Function list] | [Data scope] |

### 2.3 User Journey

[Describe the typical user usage flow]

```
User Registration → Pending Review → Review Approved → Login → Use Features → ...
```

---

## 3. Functional Requirements

### 3.1 Feature Module Overview

| Module | Priority | Description |
|--------|:--------:|-------------|
| [M1: Module Name] | P0 | [Brief description] |
| [M2: Module Name] | P0 | [Brief description] |
| [M3: Module Name] | P1 | [Brief description] |

### 3.2 Detailed Module Requirements

#### M1: [Module Name]

**Module Description**: [Describe the module's purpose and functional scope]

**Feature List**:

| Feature ID | Feature Name | Priority | Description |
|------------|--------------|:--------:|-------------|
| M1-F1 | [Feature Name] | P0 | [Description] |
| M1-F2 | [Feature Name] | P1 | [Description] |

**M1-F1: [Feature Name]**

- **Feature Description**: [Detailed description]
- **User Story**: As a [role], I want to [action], so that [value]
- **Acceptance Criteria**:
  - [ ] [Criterion 1]
  - [ ] [Criterion 2]
- **Business Rules**:
  - [Rule 1]
  - [Rule 2]
- **UI Prototype**: [Link or description]

#### M2: [Module Name]

[Same format as above]

---

## 4. Non-Functional Requirements

### 4.1 Performance Requirements

| Metric | Requirement |
|--------|-------------|
| Page Load Time | < 3 seconds |
| API Response Time | < 500ms (P95) |
| Concurrent Users | Support [X] concurrent |
| Data Storage | Support [X] records |

### 4.2 Security Requirements

- [ ] Password encrypted storage
- [ ] HTTPS transport
- [ ] Sensitive data masking
- [ ] Operation log recording
- [ ] [Other security requirements]

### 4.3 Availability Requirements

| Metric | Requirement |
|--------|-------------|
| System Availability | [e.g., 99.9%] |
| Data Backup | [e.g., Daily backup] |
| Disaster Recovery | [e.g., RTO < 4 hours] |

### 4.4 Compatibility Requirements

- **Browsers**: [e.g., Chrome 80+, Firefox 75+, Edge 80+]
- **Resolution**: [e.g., 1280x720 and above]
- **Devices**: [e.g., Desktop priority]

---

## 5. System Constraints

### 5.1 Technical Constraints

- [e.g., Must use company's existing MySQL database]
- [e.g., Must deploy on internal servers]

### 5.2 Business Constraints

- [e.g., Need to integrate with existing XX system]
- [e.g., Need to support XX format data import]

### 5.3 Compliance Constraints

- [e.g., Need to meet SOC2 requirements]
- [e.g., User data must be stored domestically]

---

## 6. Priority & Milestones

### 6.1 Priority Definitions

| Priority | Definition | Criteria |
|:--------:|------------|----------|
| **P0** | Core | MVP must-have, can't work without it |
| **P1** | Necessary | Must have at launch, otherwise affects usage |
| **P2** | Enhancement | Nice to have, can iterate later |
| **P3** | Future | Long-term consideration, just reserve in architecture |

### 6.2 MVP Scope

MVP includes the following features:

- [x] M1: [Module Name] (P0)
- [x] M2: [Module Name] (P0)
- [ ] M3: [Module Name] (P1) - Not in MVP

### 6.3 Iteration Plan

| Phase | Timeline | Included Modules | Goal |
|-------|----------|------------------|------|
| Phase 1 (MVP) | [Time] | M1, M2 | Run through core flow |
| Phase 2 | [Time] | M3, M4 | Complete basic features |
| Phase 3 | [Time] | M5, M6 | Enhance experience |

---

## 7. Appendix

### 7.1 Business Process Diagrams

[Insert diagrams or links]

### 7.2 Prototype Links

[Figma/Axure links]

### 7.3 Pending Confirmation Items

| ID | Question | Status | Owner |
|----|----------|:------:|-------|
| Q1 | [Question description] | ⬜ | [Name] |
| Q2 | [Question description] | ✅ | [Name] |

### 7.4 References

- [Reference 1]
- [Reference 2]

---

## Document Revision History

| Version | Date | Reviewer | Changes |
|---------|------|----------|---------|
| v1.0 | [Date] | [Name] | Initial version |

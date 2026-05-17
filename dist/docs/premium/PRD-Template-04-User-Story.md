# PRD Template 04: User Story Mapping Document

## For Feature Planning Using User Stories and Acceptance Criteria

This PRD focuses on organizing features as user stories with clear acceptance criteria. Use for agile development, sprint planning, and ensuring development team alignment on requirements.

---

## Document Information

**Product:** _________________________________  
**Version:** 1.0 | **Date:** ____________ | **Author:** _________________________________

---

## Section 1: User Story Framework

### 1.1 User Story Template

All user stories follow this format:

**As a** [type of user]  
**I want** [goal or action]  
**So that** [benefit or value received]

### 1.2 Acceptance Criteria Template

Each story must have clear acceptance criteria:

**Given** [context or preconditions]  
**When** [action performed]  
**Then** [expected outcome]

---

## Section 2: Epic Mapping

### Epic 1: [Epic Name]

**Description:** _________________________________

**Business Value:** _________________________________

**Target Release:** _________________________________

#### User Stories in This Epic

| ID | Story | Points | Priority | Status |
|----|-------|--------|----------|--------|
| US-001 | | | | |
| US-002 | | | | |
| US-003 | | | | |

---

### Epic 2: [Epic Name]

**Description:** _________________________________

**Business Value:** _________________________________

**Target Release:** _________________________________

#### User Stories in This Epic

| ID | Story | Points | Priority | Status |
|----|-------|--------|----------|--------|
| US-004 | | | | |
| US-005 | | | | |
| US-006 | | | | |

---

## Section 3: User Story Detail

### US-001: [Story Title]

**Story:**

**As a** _________________________________  
**I want** _________________________________  
**So that** _________________________________

**Acceptance Criteria:**

| # | Given | When | Then |
|---|-------|------|------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

**Technical Notes:**

- _______________________________________________________________________________
- _______________________________________________________________________________

**Design Reference:** _________________________________

**Dependencies:**

- US-000 (must be completed first)
- Backend API endpoint /api/___ must exist

**Definition of Done:**

- [ ] Code complete
- [ ] Unit tests passing
- [ ] Integration tests passing
- [ ] Accessibility tested
- [ ] Mobile responsive
- [ ] Documentation updated

---

### US-002: [Story Title]

**Story:**

**As a** _________________________________  
**I want** _________________________________  
**So that** _________________________________

**Acceptance Criteria:**

| # | Given | When | Then |
|---|-------|------|------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

**Technical Notes:**

- _______________________________________________________________________________
- _______________________________________________________________________________

**Design Reference:** _________________________________

**Dependencies:** _________________________________

**Definition of Done:**

- [ ] Code complete
- [ ] Unit tests passing
- [ ] Integration tests passing
- [ ] Accessibility tested
- [ ] Mobile responsive
- [ ] Documentation updated

---

### US-003: [Story Title]

**Story:**

**As a** _________________________________  
**I want** _________________________________  
**So that** _________________________________

**Acceptance Criteria:**

| # | Given | When | Then |
|---|-------|------|------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

**Technical Notes:**

- _______________________________________________________________________________
- _______________________________________________________________________________

**Design Reference:** _________________________________

**Dependencies:** _________________________________

**Definition of Done:**

- [ ] Code complete
- [ ] Unit tests passing
- [ ] Integration tests passing
- [ ] Accessibility tested
- [ ] Mobile responsive
- [ ] Documentation updated

---

## Section 4: Story Mapping Board

### Row: User Activities (Things users do)

| Activity | Feature 1 | Feature 2 | Feature 3 | Feature 4 |
|----------|-----------|-----------|-----------|-----------|
| Discover | | | | |
| Evaluate | | | | |
| Sign up | | | | |
| Onboard | | | | |
| Use core | | | | |
| Get value | | | | |
| Refer | | | | |

### Column: User Emotions (How users feel)

- **Happy Path:** What makes them successful?
- **Edge Cases:** What could go wrong?
- **Error Handling:** How do we recover?
- **Backstage:** What happens behind the scenes?

---

## Section 5: Wireframe References

| Screen | Location | Notes |
|--------|----------|-------|
| [Screen Name] | /designs/screen-name.png | Main flow |
| [Screen Name] | /designs/screen-name.png | Empty state |
| [Screen Name] | /designs/screen-name.png | Error state |
| [Screen Name] | /designs/screen-name.png | Loading state |

---

## Section 6: Sprint Planning

### Sprint 1 (2 weeks)

**Goal:** Core user activation

**Stories:**

- US-001: Onboarding flow
- US-002: First core feature
- US-003: Basic dashboard

**Total Points:** ____

**Definition of Done:**

- [ ] All stories in "Done" column
- [ ] No critical bugs
- [ ] Product Owner acceptance

---

### Sprint 2 (2 weeks)

**Goal:** Engagement and retention

**Stories:**

- US-004: Notification system
- US-005: User settings
- US-006: Help documentation

**Total Points:** ____

---

### Sprint 3 (2 weeks)

**Goal:** Polish and launch prep

**Stories:**

- US-007: Performance optimization
- US-008: Mobile responsiveness
- US-009: Analytics integration

**Total Points:** ____

---

## Section 7: Definition of Done

All user stories must meet these criteria to be considered complete:

1. **Code Complete:** All acceptance criteria implemented
2. **Tests Passing:** Unit tests at 80%+ coverage, all passing
3. **Code Review:** Reviewed and approved by at least one peer
4. **Documentation:** API docs, inline comments updated
5. **Accessibility:** WCAG 2.1 AA compliance verified
6. **Performance:** Page load under 3s, API response under 500ms
7. **Security:** No vulnerability findings
8. **Mobile Ready:** Responsive on iOS and Android
9. **Staging Verified:** Works in staging environment
10. **Product Accepted:** Product Owner approves functionality

---

## Section 8: Appendix

### A. Story Dependencies Graph

```
[US-001] ──→ [US-002] ──→ [US-004]
    │                        │
    ▼                        ▼
[US-003] ──────────→ [US-005] ──→ [US-006]
```

### B. Risk Assessment

| Story | Risk | Mitigation |
|-------|------|------------|
| | | |
| | | |

### C. Change Log

| Date | Story | Change | Reason |
|------|-------|--------|--------|
| | | | |
| | | | |

---

**Document Approval:**

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | | | |
| Scrum Master | | | |
| Tech Lead | | | |
| QA Lead | | | |
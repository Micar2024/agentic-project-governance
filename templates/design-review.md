# Design Review

> Use this template before implementing or merging changes that affect user experience, public-facing copy, safety labels, information architecture, interaction flows, or human-AI collaboration boundaries.

## 1. Design Review Summary

| Field | Value |
|---|---|
| Feature / change name | [name] |
| Review date | [YYYY-MM-DD] |
| Reviewer | [name or role] |
| Status | [PASS / PARTIAL PASS / FAIL / BLOCKED] |

## 2. Context

- **What is changing**: [describe the change in one or two sentences]
- **Who is affected**: [end users, internal users, specific roles, all visitors]
- **Why this change is needed**: [business reason, user need, safety requirement, or bug fix]
- **Source of truth**: [issue link, design spec, or conversation reference]

## 3. Scope

### In scope

- [list files, screens, components, or flows that will change]

### Out of scope

- [list files, screens, components, or flows that must NOT change]

### Files / surfaces affected

| File or surface | Type of change |
|---|---|
| [file path or screen name] | [new / modified / deleted] |

## 4. User Impact

### Primary user flow

[Describe the main user journey affected by this change. What does the user see, do, and expect?]

### User-visible copy

[List all user-facing text changes: labels, messages, prompts, tooltips, error messages, empty states.]

| Location | Current text | Proposed text |
|---|---|---|
| [screen or component] | [current] | [proposed] |

### Error / empty / loading states

- [ ] Error state designed and reviewed
- [ ] Empty state designed and reviewed
- [ ] Loading state designed and reviewed
- [ ] Edge cases considered (long text, missing data, timeout)

### Accessibility

- [ ] Color contrast meets minimum standards
- [ ] Information is not conveyed by color alone
- [ ] Interactive elements are keyboard-accessible
- [ ] Screen reader labels are present and meaningful

### Internationalization (if applicable)

- [ ] Text is not hardcoded in a single language
- [ ] Layout supports text expansion
- [ ] Date, number, and currency formats are considered

## 5. Risk Review

| Risk type | Present? | Description | Mitigation |
|---|---|---|---|
| Safety-critical wording | Yes / No | [e.g., risk labels, warnings, disclaimers] | [how preserved] |
| Data exposure risk | Yes / No | [e.g., showing private info in UI] | [how prevented] |
| Permission / privacy impact | Yes / No | [e.g., new data collection, sharing] | [how handled] |
| Misleading UI risk | Yes / No | [e.g., ambiguous status, false confidence] | [how clarified] |
| Regression risk | Yes / No | [e.g., breaking existing flows] | [how tested] |
| Operational risk | Yes / No | [e.g., increased load, new dependency] | [how mitigated] |

### Safety-critical wording rules

If the change touches risk labels, safety warnings, disclaimers, or status indicators:

- Weakening, removing, or obscuring safety-critical text is FORBIDDEN.
- Visual aesthetics must not override safety expression.
- Ambiguous or reassuring language must not replace precise risk statements.
- Human approval is REQUIRED before merge.

## 6. AI-Agent Execution Boundaries

### What the agent may change

- [list specific files, components, or content the agent is authorized to modify]

### What the agent must NOT change

- [list files, components, or content the agent must leave untouched]
- [include safety-critical text, legal copy, risk labels, etc.]

### Human approval required before

- [ ] Merging this change
- [ ] Publishing or deploying
- [ ] Modifying safety-critical wording
- [ ] Changing user-facing error or warning messages
- [ ] Altering permission or privacy-related UI

### Stop conditions

The agent must stop and request human input when:

- Safety-critical wording needs modification
- User-visible copy changes affect risk communication
- The change touches data exposure or permission boundaries
- Acceptance criteria cannot be verified
- Scope expands beyond the approved list

## 7. Acceptance Criteria

| Criterion | Verification method |
|---|---|
| Visual correctness | Screenshots or screen recording |
| Behavior correctness | Manual walkthrough or test output |
| Copy correctness | Diff of all user-facing text |
| Safety boundary preserved | Checklist confirmation |
| Accessibility verified | Contrast check, keyboard test |
| Evidence required | Attach evidence to PR or acceptance report |

## 8. Review Decision

| Verdict | Meaning |
|---|---|
| PASS | All criteria met, no risks remain, ready to merge |
| PARTIAL PASS | Mostly complete, minor gaps or follow-up needed |
| FAIL | Significant issues, must be reworked |
| BLOCKED | Cannot proceed without human decision or external input |

### Decision

- **Verdict**: [PASS / PARTIAL PASS / FAIL / BLOCKED]
- **Reason**: [one sentence]
- **Required follow-up**: [list any remaining actions, or "none"]
- **Human approval obtained**: [Yes / No / N/A]


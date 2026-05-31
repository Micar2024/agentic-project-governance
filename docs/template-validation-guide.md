# Template Validation Guide

This guide helps reviewers check whether a task packet, PR review packet, or acceptance report is complete, well-scoped, and ready for action. It is a human-readable checklist, not an automated validator.

## Validation Levels

Use these levels to judge template quality:

- **PASS** -- Information is complete, scope is clear, work is executable, and acceptance criteria are verifiable.
- **PARTIAL PASS** -- Mostly usable, but has gaps, unclear boundaries, or missing evidence. Requiresfill in before proceeding.
- **FAIL** -- Objective is unclear, scope is uncontrolled, acceptance criteria are missing, or safety risks exist. Must be reworked before any execution.
- **BLOCKED** -- Cannot proceed due to external dependency, missing authorization, or unresolved risk that requires human decision.

## Task Packet Validation Checklist

Check each item. Mark as Present, Missing, or Unclear.

| # | Item | What to look for |
|---|------|------------------|
| 1 | Clear objective | One-sentence goal that states what to achieve, not how |
| 2 | Explicit scope | Files, modules, or areas that may be changed |
| 3 | Out-of-scope items | Files, modules, or areas that must not be touched |
| 4 | Execution steps | Ordered steps the implementer should follow |
| 5 | Acceptance criteria | Specific, verifiable conditions that define "done" |
| 6 | Report format | How the implementer should document results |
| 7 | Stop conditions | Conditions that require stopping and asking for human input |
| 8 | Risk notes | Known risks, edge cases, or failure modes |
| 9 | Human approval points | Actions that require explicit human authorization before execution |

### Task Packet Verdict

- **PASS**: All 9 items present and clear.
- **PARTIAL PASS**: 1-2 items missing or unclear, no safety gaps.
- **FAIL**: 3+ items missing, or objective/scope is vague, or stop conditions absent.
- **BLOCKED**: Human approval required but not yet obtained.

## PR Review Packet Validation Checklist

| # | Item | What to look for |
|---|------|------------------|
| 1 | PR link | Valid pull request URL |
| 2 | Branch and target | Source branch and target branch clearly stated |
| 3 | Files changed | List of all modified, added, or deleted files |
| 4 | Summary of changes | What changed and why, in plain language |
| 5 | Issue linkage | Related issue number or URL |
| 6 | Validation performed | What tests, checks, or verifications were run |
| 7 | Scope discipline | No changes outside the approved scope |
| 8 | Security/sanitization | No secrets, credentials, or private data in the diff |
| 9 | Reviewer recommendation | APPROVE, REQUEST CHANGES, or BLOCK with reason |

### PR Review Packet Verdict

- **PASS**: All 9 items present, scope clean, recommendation is APPROVE.
- **PARTIAL PASS**: 1-2 items missing or weak, no scope violations.
- **FAIL**: Scope discipline violated, or security/sanitization check failed, or recommendation is BLOCK.
- **BLOCKED**: Reviewer cannot verify safety or scope; human intervention required.

## Acceptance Report Validation Checklist

| # | Item | What to look for |
|---|------|------------------|
| 1 | Final status | PASS, PARTIAL PASS, FAIL, or BLOCKED |
| 2 | Evidence reviewed | Test results, screenshots, command output, or verification steps |
| 3 | What passed | Specific items that met acceptance criteria |
| 4 | What failed or is risky | Specific items that did not meet criteria or carry residual risk |
| 5 | Merge/release recommendation | APPROVE, HOLD, or REJECT with reason |
| 6 | Follow-up actions | Remaining work, if any |
| 7 | Stop conditions if unresolved | What happens if follow-up actions are not completed |

### Acceptance Report Verdict

- **PASS**: All 7 items present, final status is PASS, evidence is concrete.
- **PARTIAL PASS**: 1-2 items missing or weak, no blocking risks.
- **FAIL**: Evidence absent or unverifiable, or final status contradicts the evidence.
- **BLOCKED**: Final status is BLOCKED with clear reason and human decision pending.

## Common Failure Patterns

Watch for these patterns when validating templates:

| Pattern | Problem | Impact |
|---|---|---|
| Missing acceptance criteria | No way to verify "done" | Work may be incomplete or wrong |
| Vague scope | "Update the project" with no file list | Scope creep, unintended changes |
| Hidden production impact | Task touches config, secrets, or deploy | Safety risk not flagged |
| No evidence | Claims "tests pass" without output | Unverifiable claims |
| No rollback or stop condition | No plan if things go wrong | Cannot recover from failure |
| Mixing unrelated changes | Bug fix bundled with feature work | Hard to review, hard to revert |
| Real internal data in examples | Customer names, paths, tokens in templates | Privacy/security leak |
| Agent output treated as verified fact | "AI said it works" without testing | False confidence |

## Lightweight Review Workflow

Recommended process for reviewing templates:

1. **Author fills template** using the appropriate template from `templates/`.
2. **Reviewer checks checklist** against the relevant validation checklist above.
3. **Reviewer returns verdict**: PASS, PARTIAL PASS, FAIL, or BLOCKED.
4. **If PARTIAL PASS**: Authorfill in missing items, reviewer re-checks.
5. **If FAIL**: Template is reworked from scratch with clearer scope and criteria.
6. **If BLOCKED**: Human decision required before proceeding.
7. **Human approval required** for medium/high risk actions (see `docs/workflow.md`).
8. **Unresolved risks block** merge and release.

## Example Validation

This is a fictional example showing how to apply the checklists.

### Scenario

A team submits a task packet to "fix a typo in the README." The packet includes:

- Objective: Fix spelling error in README line 42
- Scope: README.md only
- Out of scope: All other files
- Steps: Edit line 42, commit, push
- Acceptance criteria: Typo is corrected, no other lines changed
- Report format: Show before/after diff
- Stop conditions: If README has other issues, ask first
- Risk notes: None (low risk)
- Human approval: Not required

### Validation Result

| Template | Verdict | Reason |
|---|---|---|
| Task packet | PASS | All 9 items present, scope is tight, criteria are verifiable |
| PR review packet | (pending) | To be filled after PR is created |
| Acceptance report | (pending) | To be filled after review |

### Another Scenario

A team submits a task packet to "improve the dashboard." The packet includes:

- Objective: Make the dashboard better
- Scope: Dashboard files
- Out of scope: (empty)
- Steps: Review current dashboard, make improvements
- Acceptance criteria: Dashboard looks better
- Report format: Screenshots
- Stop conditions: (empty)
- Risk notes: (empty)
- Human approval: (empty)

### Validation Result

| Template | Verdict | Reason |
|---|---|---|
| Task packet | FAIL | Vague objective, no out-of-scope, no clear acceptance criteria, no stop conditions |
| PR review packet | BLOCKED | Cannot review without a clear task |
| Acceptance report | BLOCKED | Cannot accept without verifiable criteria |

This task packet must be reworked before any execution begins.

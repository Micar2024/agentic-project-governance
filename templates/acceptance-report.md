# Acceptance Report

An acceptance report closes a task with evidence. It states what was done, how it was verified, what remains risky, and whether the task passes.

If you cannot write an acceptance report, the task is not done.

## 1. Requested task

What was requested? Link to the task packet or issue. Examples:

- Implement login retry logic (task packet TP-20260531-001)
- Add FAQ documentation (issue #42)

## 2. Completed work

What was changed? Be specific. Examples:

- Added retry logic to `src/auth/login.ts` with 3 retries and 2s backoff
- Created `docs/faq.md` with 5 entries
- Updated README to link to FAQ

## 3. Not changed

What was intentionally left untouched? Examples:

- Signup flow (out of scope)
- Rate limiting (not requested)
- Database schema (no migration needed)

## 4. Acceptance criteria mapping

Map each acceptance criterion from the task packet to evidence and result.

| Acceptance criterion | Evidence | Result |
|---|---|---|
| Criterion 1 | Evidence source | PASS / FAIL |
| Criterion 2 | Evidence source | PASS / FAIL |
| Criterion 3 | Evidence source | PASS / FAIL |

## 5. Verification evidence

How was the result verified? Include specific commands, logs, screenshots, tests, or direct inspection. Examples:

```bash
npm test
# 12 tests pass, 0 fail
```

Or:

- `docs/faq.md` exists and contains 5 sections
- README link to FAQ resolves correctly
- `grep -r "oldFunction" src/auth/` returns nothing

If verification was not possible, say so and explain why.

## 5. Result

Choose one:

- **PASS**: All acceptance criteria met, evidence collected, no blocking risks.
- **HOLD**: Work is done but requires human review, additional verification, or has open questions.
- **FAIL**: Acceptance criteria not met, evidence insufficient, or blocking issues found.

## 6. Residual risks

What risks remain after completion? Examples:

- Retry logic may need tuning based on production metrics
- FAQ entries are English-only; translations needed for other locales

## 7. Rollback path

How to undo the change if needed. Examples:

- `git revert <commit>`
- Delete `docs/faq.md` and remove README link
- Restore `src/auth/login.ts` from backup

## 8. Follow-up tasks

What open items or next actions exist? Examples:

- Tune retry parameters based on production data
- Add FAQ translations
- Monitor login error rates

## 9. Human approval status

- Approved: Human has reviewed and approved.
- Pending: Awaiting human review.
- Not required: Task does not need human approval.

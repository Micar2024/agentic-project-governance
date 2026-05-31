# PR Review Packet

A PR review packet summarizes what changed, why, and whether the change matches the task packet. It is the reviewer's evidence that the change is safe to accept.

A PR without a review packet is not ready for merge.

## 1. Summary

What changed? One or two sentences. Examples:

- Added retry logic to the login endpoint
- Updated FAQ documentation with 5 new entries
- Refactored the auth module to use async/await

## 2. Linked task packet

Link to the issue or task packet that defines this work. Examples:

- Closes #42
- Implements task packet from `task-packets/TP-20260531-001.md`

## 3. Files changed

List major files and why they changed. Examples:

| File | Reason |
|---|---|
| `src/auth/login.ts` | Added retry logic |
| `src/auth/login.test.ts` | Added tests for retry logic |
| `docs/faq.md` | New FAQ entries |

## 4. Scope compliance

Did the change stay within the allowed scope?

- Within allowed scope: Yes / No
- Touched forbidden scope: Yes / No

If either answer is No, explain why.

## 5. Verification evidence

What evidence proves the change works? Include commands, logs, screenshots, tests, or inspection notes. Examples:

```bash
npm test -- --grep "login retry"
# All 12 tests pass
```

Or:

- Manual inspection of `docs/faq.md` confirms 5 new entries
- Link check passes: no broken links

## 6. Known risks

What risks remain after this change? Examples:

- Retry logic may mask network issues if retry count is too high
- FAQ entries are in English only; translations pending

## 7. Out-of-scope items

What was intentionally not changed? Examples:

- Did not update the signup flow (separate task)
- Did not add rate limiting (out of scope)

## 8. Reviewer checklist

- [ ] Change matches task packet
- [ ] No forbidden files touched
- [ ] Evidence is sufficient
- [ ] Risks are stated
- [ ] No secrets or private data included
- [ ] No unnecessary scope expansion

## 9. What this packet does not prove

This packet does not prove production safety, business correctness, or future compatibility unless explicitly verified. It documents what changed, why, and what evidence was collected.

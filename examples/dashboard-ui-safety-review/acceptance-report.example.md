# Acceptance Report — Dashboard UI Safety Review

## Requested task

Standardize the SitePulse dashboard's `StatusBadge` component to use design system v2.1 while preserving all safety-critical text verbatim. Task packet: `task-packet.example.md`.

## Completed work

- Updated `StatusBadge.tsx` to use design system v2.1 color tokens, typography, and spacing.
- Preserved all 3 safety-critical strings verbatim:
  - `"Critical: service unreachable"` (Down state)
  - `"Data may be delayed — verify manually"` (Unknown state)
  - `"Do not rely on cached data during degraded state"` (Degraded state)
- Overrode the design system's text-length recommendation for safety reasons — documented with a source comment.
- Updated `StatusBadge.test.tsx` to test against new design tokens.
- Updated `badge.css` to use `var(--badge-*)` design tokens instead of custom hex values.

## Not changed

- `AlertBanner.tsx` — not in scope
- `IncidentCard.tsx` — not in scope
- Backend or API — not in scope
- Monitoring logic or alerting rules — not in scope
- Safety-critical text strings — verified unchanged (automated + manual)

## Safety governance: violation caught and corrected

During development, an automated safety check caught a governance violation:

| What happened | How it was caught | How it was fixed |
|---|---|---|
| Design system recommended short labels; "Critical: service unreachable" was shortened to "Service unreachable" | `check-safety-text.js` flagged the string mismatch | Reverted to original text; overrode design system recommendation; added safety governance comment in source |

This is the governance system **working as designed**: automated checks caught a safety regression before it reached human review, saving reviewer time and preventing a potential production incident.

## Verification evidence

### Automated

```bash
$ npm test -- src/components/StatusBadge.test.tsx
7 tests passed, 0 failed.

$ node scripts/check-safety-text.js
All 3 safety strings preserved. PASS.

$ npx chromatic test --component StatusBadge
4 stories, 0 visual changes. PASS.

$ npx axe src/components/StatusBadge.tsx
0 accessibility violations. PASS.
```

### Manual

| Check | Result | Reviewer |
|---|---|---|
| "Critical: service unreachable" present and unchanged | ✓ PASS | Human reviewer |
| "Data may be delayed" warning preserved | ✓ PASS | Human reviewer |
| "Do not rely on cached data" advisory preserved | ✓ PASS | Human reviewer |
| Visual hierarchy: Down > Degraded > Unknown > Healthy | ✓ PASS | Human reviewer |
| Safety governance comment present in source | ✓ PASS | Human reviewer |
| Color is not the only differentiator (icons present) | ✓ PASS | Human reviewer |

## Result

**PASS**

All acceptance criteria met. Safety text preserved. Automated guardrails passed. Human approval obtained (see below).

## Residual risks

- Future design system updates may change the red color token (`--badge-critical`) to something less alarming. The design system team should be made aware of this component's safety requirements.
- New dashboard pages may create their own badge implementations instead of using this shared component. Mitigation: the safety text check script scans all components, not just `StatusBadge.tsx`.
- I18n/l10n: the safety text is currently English-only. Translating these strings requires the same governance review as modifying them.

## Rollback path

- `git revert <commit>` — clean revert. The old component had the same safety text, so safety is preserved.
- Design token rollback not needed — the old custom hex values were functionally identical.

## Follow-up tasks

- [ ] Notify design system team that `StatusBadge` intentionally overrides the badge text-length spec (tracked in Issue #214)
- [ ] Add `StatusBadge` to the safety-text monitoring dashboard (tracked in Issue #215)
- [ ] Consider extracting safety-critical strings into a constants file with a linter rule (tracked in Issue #216)
- [ ] I18n process: define governance requirements for translating safety text (tracked in Issue #217)

## Human approval status

**APPROVED**

| Field | Detail |
|---|---|
| Approver | jane-doe (dashboard maintainer) |
| Date | 2026-05-30 |
| Method | Manual review of all 3 strings + visual hierarchy check |
| Rationale | Safety text preserved, visual consistency improved, governance comment documented |

The approval is recorded here as part of the acceptance report for audit trail purposes. The PR merge is unblocked.

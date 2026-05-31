# PR Review Packet — Dashboard UI Safety Review

## Summary

Standardizes the SitePulse dashboard's `StatusBadge` component to use design system v2.1 color tokens, typography, and spacing. All safety-critical text strings are preserved verbatim. A governance violation (weakening of "Critical: service unreachable" to "Service unreachable") was caught by automated safety checks during development and fixed before PR submission.

## Linked task packet

`task-packet.example.md` — includes the safety violation catch and correction log.

## Safety governance gate (MANDATORY REVIEW)

This PR touches `src/components/StatusBadge.tsx`, which contains safety-critical user-facing text. The following governance rule applies:

> **Rule S1:** Any change to safety-critical text (risk warnings, alert labels, severity indicators) requires human approval before merge. Visual improvements must not weaken the clarity of safety communication. Safety > Aesthetics.

### Safety text integrity check

| String | Status | Verified by |
|---|---|---|
| `"Critical: service unreachable"` | **UNCHANGED** ✓ | `check-safety-text.js` + manual |
| `"Data may be delayed — verify manually"` | **UNCHANGED** ✓ | `check-safety-text.js` + manual |
| `"Do not rely on cached data during degraded state"` | **UNCHANGED** ✓ | `check-safety-text.js` + manual |

### Safety-first design decision

The design system v2.1 badge spec recommends short labels (max 25 characters). The safety-critical labels are 31, 38, and 48 characters respectively. **The design system recommendation was intentionally overridden** for this component. A comment was added in the source code to document this decision.

## Files changed

| File | Reason |
|---|---|
| `src/components/StatusBadge.tsx` | Updated colors, typography, spacing to design system v2.1. Preserved all safety text. Added safety governance comment. |
| `src/components/StatusBadge.test.tsx` | Updated test assertions to match new design tokens. No safety text tests were changed. |
| `src/styles/badge.css` | Replaced custom hex colors with `var(--badge-*)` design tokens. Added `.badge-safety-critical` class for documentation. |

## Scope compliance

- Within allowed scope: Yes
- Touched forbidden scope: **No** — `AlertBanner.tsx`, `IncidentCard.tsx`, backend, API untouched
- Safety text modified: **No** — all 3 strings preserved verbatim

## Verification evidence

### Automated checks

```bash
$ npm test -- src/components/StatusBadge.test.tsx
 PASS  src/components/StatusBadge.test.tsx
  ✓ renders healthy badge (12ms)
  ✓ renders degraded badge with safety text (8ms)
  ✓ renders down badge with "Critical:" prefix (7ms)
  ✓ renders unknown badge with manual verify warning (6ms)
  ✓ uses design system color tokens (5ms)
  ✓ applies correct typography classes (4ms)
  ✓ preserves all safety-critical text verbatim (3ms)

Test Suites: 1 passed, 1 total
Tests:       7 passed, 7 total

$ node scripts/check-safety-text.js
PASS: "Critical: service unreachable" — preserved verbatim
PASS: "Data may be delayed — verify manually" — preserved verbatim
PASS: "Do not rely on cached data during degraded state" — preserved verbatim
All 3 safety strings verified. No violations.

$ npx chromatic test --component StatusBadge
✓ StatusBadge: 4 stories, 0 visual changes (baseline matched)
  - Healthy state: no change
  - Degraded state: no change
  - Down state: no change
  - Unknown state: no change

$ npx axe src/components/StatusBadge.tsx
0 accessibility violations found.
```

### Manual review

- [x] Down state is still visually distinct (red background + "Critical:" prefix + icon)
- [x] Color contrast ratios meet WCAG AA for all badge states
- [x] Icons are present alongside color — color is not the only differentiator
- [x] Safety governance comment is present in the source code

## Known risks

- Design system may update the badge spec in the future. The maintainer who applies that update must be aware of the safety text override in this component. Mitigation: the source comment and this PR review packet document the override.
- The `--badge-critical` design token is defined as `#dc3545` (red). If the design system changes this to a less alarming color in future, it could weaken the perceived severity. Mitigation: this is tracked as a design system dependency risk, not a code change risk.

## Out-of-scope items

- `AlertBanner.tsx` — not touched. Alert text is unchanged.
- `IncidentCard.tsx` — not touched. Severity display is unchanged.
- Backend monitoring logic — not touched.
- Design system v2.1 itself — not in scope (consumed as-is).

## Reviewer checklist

- [x] Change matches task packet
- [x] No forbidden files touched
- [x] Safety-critical text is preserved verbatim
- [x] Automated safety check passes
- [x] Evidence is sufficient
- [x] Risks are stated
- [x] No secrets or private data included
- [x] No unnecessary scope expansion

## Human approval gate

**STATUS: PENDING**

This PR cannot be merged until a human reviewer has:
1. Manually verified all 3 safety-critical strings are unchanged
2. Confirmed the visual hierarchy (Down > Degraded > Unknown > Healthy) is preserved
3. Signed off in the acceptance report

The human approval requirement is **not bypassable** by automated checks alone. This is by design — safety-critical text requires human judgment.

## What this packet does not prove

This packet does not prove that the design system tokens will render identically in all browsers, that the visual regression baselines are accurate, or that future design system updates won't reintroduce the safety text issue. The safety governance comment in the source code is a documentation mitigation, not a technical enforcement.

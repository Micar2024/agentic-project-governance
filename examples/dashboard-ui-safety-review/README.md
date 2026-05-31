# Dashboard UI Safety Review — Human Approval Gate Example

A fictional dashboard UI change involving user-visible risk indicators and alert text. Demonstrates the governance requirement that **human approval is mandatory** before merging changes that affect safety-critical user-facing content.

## What this example demonstrates

- A UI change that touches user-visible risk indicators and alert messages
- Automated checks catching a violation of safety-text requirements
- The governance rule: "never weaken or obscure risk warnings for aesthetic reasons"
- Human approval as a **mandatory merge gate** — not optional, not bypassable
- Concrete acceptance criteria for safety-related UI changes

## Governance concepts covered

- **Human approval gate**: changes to user-visible safety/risk text MUST have human sign-off
- **Safety over aesthetics**: visual improvements must not weaken the clarity of risk communication
- **Automated guardrails**: linter/check catching forbidden changes to safety-critical text
- **Merge blocking**: the PR cannot be merged until the human approval condition is satisfied
- **Audit trail**: documenting who approved, when, and why

## Risk level

Medium. UI-only change, but affects user-facing risk communication.

## Channel decision

Standard channel with mandatory human approval gate. Cannot be fast-channeled due to safety-text involvement.

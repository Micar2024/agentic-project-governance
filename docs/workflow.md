# Workflow

## Standard channel

Use the standard channel for non-trivial, risky, cross-file, production-adjacent, or externally visible work.

1. Define task packet.
2. Confirm scope and forbidden scope.
3. Implement on a branch or isolated workspace.
4. Produce evidence.
5. Prepare PR review packet.
6. Run independent review.
7. Prepare acceptance report.
8. Human decides merge, release, or hold.

## Fast channel

Use the fast channel only for low-risk edits:

- Typos
- Small documentation edits
- Formatting fixes
- Clearly reversible changes

Upgrade to the standard channel if the task touches security, production, credentials, data, permissions, release, or multiple modules.

## Stop conditions

Stop and ask a human when:

- Scope is unclear
- Required files or facts are missing
- The agent needs permission escalation
- The task requires external publishing
- The task involves secrets, production systems, or destructive actions
- Verification cannot be performed

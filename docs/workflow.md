# Workflow

This document defines how tasks flow through the governance process, including channel selection, escalation triggers, and human approval gates.

## Channel selection

Use this decision tree to determine which channel a task should enter.

```mermaid
flowchart TD
    Start[New task] --> Q1{Low risk?}
    Q1 -- No --> Std[Standard channel]
    Q1 -- Yes --> Q2{Small scope?}
    Q2 -- No --> Std
    Q2 -- Yes --> Q3{No external behavior change?}
    Q3 -- No --> Std
    Q3 -- Yes --> Q4{No sensitive data involved?}
    Q4 -- No --> Std
    Q4 -- Yes --> Q5{Quickly human-verifiable?}
    Q5 -- No --> Std
    Q5 -- Yes --> Fast[Fast channel]
```

### Fast channel criteria

All of the following must be true:

- Low risk, clearly reversible change
- Small file scope (one or two files)
- No external behavior change
- No sensitive data, credentials, or production systems involved
- No configuration, permission, or dependency changes
- Quickly human-verifiable

Examples: typo fixes, minor documentation edits, formatting corrections.

### Standard channel criteria

Any of the following triggers the standard channel:

- Involves code logic changes
- Affects user-visible behavior
- Touches security boundaries
- Involves configuration, permissions, releases, data, or dependencies
- Requires independent review
- Requires acceptance evidence

## Standard channel workflow

```mermaid
flowchart LR
    A[Human objective] --> B[Task packet]
    B --> C[Implementation]
    C --> D[Pull request]
    D --> E[Independent review]
    E --> F[Acceptance report]
    F --> G[Human decision]
```

1. Define task packet with scope, forbidden scope, verification commands, and rollback path.
2. Confirm scope and forbidden scope with the human.
3. Implement on a branch or isolated workspace.
4. Produce evidence (test results, screenshots, verification output).
5. Prepare PR review packet.
6. Run independent review (reviewer must not be the implementer).
7. Prepare acceptance report with evidence.
8. Human decides: merge, hold, or reject.

## Escalation triggers

A task that starts in the fast channel must upgrade to the standard channel if any of the following are discovered during execution.

```mermaid
flowchart TD
    Fast[Fast channel task] --> T1{Impact larger than expected?}
    T1 -- Yes --> Esc[Upgrade to standard channel]
    T1 -- No --> T2{Additional files need changes?}
    T2 -- Yes --> Esc
    T2 -- No --> T3{Backward compatibility risk?}
    T3 -- Yes --> Esc
    T3 -- No --> T4{User-visible copy or risk labels affected?}
    T4 -- Yes --> Esc
    T4 -- No --> T5{Data, permissions, config, secrets, or production touched?}
    T5 -- Yes --> Esc
    T5 -- No --> T6{Reviewer judgment needed?}
    T6 -- Yes --> Esc
    T6 -- No --> T7{Cannot meet original acceptance criteria?}
    T7 -- Yes --> Esc
    T7 -- No --> Cont[Continue in fast channel]
```

Summary of escalation triggers:

- Impact larger than expected
- Additional files need modification
- Backward compatibility risk
- User-visible copy or risk labels affected
- Data, permissions, configuration, secrets, or production systems touched
- Reviewer judgment required
- Cannot meet original acceptance criteria

When escalation triggers, stop fast channel execution, create a task packet, and restart in the standard channel.

## Human approval gates

Some decisions cannot be made by an agent alone. These require explicit human approval before proceeding.

```mermaid
flowchart TD
    Action[Action or decision] --> G1{Medium-high risk?}
    G1 -- Yes --> HA[Human approval required]
    G1 -- No --> G2{Publish, deploy, or production change?}
    G2 -- Yes --> HA
    G2 -- No --> G3{Delete, migrate, or rename key content?}
    G3 -- Yes --> HA
    G3 -- No --> G4{Modify security boundaries?}
    G4 -- Yes --> HA
    G4 -- No --> G5{Change public commitments, disclaimers, or risk labels?}
    G5 -- Yes --> HA
    G5 -- No --> G6{Handle sensitive data or external publishing?}
    G6 -- Yes --> HA
    G6 -- No --> G7{Agent cannot self-verify safety?}
    G7 -- Yes --> HA
    G7 -- No --> Proceed[Agent may proceed]
```

Summary of human approval gates:

- Medium or high risk action
- Publishing, deployment, or production changes
- Deletion, migration, or renaming of key content
- Modification of security boundaries
- Changes to public commitments, disclaimers, or risk labels
- Handling sensitive data or external publishing
- Agent cannot self-verify safety

Human approval is a merge blocker. A PR that passes review but lacks required human approval cannot be merged.

## Stop conditions

Stop and ask a human when:

- Scope is unclear or conflicting
- Required files or facts are missing
- The agent needs permission escalation
- The task involves secrets, production systems, or destructive actions
- Verification cannot be performed
- The task requires external publishing
- An escalation trigger fires but the agent cannot determine the new channel

## Channel summary

| Aspect | Fast channel | Standard channel |
|---|---|---|
| Risk level | Low | Medium to high |
| Scope | One or two files, no behavior change | Code logic, multiple files, behavior change |
| Review | Quick human check | Independent review with PR review packet |
| Acceptance | Visual confirmation | Acceptance report with evidence |
| Escalation | Automatic if triggers fire | N/A |
| Human approval | Not required unless risk appears | Required for merge on medium-high risk |

# CLI Tool Bugfix — Channel Escalation Example

A fictional CLI tool bugfix that starts as a low-risk fast-channel change, then gets escalated to standard channel when unexpected complexity is discovered during execution.

## What this example demonstrates

- Initial risk assessment and channel selection (fast channel for small bugfix)
- In-execution discovery of hidden complexity (config parsing, backward compatibility)
- Decision to stop fast channel and escalate to standard channel
- Re-running PR review and acceptance report under the escalated channel
- Documenting why the escalation happened and what changed

## Governance concepts covered

- **Channel selection**: fast vs standard
- **Risk reassessment**: what looks low-risk can become medium/high during execution
- **Stop-and-reassess**: agent's responsibility to halt when complexity exceeds initial estimate
- **Scope creep detection**: config parsing changes were not in the original scope
- **Traceability**: documenting why a channel change occurred

## Risk level

Initially low (small bugfix). Escalated to medium during execution due to config parsing impact and backward compatibility concerns.

## Channel decision

Started as fast channel → escalated to standard channel (requires PR review + acceptance report before merge).

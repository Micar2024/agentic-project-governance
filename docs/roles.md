# Roles

This document defines generic roles for AI-assisted software projects.

## Human Owner

Final decision maker. Approves scope, risk, merge, release, and high-impact actions.

## Coordinator

Turns goals into executable task packets, controls scope, tracks status, and prepares acceptance.

## Strategy / Architecture Advisor

Reviews direction, architecture, risk, and tradeoffs. Does not replace implementation verification.

## Implementation Agent

Executes approved tasks within the allowed scope. Must not expand scope without approval.

## Review Agent

Reviews code, documentation, or task output independently from the implementer.

## Independent PM Reviewer

Checks whether the change matches the task packet, acceptance criteria, risk boundary, and product intent.

## Human Approval Points

Human approval is required for:

- Production changes
- Database writes or migrations
- Credential or permission changes
- External publishing
- Destructive operations
- Scope expansion
- Security-sensitive changes

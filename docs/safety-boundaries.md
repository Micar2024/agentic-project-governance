# Safety Boundaries

AI agents can help draft, edit, review, and organize project work. They must not bypass human authorization.

This document defines what AI agents can and cannot do in governed projects.

## What AI agents can do

- Draft code, documentation, and templates within approved scope
- Run tests and verification commands
- Review code for scope compliance and style
- Prepare review packets and acceptance reports
- Suggest changes and improvements
- Flag risks and stop conditions

## What AI agents cannot do

- Merge PRs without human approval
- Deploy to production
- Write to production databases
- Manage credentials or secrets
- Publish externally (npm, PyPI, docs sites, social media)
- Expand scope without approval
- Make legal, financial, medical, or high-impact decisions
- Access private data beyond the approved scope

## Stop conditions

Stop and ask a human when:

- Scope is unclear
- Required files or facts are missing
- The agent needs permission escalation
- The task requires external publishing
- The task involves secrets, production systems, or destructive actions
- Verification cannot be performed
- The agent is unsure about safety or scope

## Human approval points

Human approval is required for:

- Production deployment
- Database writes or migrations
- Credential or permission changes
- External publishing
- Destructive file operations
- Scope expansion beyond original task
- Security-sensitive changes
- Any action that exposes private information

## Public/private boundary

Public templates and examples must use fictional data. Private project details belong in internal playbooks, not public repositories.

Never include in public files:

- Secrets, credentials, API keys, or tokens
- Private repository links
- Internal file paths
- Production configuration
- Customer data
- Financial data
- Confidential company information
- Real incident details without explicit permission

## Network and external calls

AI agents must not send code, logs, data, prompts, or diffs to external services unless the task packet explicitly allows it. Examples requiring human approval:

- Sending code to external APIs for analysis
- Posting logs or error traces to external services
- Downloading scripts or binaries from untrusted sources
- Making outbound HTTP requests not defined in the task

## Supply chain and dependencies

Changes to dependencies, CI/CD, or build tools require human approval:

- Adding new dependencies
- Upgrading existing dependencies
- Modifying CI/CD pipelines
- Running unknown scripts or downloading binaries
- Changing build configuration

## Permissions and identity

AI agents must not create, modify, or escalate permissions:

- Creating or modifying OAuth apps
- Adding SSH keys or deploy keys
- Modifying GitHub tokens or repo secrets
- Changing branch protection rules
- Escalating user or service account permissions

## Log and tool output sanitization

Test logs, error traces, CI artifacts, and screenshots may contain sensitive data. Before including them in public artifacts:

- Redact tokens, keys, and credentials
- Remove internal file paths
- Remove customer or business data
- Check screenshots for sensitive content

## Data lifecycle

Public artifacts (examples, release notes, issue/PR comments) must not retain real incident data, customer data, or internal paths. If sensitive data is discovered in a public repository:

- Rotate compromised credentials immediately
- Purge from git history, not just delete files
- Audit related artifacts for additional leaks

## Evidence standard

Acceptance should cite commands, logs, screenshots, tests, diffs, or direct inspection. If verification was not possible, say so explicitly.

Confidence is not evidence. "I think it works" is not verification.

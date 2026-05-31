# agentic-project-governance

A lightweight governance template for AI-assisted software projects.

Use this repository when your team uses coding agents, review agents, planning assistants, or multiple AI tools in the same project. It gives you task packets, PR review packets, acceptance reports, safety boundaries, and GitHub templates -- so AI-assisted work is governed, not improvised.

## Why this exists

AI-assisted software projects often fail for boring reasons:

- Tasks are described in chat but not packaged for execution
- AI agents change files outside the intended scope
- The source of truth is unclear
- Implementation agents review their own work
- PRs lack evidence and risk notes
- Acceptance is based on confidence instead of verification
- Private project context leaks into public documentation

This template treats AI-assisted work as a governed delivery process, not a casual conversation.

## What this is

- A generic role model for human and AI collaborators
- A standard workflow from task packet to acceptance report
- Templates for AI-agent tasks, PR review packets, and acceptance reports
- Safety boundaries for private data, credentials, production systems, and external publishing
- GitHub issue and pull request templates
- Minimal fictional examples

## What this is not

- Not a prompt collection
- Not a multi-agent runtime
- Not an AI coding tool
- Not a replacement for GitHub, GitLab, Linear, Jira, or human review
- Not a guarantee that AI-generated code is correct
- Not a production automation framework
- Not a place to store private project details, secrets, credentials, or business-sensitive information

## Who this is for

- Teams using coding agents, AI IDEs, review assistants, and planning tools
- Developers who want to move from "chat with AI" to "governed AI delivery"
- Maintainers who need clear task packets, review packets, and acceptance evidence
- Small teams that want safe defaults without heavy process

## How this differs

This project does not try to make agents smarter. It makes agent work reviewable.

- **Not a prompt collection.** Prompt collections optimize agent output. This project optimizes for human review of agent work.
- **Not an agent runtime.** Runtimes execute tasks. This project defines how tasks should be packaged, reviewed, and accepted before and after execution.
- **Not an enterprise compliance framework.** Enterprise frameworks enforce policy. This project provides lightweight templates that any team can adopt without process overhead.
- **Not a code quality tool.** Code quality tools check syntax and style. This project checks whether the right work was done, with the right evidence, and the right approvals.

The focus is on task packets, PR evidence, acceptance reports, stop conditions, and human approval gates -- the governance layer that sits between "ask an AI" and "ship to production."

## Core principles

```text
Task packet before execution.
GitHub as source of truth.
One PR, one problem.
Implementer does not review itself.
Review must be independent.
Acceptance needs evidence.
High-risk actions require human authorization.
Private facts stay private.
```

## Quick start

1. Copy `templates/task-packet.md` and define the task.
2. Confirm allowed scope, forbidden scope, verification commands, and rollback path.
3. Let an implementation agent work only within the approved scope.
4. Use `templates/pr-review-packet.md` for independent review.
5. Use `templates/acceptance-report.md` before closing the task.
6. Keep private or sensitive details out of public files.

## Minimal workflow

```mermaid
flowchart LR
  A[Human objective] --> B[Task packet]
  B --> C[Implementation agent]
  C --> D[Pull request]
  D --> E[Independent review]
  E --> F[Acceptance report]
  F --> G[Human decision]
```

## Workflow diagrams

For detailed channel selection, escalation triggers, and human approval gates, see [docs/workflow.md](docs/workflow.md).

## Template validation

For checklists to verify task packets, PR review packets, and acceptance reports, see [docs/template-validation-guide.md](docs/template-validation-guide.md).

## Repository layout

```text
docs/        Governance concepts and workflow documentation
templates/   Reusable task, review, acceptance, handoff, and risk templates
examples/    Fictional examples only
.github/     Issue and pull request templates
releases/    Release notes
```

## Templates

| Template | Purpose |
|---|---|
| `templates/task-packet.md` | Define an executable AI-agent task with scope, evidence, and rollback |
| `templates/pr-review-packet.md` | Independent review checklist for pull requests |
| `templates/acceptance-report.md` | Evidence-based task closure report |
| `templates/handoff.md` | Context handoff between agents or sessions |
| `templates/risk-register.md` | Track known risks and mitigations |
| `templates/project-current.md` | Current project status snapshot |
| `templates/task-list.md` | Task tracking table |
| `templates/sprint-plan.md` | Sprint planning template |
| `templates/postmortem.md` | Post-incident review |
| `templates/design-review.md` | Design review for UX, copy, safety, and interaction changes |

## Examples

See `examples/minimal-agentic-project/` for a fictional documentation website task that demonstrates the full workflow: task packet -> PR review -> acceptance report.

## Safety note

Do not put secrets, credentials, private repository links, production configuration, internal paths, customer data, financial data, or company-sensitive information in public examples or templates.

Use fictional examples unless you have explicit permission to publish real project details.

## Roadmap

### v0.2.0 (current)
- Additional fictional examples: channel escalation, human approval, multi-task aggregation
- Workflow diagrams for fast channel vs standard channel with Mermaid
- Template validation guide for task packets, PR review packets, and acceptance reports
- Design review template for UX, copy, safety, and interaction changes
- ASCII-only Markdown cleanup across all files

### v0.3.0 (planned)

Priority items for the next release:

- **P0**: GitHub Action for template validation (advisory checks on pull requests)
- **P0**: Task packet schema / validation configuration
- **P1**: Dependency audit template
- **P2**: Multi-language README translations

Community-contributed examples welcome at any time.

## Contributing

See `CONTRIBUTING.md`. We accept public, sanitized governance templates and fictional examples.

## Status

v0.2.0 -- usability and review workflow update. Not a runtime, not a production system, not an enterprise compliance framework, and not a production safety certification.

## License

MIT. See `LICENSE`.

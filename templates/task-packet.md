# Task Packet

A task packet is the single source of truth for an AI-agent task. It defines what to do, what not to do, how to verify, and when to stop.

If you cannot fill in a task packet, the task is not ready for an AI agent.

## 1. Objective

What should be achieved? Be specific. "Fix the bug" is not an objective. "Fix the login timeout on slow connections by increasing the retry window from 3s to 10s" is.

## 2. Background

What facts are already known? Include context the agent needs, but keep it minimal. Link to issues, docs, or prior decisions instead of pasting entire histories.

## 3. Allowed scope

Files, directories, modules, or actions the agent may touch. Be explicit. Examples:

- `src/auth/`
- `docs/api.md`
- `package.json` (dependency section only)

## 4. Forbidden scope

Files, systems, data, or actions the agent must not touch. Examples:

- No production database
- No credentials or secrets
- No files outside `src/auth/`
- No dependency upgrades

## 5. Inputs and references

Links, docs, issues, or prior decisions that inform this task. Examples:

- Issue #42
- `docs/api-spec.md`
- PR #38 review comments

## 6. Non-goals

What is explicitly out of scope, even if related? Examples:

- Performance optimization (not requested)
- Backward compatibility with v1 (not in scope)
- Refactoring unrelated modules

## 7. Expected deliverables

What files, artifacts, or outputs should the agent produce? Examples:

- Modified `src/auth/login.ts`
- New test file `src/auth/login.test.ts`
- Updated `docs/api.md`

## 8. Output format

How should the agent deliver results? Examples:

- Single PR with all changes
- Draft PR for review
- Files in workspace, no PR until approved

## 9. Acceptance criteria

What must be true for the task to be considered done? Use checkboxes.

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## 10. Verification plan

How will the result be verified? Include specific commands, tests, screenshots, diff inspection, or manual checks. Examples:

```bash
npm test
grep -r "oldFunction" src/auth/  # should return nothing
```

## 11. Rollback plan

How to revert safely if the change causes problems. Examples:

- `git revert <commit>`
- Restore backup file from `backups/`
- Run `npm run db:rollback`

## 12. Risk level

Low / Medium / High

- **Low**: Typo fix, documentation edit, formatting change.
- **Medium**: Code change in one module, new template, non-breaking refactor.
- **High**: Multi-module change, credential handling, database migration, external publishing, production-adjacent work.

## 13. Human confirmation required?

Yes / No. If yes, specify why.

Examples requiring human confirmation:

- Production deployment
- Database writes or migrations
- Credential or permission changes
- External publishing
- Destructive file operations
- Scope expansion beyond original task

## 14. Stop / escalation conditions

When should the agent stop and escalate to a human? Examples:

- Scope becomes unclear
- Required files or facts are missing
- Agent needs permission escalation
- Task involves secrets, production systems, or destructive actions
- Verification cannot be performed
- Agent is unsure about safety or scope

## 15. Agent self-check before execution

Before starting, the agent should confirm:

- [ ] I understand the objective.
- [ ] I know the allowed scope.
- [ ] I know the forbidden scope.
- [ ] I can verify the result.
- [ ] I know when to stop and ask.

If any answer is no, stop and ask the human.

# Example Task Packet

## Objective

Add a FAQ section to the fictional documentation website at `docs-site.example.com`.

## Background

The documentation site currently has a Getting Started guide and an API Reference, but no FAQ. Users frequently ask the same questions in GitHub issues. A FAQ page will reduce duplicate issues and help users self-serve.

## Allowed scope

- `docs/faq.md` (new file)
- `README.md` (add link to FAQ in the navigation section)

## Forbidden scope

- No changes to `src/` (application code)
- No changes to deployment configuration
- No changes to credentials or secrets
- No dependency changes

## Inputs and references

- Issue #42: "Add FAQ page"
- `docs/getting-started.md` (for style reference)
- Common questions from issues #38, #39, #41

## Acceptance criteria

- [ ] `docs/faq.md` exists with at least 5 questions and answers
- [ ] `README.md` links to `docs/faq.md` in the navigation section
- [ ] No broken links (manual or automated check)
- [ ] FAQ follows the same formatting style as `docs/getting-started.md`

## Verification plan

```bash
# Check file exists
ls -la docs/faq.md

# Check README links to FAQ
grep -q "faq.md" README.md

# Check FAQ has content (at least 5 questions)
grep -c "^##" docs/faq.md  # should be >= 5
```

## Rollback plan

- Delete `docs/faq.md`
- Remove the FAQ link from `README.md`

## Risk level

Low

## Human confirmation required?

No. Documentation-only change with no production impact.

## Agent self-check before execution

- [x] I understand the objective.
- [x] I know the allowed scope.
- [x] I know the forbidden scope.
- [x] I can verify the result.
- [x] I know when to stop and ask.

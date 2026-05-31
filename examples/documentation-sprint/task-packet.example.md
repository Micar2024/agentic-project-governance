# Task Packet — Documentation Sprint (Multi-Task)

## Objective

Execute a documentation sprint for the fictional "OpenMeter" API platform. The sprint covers three independent documentation tasks, each with its own scope and acceptance criteria. Tasks can be executed in parallel; there are no cross-task dependencies.

## Sprint overview

| Task ID | Description | Files | Risk | Channel |
|---|---|---|---|---|
| DOC-1 | API rate limiting guide | `docs/guides/rate-limiting.md` (new) | Low | Fast |
| DOC-2 | Update authentication docs for v2 | `docs/reference/auth.md` (update) | Low | Fast |
| DOC-3 | Add changelog for v2.1.0 | `CHANGELOG.md` (update) | Low | Fast |

## DOC-1: API Rate Limiting Guide

### Objective

Create a new guide documenting the rate limiting behavior of the OpenMeter API, including headers, limits, and best practices for handling 429 responses.

### Allowed scope

- `docs/guides/rate-limiting.md` (new file)
- `docs/guides/README.md` (add link to new guide)

### Forbidden scope

- No changes to `src/` (application code)
- No changes to rate limiting implementation
- No changes to API documentation endpoints

### Acceptance criteria

- [ ] `docs/guides/rate-limiting.md` exists with sections: Overview, Rate Limit Headers, Handling 429 Responses, Best Practices, Examples
- [ ] Includes at least 3 code examples in different languages (e.g., Python, JavaScript, Go)
- [ ] `docs/guides/README.md` links to the new guide
- [ ] No broken internal links

### Verification

```bash
ls -la docs/guides/rate-limiting.md
grep -c "^##" docs/guides/rate-limiting.md  # should be >= 5 sections
grep -q "rate-limiting.md" docs/guides/README.md
```

## DOC-2: Update Authentication Docs for v2

### Objective

Update the authentication reference to document the new API key scopes and service account tokens introduced in API v2.

### Allowed scope

- `docs/reference/auth.md` (update existing)
- `docs/reference/auth-v1-v2-migration.md` (new file — migration guide)

### Forbidden scope

- No changes to authentication implementation
- No changes to API key generation or validation
- No changes to other reference docs

### Acceptance criteria

- [ ] `auth.md` documents the new v2 scopes: `read:usage`, `write:config`, `admin:keys`
- [ ] `auth.md` documents service account tokens (usage, creation, rotation)
- [ ] `auth-v1-v2-migration.md` provides step-by-step migration from v1 API keys to v2
- [ ] No v1 authentication documentation is removed (v1 is still supported)

### Verification

```bash
grep -q "read:usage" docs/reference/auth.md
grep -q "service account" docs/reference/auth.md
grep -q "migration" docs/reference/auth-v1-v2-migration.md
```

## DOC-3: Add Changelog for v2.1.0

### Objective

Add the changelog entry for OpenMeter API v2.1.0 release, following the Keep a Changelog format.

### Allowed scope

- `CHANGELOG.md` (add v2.1.0 section at top)

### Forbidden scope

- No changes to existing changelog entries
- No changes to release process or version numbers
- No changes to any other file

### Acceptance criteria

- [ ] v2.1.0 section added at top of `CHANGELOG.md`
- [ ] Follows Keep a Changelog format with sections: Added, Changed, Fixed, Deprecated
- [ ] Each entry references the relevant issue or PR number
- [ ] Date is set to the release date (2026-05-30)

### Verification

```bash
grep -A1 "^## " CHANGELOG.md | head -2  # v2.1.0 should be first
grep -c "### " CHANGELOG.md              # should have Added/Changed/Fixed/Deprecated
```

## Overall risk level

Low. All tasks are documentation-only with no production impact.

## Overall channel

Individual tasks: **Fast channel** (docs-only, low risk).
Consolidated release: **Standard channel** (aggregate review before shipping).

## Human confirmation required?

Not required for individual docs tasks. Required for the consolidated release acceptance.

## Agent self-check before execution (for each task)

- [x] I understand this task's objective.
- [x] I know this task's allowed scope.
- [x] I know this task's forbidden scope.
- [x] I can verify this task's result independently.
- [x] I know when to stop and ask.

---

## Sprint Execution Log (added during work)

### DOC-1 — COMPLETED

- Created `docs/guides/rate-limiting.md` with 6 sections: Overview, Rate Limit Headers, Handling 429 Responses, Best Practices, Examples, FAQ
- Included code examples in Python (exponential backoff), JavaScript (axios interceptor), and Go (retry middleware)
- Updated `docs/guides/README.md` with link to new guide
- Verification: 6 sections, 3 code examples, link present ✓

### DOC-2 — COMPLETED

- Updated `docs/reference/auth.md` with v2 scope documentation and service account token section
- Created `docs/reference/auth-v1-v2-migration.md` with 5-step migration guide
- Preserved all v1 authentication documentation (v1 is still supported)
- Verification: v2 scopes documented, service account section present, migration guide complete ✓

### DOC-3 — COMPLETED

- Added v2.1.0 section at top of `CHANGELOG.md`
- 4 subsections: Added (3 entries), Changed (2 entries), Fixed (4 entries), Deprecated (1 entry)
- All entries reference issue/PR numbers (#201–#210)
- Date: 2026-05-30
- Verification: v2.1.0 is first entry, 4 subsection categories present ✓

### Sprint summary

All 3 tasks completed independently. Each passes its own verification. Ready for consolidated acceptance review.

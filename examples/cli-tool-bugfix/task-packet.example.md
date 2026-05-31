# Task Packet -- CLI Tool Bugfix

## Objective

Fix a bug in the fictional `devkit` CLI tool where the `--config` flag ignores nested YAML keys when the value contains a colon character.

## Background

A user reported (Issue #127) that `devkit build --config ci/settings.yaml` fails silently when the YAML file contains values with colons (e.g., `timeout: "10:00:00"`). The root cause is believed to be a naive string-split on `:` in the config parser that doesn't account for quoted values. The fix is expected to be a one-line change in the parsing logic.

## Allowed scope

- `internal/config/parser.go` -- fix the colon-split bug in `parseLine()`
- `internal/config/parser_test.go` -- add test cases for colon-containing values

## Forbidden scope

- No changes to `cmd/` (CLI entry points)
- No changes to `internal/config/schema.go` (config schema definition)
- No changes to `go.mod` or `go.sum`
- No changes to documentation
- No changes to CI/CD configuration

## Inputs and references

- Issue #127: "devkit build --config fails with colon in YAML values"
- `internal/config/parser.go` -- the file containing the bug
- Test fixture: `testdata/config-with-colons.yaml`

## Acceptance criteria

- [ ] `parseLine()` correctly handles quoted values containing colons
- [ ] New test cases cover: quoted colons, unquoted colons, escaped colons
- [ ] Existing tests still pass (`go test ./internal/config/...`)
- [ ] No changes to the public API of the config package

## Verification plan

```bash
# Run config package tests
go test ./internal/config/... -v

# Run full test suite to check for regressions
go test ./... -count=1

# Manual verification: test the reported failing case
go run ./cmd/devkit build --config testdata/config-with-colons.yaml
```

## Rollback plan

- `git revert <commit>` -- single commit, clean revert

## Risk level

Low. Isolated fix in one function, covered by existing test suite.

## Channel

**Fast channel** -- small, isolated bugfix with clear reproduction and test coverage.

## Human confirmation required?

No. Bugfix with clear reproduction steps and test coverage.

## Agent self-check before execution

- [x] I understand the objective.
- [x] I know the allowed scope.
- [x] I know the forbidden scope.
- [x] I can verify the result.
- [x] I know when to stop and ask.

---

## Execution Log (added during work)

### Phase 1 -- Initial investigation

Started working on `parseLine()` in `internal/config/parser.go`. The colon-split bug is on line 142:

```go
parts := strings.SplitN(line, ":", 2)
```

This is indeed naive and doesn't handle quoted values.

### Phase 2 -- Discovery

While writing the fix, I discovered:

1. **`parseLine()` is called from `parseBlock()` which is also used by `internal/config/schema.go`** -- changing the parsing logic could affect schema validation.
2. **Three other CLI subcommands** (`devkit validate`, `devkit export`, `devkit init`) also pass through this code path -- the impact is wider than initially assessed.
3. **A workaround exists in user configs**: some users have been double-quoting colons as a workaround (e.g., `timeout: "\"10:00:00\""`). A naive fix would break these workarounds -- a **backward compatibility** concern.

### Phase 3 -- Decision: ESCALATE

**The initial assessment was wrong.** This is not a one-line fix. It requires:

- A proper YAML-aware parser change (not just a string-split tweak)
- Backward compatibility for existing workarounds
- Coordination with the schema validation code path
- Testing across multiple subcommands

**Channel escalated from fast -> standard.**

Reason: scope expanded beyond the initial estimate, backward compatibility is at risk, and the change touches a code path shared by schema validation.

### Phase 4 -- Revised scope (under standard channel)

- `internal/config/parser.go` -- replace naive split with a proper quoted-string-aware parser
- `internal/config/parser_test.go` -- comprehensive test cases including backward compat
- `internal/config/schema.go` -- verify schema validation isn't affected (may need minor adjustment)
- `internal/config/schema_test.go` -- add regression test for schema validation path

Human confirmation is now requested before proceeding with the revised scope.

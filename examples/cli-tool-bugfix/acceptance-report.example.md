# Acceptance Report -- CLI Tool Bugfix (Escalated)

## Requested task

Fix the `--config` flag bug in `devkit` CLI where YAML values containing colons were silently corrupted. Issue #127. Task packet: `task-packet.example.md`.

Originally assessed as low-risk (fast channel). Escalated to medium-risk (standard channel) during execution when cross-cutting impact on schema validation and backward compatibility was discovered.

## Completed work

- Replaced naive `strings.SplitN(line, ":", 2)` in `parseLine()` with a quoted-string-aware parser that correctly handles quoted values, nested quotes, and escape sequences.
- Added 12 test cases covering: quoted colons, unquoted colons, escaped colons, backward compat workarounds (double-quoted values), and edge cases (empty values, unbalanced quotes).
- Adjusted `validateConfigBlock()` in `internal/config/schema.go` to use the updated parser return type -- a minor signature change.
- Added regression test for schema validation path with colon-containing values.

## Not changed

- `cmd/` -- no CLI entry point changes
- `go.mod` / `go.sum` -- no dependency changes
- Documentation -- separate task recommended, not in this scope
- CI/CD configuration -- not touched

## Channel escalation summary

| Phase | Channel | Risk | Trigger |
|---|---|---|---|
| Initial assessment | Fast | Low | One-line fix expected |
| During execution | **STOP** | -- | Discovered schema validation coupling |
| Revised assessment | Standard | Medium | Cross-cutting impact + backward compat |
| Final delivery | Standard | Medium | All standard channel gates passed |

The escalation decision was documented in the task packet execution log before any code was written under the revised scope. Human confirmation was obtained before proceeding.

## Verification evidence

```bash
$ go test ./internal/config/... -v
# All 18 tests pass (6 existing + 12 new)
PASS
ok      devkit/internal/config    0.234s

$ go test ./... -count=1
# Full suite passes, no regressions
PASS (all packages)

$ go run ./cmd/devkit build --config testdata/config-with-colons.yaml
Build completed successfully. Config loaded: 12 keys.
```

Manual verification across all affected subcommands:

| Subcommand | Result |
|---|---|
| `devkit build` | Config with colons now loads correctly |
| `devkit validate` | No regression in schema validation |
| `devkit export` | Output unchanged from baseline |
| `devkit init` | No impact |

Backward compatibility verified: existing configs using the double-quote workaround produce a deprecation warning but continue to parse correctly.

## Result

**PASS**

All acceptance criteria met. All tests pass. Backward compatibility maintained with deprecation path.

## Residual risks

- Windows path separator (`\`) interaction with quote escaping -- not tested on Windows.
- CI pipelines that treat stderr output as errors may be affected by the new deprecation warning. Mitigation: the warning is printed only when the old workaround pattern is detected, which is rare.
- Non-UTF-8 config files -- not tested. The existing test suite only covers UTF-8.

## Rollback path

- `git revert <commit>` -- single commit reverts all changes
- Alternatively, revert just the parser change and keep the test additions (tests would fail, flagging the unreverted bug)

## Follow-up tasks

- [ ] Test on Windows (tracked in Issue #135)
- [ ] Add documentation for the new parser behavior (tracked in Issue #136)
- [ ] Remove backward compat workaround support in v2.0 (tracked in Issue #137)
- [ ] Add fuzz testing for the config parser (tracked in Issue #138)

## Human approval status

Approved. Escalation from fast to standard channel was reviewed and accepted. Standard channel PR review completed. Ready for merge.

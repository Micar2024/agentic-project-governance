# PR Review Packet — CLI Tool Bugfix (Escalated)

## Summary

Fixes the `--config` flag bug in `devkit` where YAML values containing colons were silently corrupted. The fix replaces a naive `strings.SplitN` approach with a quoted-string-aware parser. Originally scoped as a one-line fast-channel fix, but escalated to standard channel after discovering cross-cutting impact on schema validation and backward compatibility concerns.

## Linked task packet

`task-packet.example.md` — includes the escalation log documenting why the channel changed from fast to standard.

## Channel escalation record

| Field | Before | After |
|---|---|---|
| Channel | Fast | Standard |
| Risk level | Low | Medium |
| Reason | One-line fix expected | Discovered cross-cutting impact on schema validation + backward compat concerns |
| Files in scope | 2 | 4 |
| Human confirmation | Not required | Required |

## Files changed

| File | Reason |
|---|---|
| `internal/config/parser.go` | Replaced naive colon-split with quoted-string-aware parser in `parseLine()` |
| `internal/config/parser_test.go` | Added 12 test cases: quoted colons, nested quotes, backward compat workarounds |
| `internal/config/schema.go` | Adjusted `validateConfigBlock()` to use the updated parser return type |
| `internal/config/schema_test.go` | Added regression test for schema validation with colon-containing values |

## Scope compliance

- Within allowed (revised) scope: Yes
- Touched forbidden scope: No
- Original fast-channel scope was respected until escalation decision was documented

## Verification evidence

```bash
$ go test ./internal/config/... -v
=== RUN   TestParseLine_QuotedColons
--- PASS: TestParseLine_QuotedColons (0.00s)
=== RUN   TestParseLine_BackwardCompat
--- PASS: TestParseLine_BackwardCompat (0.00s)
=== RUN   TestParseLine_UnquotedColons
--- PASS: TestParseLine_UnquotedColons (0.00s)
=== RUN   TestSchemaValidation_ColonValues
--- PASS: TestSchemaValidation_ColonValues (0.00s)
...
PASS
ok      devkit/internal/config    0.234s

$ go test ./... -count=1
ok      devkit/cmd                 0.451s
ok      devkit/internal/config     0.238s
ok      devkit/internal/builder    0.312s
...
PASS (all tests pass, no regressions)

$ go run ./cmd/devkit build --config testdata/config-with-colons.yaml
Build completed successfully. Config loaded: 12 keys.
```

Manual verification:

- [x] Tested `devkit build` with the reported failing config — now works correctly
- [x] Tested `devkit validate` with colon-containing configs — no regression
- [x] Tested `devkit export` — output unchanged
- [x] Tested `devkit init` — no impact
- [x] Verified backward compat: existing configs with double-quote workarounds still parse correctly

## Known risks

- Users who relied on the buggy behavior (unquoted colons splitting at the wrong position) may have configs that now parse differently. Mitigation: the backward compat test cases cover the known workaround patterns. A deprecation warning is emitted when the old workaround pattern is detected.
- The new parser is slightly stricter about quote pairing — unbalanced quotes now produce a clear error instead of silent corruption. This is intentional and documented.

## Out-of-scope items

- `cmd/` — no CLI entry point changes
- `go.mod` / `go.sum` — no dependency changes
- Documentation — not in scope (separate task recommended)
- CI/CD configuration — not touched

## Reviewer checklist

- [x] Change matches revised task packet
- [x] No forbidden files touched
- [x] Evidence is sufficient
- [x] Risks are stated
- [x] No secrets or private data included
- [x] No unnecessary scope expansion
- [x] Channel escalation is documented and justified

## What this packet does not prove

This packet does not prove that all possible YAML edge cases are handled, that the fix works on Windows (not tested), or that the deprecation warning won't break CI pipelines that parse stderr. These are tracked as follow-up items.

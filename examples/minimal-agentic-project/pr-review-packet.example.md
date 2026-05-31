# Example PR Review Packet

## Summary

Adds a FAQ page (`docs/faq.md`) with 6 common questions and links it from README.

## Linked task packet

Closes #42. Implements task packet `task-packet.example.md`.

## Files changed

| File | Reason |
|---|---|
| `docs/faq.md` | New file: FAQ with 6 questions and answers |
| `README.md` | Added FAQ link in navigation section |

## Scope compliance

- Within allowed scope: Yes
- Touched forbidden scope: No

## Verification evidence

```bash
$ ls -la docs/faq.md
-rw-r--r-- 1 user staff 1234 May 31 12:00 docs/faq.md

$ grep -q "faq.md" README.md
(exit 0)

$ grep -c "^##" docs/faq.md
6
```

Manual inspection: FAQ content follows the same style as `docs/getting-started.md`. No broken links.

## Known risks

- FAQ is English-only; translations not in scope.
- Some answers may need updating as the API evolves.

## Out-of-scope items

- Application code (`src/`) not touched.
- Deployment configuration not touched.
- Dependencies not changed.

## Reviewer checklist

- [x] Change matches task packet
- [x] No forbidden files touched
- [x] Evidence is sufficient
- [x] Risks are stated
- [x] No secrets or private data included
- [x] No unnecessary scope expansion

## What this packet does not prove

This packet does not prove the FAQ content is factually correct, that the site will render it correctly in production, or that translations will not be needed. It documents that the files were created as requested and verified.

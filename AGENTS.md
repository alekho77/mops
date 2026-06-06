# Agent Instructions

MOPS uses a single memory layer for project continuity:

- `README.md` defines the public project scope.
- `docs/adr/` stores accepted architectural and product decisions.
- `docs/architecture/` stores stable technical architecture notes.
- `memory-bank/` stores working context for current and future agents.

## Required reading order

1. `README.md`
2. `docs/adr/`
3. `docs/architecture/`
4. `memory-bank/activeContext.md`
5. `memory-bank/systemPatterns.md`
6. `memory-bank/techContext.md`

## Project boundaries

MOPS is an individual, local-first, privacy-first personal AI system.

It is not a team platform, enterprise knowledge base, collaborative workspace, or mandatory cloud backend.

## Documentation rules

- Capture accepted decisions as ADRs.
- Keep architecture documents technical and concise.
- Keep `memory-bank/activeContext.md` focused on recent decisions.
- Do not replace source files with vector memory.
- Treat vector indexes as rebuildable local caches.
- Treat semantic DB and memory metadata as the durable source of truth.

## Source file headers

Every new or modified source file must start with a language-native comment header:

```text
Copyright 2026 Aleksei Khozin (https://github.com/alekho77)
License: PolyForm Noncommercial License 1.0.0. See LICENSE.md.
```

Rules:

- Use the comment syntax of the target language or file format.
- If a file has a shebang, keep the shebang as the first line and place the header immediately after it.
- Do not add this header to generated files, vendored files, lockfiles, binary assets, or documentation files.
- Preserve existing external copyright notices.

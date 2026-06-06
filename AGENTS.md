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

# ADR-0011: Release maturity and desktop boundary

## Status: Accepted

## Supersedes

This ADR supersedes:

- the release-maturity interpretation of ADR-0010;
- the desktop-scope portions of ADR-0002.

ADR-0010 remains accepted for the private pre-1.0 mobile feature sequence. ADR-0002 remains historical context for an older mobile/desktop split, but its desktop-owned semantic engine is not part of the current `v0.x` or `v1.x` product line.

## Context

The `v0.1` through `v0.9` release train is useful for phone-testable increments, but those builds are not the public MVP. Treating them as MVP releases creates a contradiction with the mobile-only UX boundary and with older documents that described desktop as owner of the embedding pipeline, vector index, reindexing, and semantic workbench.

MOPS needs an explicit version maturity ladder:

- pre-1.0 builds validate the mobile product privately;
- v1.x is the public mobile-only product line;
- desktop begins only after the mobile product baseline is stable.

## Decision

MOPS uses this version contract:

| Version family | Maturity | Product boundary |
| --- | --- | --- |
| `v0.x` | Private pre-1.0 alpha/beta builds | Mobile-only phone-test builds. Not MVP. Not public releases. |
| `v1.0` | First public MVP/product baseline | Mobile-only public release. No desktop app and no desktop processing dependency. |
| `v1.x` | Public mobile iteration line | Mobile-only improvements over v1.0. No desktop app and no desktop processing dependency. |
| `v2.0+` | Desktop expansion line | First allowed version family for desktop app, desktop semantic engine, heavier embedding/index/reindex tooling, and cross-device workflow decisions. |

`v0.x` builds must be described as private alpha/beta builds or pre-1.0 milestones, not as MVP releases.

Desktop must not own or be required for:

- `v0.x` private mobile alpha/beta builds;
- `v1.0` public mobile MVP/product baseline;
- any `v1.x` public mobile iteration.

Desktop-owned embedding pipelines, vector indexes, reindexing tools, semantic workbench workflows, and cross-device processing flows are allowed only in `v2.0+` planning.

## Consequences

Positive:

- The mobile alpha/beta train can stay small and private without claiming MVP maturity.
- The first public MVP has a clean mobile-only boundary.
- Historical desktop-engine ideas remain available for v2.0+ without creating v0.x/v1.x dependency.

Trade-offs:

- The desktop architecture is intentionally not detailed yet.
- Heavy processing and desktop debugging workflows remain deferred until v2.0+.
- v1.x must solve its public mobile product baseline without relying on desktop ownership.

Obligations:

- Do not call any `v0.x` release an MVP.
- Do not assign desktop ownership of embeddings, vector indexes, reindexing, or semantic workbench behavior before v2.0.
- Keep README, architecture notes, and memory-bank files aligned on the same version ladder.

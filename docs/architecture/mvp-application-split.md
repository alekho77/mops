# Application Version Split

## Version ladder

MOPS separates private mobile validation builds from the public mobile product and from future desktop expansion.

| Version family | Maturity | Product boundary |
| --- | --- | --- |
| `v0.x` | Private pre-1.0 alpha/beta builds | Mobile-only phone-test builds. Not MVP. Not public releases. |
| `v1.0` | First public MVP/product baseline | Mobile-only public release. No desktop app or desktop processing dependency. |
| `v1.x` | Public mobile iteration line | Mobile-only improvements over v1.0. No desktop app or desktop processing dependency. |
| `v2.0+` | Desktop expansion line | First allowed family for desktop app, desktop semantic engine, heavier embedding/index/reindex tooling, and cross-device workflows. |

## Private pre-1.0 mobile train

Each `v0.x` build must be buildable as an Android APK or iOS/Xcode build and must have a manual phone acceptance scenario.

| Version | User-facing capability | First required objects |
| --- | --- | --- |
| v0.1 Mobile Capture + Inbox | Capture raw sketches locally and manage Inbox. | `ActiveSketchBuffer`, `Sketch` |
| v0.2 Semantic Outbox | Process sketches into vectorized Outbox records and inspect similar sketches. | `SemanticSketch`, `EmbeddingRecord` |
| v0.3 Semantic Links | Review, confirm, reject, and persist semantic links. | `SemanticLink`, `CorrectionEvent` |
| v0.4 Bundles | Group linked sketches and edit Bundle membership. | `Bundle`, `BundleSuggestion` |
| v0.5 Drafts | Generate and edit Drafts from Bundles. | `Draft` |
| v0.6 Knowledge Base | Persist, embed, search, and open KnowledgeItems. | `KnowledgeItem`, `SearchResult` |
| v0.7 KnowledgeAreas | Assign KnowledgeItems to long-term areas. | `KnowledgeArea`, `KnowledgeAreaAssignment` |
| v0.8 2D Semantic Map | Visualize semantic records, links, Bundles, and areas. | map projection/view state |

## Target semantic pipeline

ADR-0005 remains the target semantic model:

```text
Sketch
  -> Embedding Pipeline
  -> SemanticSketch
  -> Semantic Links
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> Vector DB
  -> KnowledgeArea
  -> Semantic Map
```

This pipeline is not the v0.1 scope and does not make `v0.x` an MVP. It is validated incrementally across the private pre-1.0 mobile train in ADR-0010.

Core model:

```text
Inbox = Sketch input buffer
Outbox = SemanticSketch workbench
Vector DB = long-term KnowledgeItem memory
```

## Product split

```text
Mobile app = full user-facing app for v0.x and v1.x
Desktop app = v2.0+ expansion only
Shared core = common domain and processing contracts
```

`v0.x` and `v1.x` are mobile-only. Desktop remains a future `v2.0+` processing surface and must not be required for private pre-1.0 builds or the public v1.x mobile product.

## Mobile v0.1 responsibilities

- No account.
- No registration.
- Local device storage only.
- Start on Sketch Editor.
- Autosave all input.
- Persist `ActiveSketchBuffer` independently from Inbox records.
- Create `Sketch` records only after an explicit Send to Inbox action.
- Send to Inbox without confirmation and provide Undo.
- Support Sketch create, list, edit, delete, and status tracking.
- Provide Inbox and Settings screens.
- Confirm destructive, reset, and batch capture/Inbox actions.
- Build and install on Android or iOS for phone testing.

v0.1 does not include Outbox, embeddings, cosine similarity, semantic links, graph persistence, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, semantic search, or Semantic Map.

## Later private mobile responsibilities

- v0.2 adds local embedding processing, embedding metadata, similar-sketch lookup, and Outbox list/detail views.
- v0.3 adds semantic link suggestions, confirmation/rejection, correction events, and basic graph persistence.
- v0.4 adds Bundle creation, inspection, merge/split, and manual assignment.
- v0.5 adds Draft generation, editing, and confirmation as a KnowledgeItem candidate.
- v0.6 adds KnowledgeItem persistence, embedding, semantic search, and basic Knowledge Base list/detail views.
- v0.7 adds KnowledgeArea creation, assignment, rename/delete, suggestions, and correction events.
- v0.8 adds a phone-testable 2D Semantic Map.

## Desktop v2.0+ boundary

Desktop is not part of `v0.x`, `v1.0`, or any `v1.x` release. It must not own or be required for embeddings, vector indexes, reindexing, or semantic workbench behavior before `v2.0`.

Candidate `v2.0+` desktop responsibilities:

- batch inbox review;
- heavy transcription and reprocessing;
- cleaned note generation;
- embedding pipeline maintenance;
- vector index rebuilds;
- clustering and Bundle diagnostics;
- KnowledgeArea and project extraction diagnostics;
- model migration and reindexing tools.

These responsibilities are placeholders for v2.0+ planning, not private pre-1.0 or v1.x scope.

## Shared core packages

Planned shared packages:

```text
packages/core
packages/db
packages/ingestion
packages/embeddings
packages/clustering
packages/search
packages/sync
packages/shared-types
```

For v0.1, only the shared contracts needed by `ActiveSketchBuffer`, `Sketch`, local storage, Send to Inbox Undo, destructive-action confirmations, and Settings are required. Later packages can be introduced when their release milestone requires them.

## Monorepo boundary

The main product monorepo can contain:

- mobile app;
- v2.0+ desktop app placeholder or future app;
- shared product core;
- semantic DB schema;
- ingestion contracts;
- embedding contracts;
- clustering logic;
- search logic;
- sync contracts;
- architecture docs;
- memory-bank files.

Separate repositories are reserved for future independent engines:

- custom vector database fork;
- custom embedding model fork;
- speech-to-text model fork;
- OCR/vision model fork;
- benchmarks and datasets;
- native runtime experiments.

## Exclusions from v0.1

Do not include in the first private mobile build:

- embeddings or vector search;
- Outbox;
- graph persistence;
- Bundles;
- Draft generation;
- KnowledgeItems;
- KnowledgeAreas;
- Semantic Map;
- photo analysis;
- document ingestion;
- web URL scraping;
- browser extension;
- GitHub ingestion;
- custom vector DB fork;
- custom model training;
- device sync;
- collaboration;
- automatic file import;
- full iOS + Android + macOS + Windows + Linux launch at once.

Do not include in `v0.x` or `v1.x`:

- desktop app;
- desktop-owned embedding pipeline;
- desktop-owned vector index;
- desktop-owned reindexing workflow;
- desktop-owned semantic workbench;
- cross-device workflow dependency.

## Design rule

Automatic classification must work as suggestion, not silent truth.

```text
system suggests
user confirms/rejects/corrects
correction is stored as training signal
```

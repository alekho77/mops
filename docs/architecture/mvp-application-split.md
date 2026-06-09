# MVP Application Split

## Release train

MOPS uses an incremental mobile release train. Each `v0.x` release must be buildable as an Android APK or iOS/Xcode build and must have a manual phone acceptance scenario.

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

This pipeline is not the v0.1 scope. It is delivered across the release train in ADR-0010.

Core model:

```text
Inbox = Sketch input buffer
Outbox = SemanticSketch workbench
Vector DB = long-term KnowledgeItem memory
```

## Product split

```text
Mobile app = full user-facing app for the current v0.x milestone
Desktop app = deferred semantic memory engine
Shared core = common domain and processing contracts
```

v0.1 is mobile-only and local-only. Desktop remains a future processing surface and is not part of the first user-facing release.

## Mobile v0.1 responsibilities

- No account.
- No registration.
- Local device storage only.
- Start on Sketch Editor.
- Autosave all input.
- Persist `ActiveSketchBuffer` independently from Inbox records.
- Create `Sketch` records only after explicit confirmation.
- Support Sketch create, list, edit, delete, and status tracking.
- Provide Inbox and Settings screens.
- Confirm destructive capture and Inbox actions.
- Build and install on Android or iOS for phone testing.

v0.1 does not include Outbox, embeddings, cosine similarity, semantic links, graph persistence, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, semantic search, or Semantic Map.

## Later mobile responsibilities

- v0.2 adds local embedding processing, embedding metadata, similar-sketch lookup, and Outbox list/detail views.
- v0.3 adds semantic link suggestions, confirmation/rejection, correction events, and basic graph persistence.
- v0.4 adds Bundle creation, inspection, merge/split, and manual assignment.
- v0.5 adds Draft generation, editing, and confirmation as a KnowledgeItem candidate.
- v0.6 adds KnowledgeItem persistence, embedding, semantic search, and basic Knowledge Base list/detail views.
- v0.7 adds KnowledgeArea creation, assignment, rename/delete, suggestions, and correction events.
- v0.8 adds a phone-testable 2D Semantic Map.

## Desktop responsibilities

Desktop is deferred from v0.1. Later, it may own heavier processing and debugging workflows:

- batch inbox review;
- heavy transcription and reprocessing;
- cleaned note generation;
- embedding pipeline maintenance;
- vector index rebuilds;
- clustering and Bundle diagnostics;
- KnowledgeArea and project extraction diagnostics;
- model migration and reindexing tools.

Desktop must not become a required backend for v0.1 mobile capture.

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

For v0.1, only the shared contracts needed by `ActiveSketchBuffer`, `Sketch`, local storage, confirmations, and Settings are required. Later packages can be introduced when their release milestone requires them.

## Monorepo boundary

The main product monorepo contains:

- mobile app;
- desktop app placeholder or future app;
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

Do not include in the first mobile release:

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

## Design rule

Automatic classification must work as suggestion, not silent truth.

```text
system suggests
user confirms/rejects/corrects
correction is stored as training signal
```

# Tech Context

## Confirmed technical direction

- Local-first architecture.
- Mobile-first capture interface.
- Offline-first personal memory.
- SQLite-backed local persistence remains the default candidate.
- Semantic DB and memory metadata are durable source of truth.
- Vector indexes are rebuildable local caches.
- Original files remain outside vector DB by default.
- Explicit model and embedding versioning is required once embeddings are introduced.
- Model Layer remains separated from Vector DB Layer.
- Mobile MVP uses Flutter and Dart.
- `apps/mobile` targets iOS and Android from one shared Flutter codebase.
- Rust is not part of the first mobile MVP by default.
- Desktop app is deferred from v0.1.
- Product code should live in one monorepo.
- Independent low-level engine forks should live in separate repositories.

## Mobile release train

Each `v0.x` release must be buildable as Android APK or iOS/Xcode build and testable on a real phone.

| Version | Milestone | Technical boundary |
| --- | --- | --- |
| v0.1 | Mobile Capture + Inbox | Flutter app shell, local storage, `ActiveSketchBuffer`, `Sketch`, Inbox, Settings, confirmations. |
| v0.2 | Semantic Outbox | Embedding provider contract, embedding metadata, `SemanticSketch`, cosine similarity, Outbox list/detail. |
| v0.3 | Semantic Links | `SemanticLink`, confirmed/rejected link persistence, correction events, basic graph persistence. |
| v0.4 | Bundles | Bundle builder, Bundle persistence, merge/split/manual assignment. |
| v0.5 | Drafts | Draft generation contract, editable Draft persistence, KnowledgeItem candidate confirmation. |
| v0.6 | Knowledge Base | KnowledgeItem persistence, KnowledgeItem embeddings/search, Knowledge Base list/detail/search. |
| v0.7 | KnowledgeAreas | KnowledgeArea persistence, assignment suggestions, manual correction events. |
| v0.8 | 2D Semantic Map | Phone map projection/view state over semantic records, links, Bundles, items, and areas. |

## v0.1 scope

Required v0.1 object chain:

```text
ActiveSketchBuffer
  -> Sketch
```

Required v0.1 screens:

```text
Sketch Editor
  -> Inbox
  -> Settings
```

Required v0.1 implementation:

- local accountless startup;
- `ActiveSketchBuffer` autosave and restart restoration;
- `Sketch` create/list/edit/delete/status tracking;
- monotonic local Sketch title/counter support;
- local persistence;
- confirmation dialogs for clear, send to Inbox, delete, clear Inbox, and full local reset;
- Android APK and iOS/Xcode build path.

Excluded from v0.1:

- embeddings;
- cosine similarity;
- Outbox;
- `SemanticSketch`;
- semantic links;
- graph persistence;
- Bundles;
- Draft generation;
- KnowledgeItems;
- KnowledgeAreas;
- semantic search;
- Semantic Map;
- sync;
- desktop dependency.

## Target semantic pipeline

The ADR-0005 pipeline remains the target architecture, delivered after v0.1:

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

Core semantic roles:

```text
Inbox = Sketch input buffer
Outbox = SemanticSketch workbench
Vector DB = long-term KnowledgeItem memory
```

## MVP monorepo structure

```text
mops/
  apps/
    mobile/
    desktop/
  packages/
    core/
    db/
    ingestion/
    embeddings/
    clustering/
    search/
    sync/
    shared-types/
  services/
    model-runner/
    sync-server/
  docs/
  memory-bank/
  tools/
  tests/
```

Only the packages needed for the active milestone should be implemented. v0.1 does not require embeddings, clustering, search, sync, model-runner, or desktop implementation.

## Package responsibilities

### apps/mobile

- Flutter/Dart application.
- Shared iOS and Android codebase.
- v0.1 capture, Inbox, Settings, local persistence UI, and confirmations.
- Later milestones add Outbox, semantic search, Bundle review, Draft review, Knowledge Base, KnowledgeAreas, and Semantic Map.
- Narrow native platform adapters only where required.

### apps/desktop

Deferred from v0.1.

- full inbox review later;
- heavy transcription/reprocessing later;
- cleaned note generation later;
- embedding jobs later;
- vector index management later;
- Bundle and KnowledgeArea diagnostics later;
- reindexing and migration tools later.

### packages/core

- domain entities;
- use cases;
- processing state machine;
- correction events;
- semantic link and Bundle rules when their milestones arrive;
- Draft lifecycle when v0.5 arrives;
- KnowledgeItem and KnowledgeArea rules when v0.6-v0.7 arrive.

### packages/db

- local persistence schema;
- migrations;
- repositories;
- graph persistence from v0.3;
- Draft and KnowledgeItem persistence from v0.5-v0.6;
- vector-store adapter from v0.2/v0.6 as needed.

### packages/ingestion

- text ingestion contracts for v0.1;
- voice ingestion contracts later;
- transcription normalization later;
- cleaned note generation contracts later.

### packages/embeddings

- deferred until v0.2;
- embedding provider interface;
- embedding job pipeline;
- model metadata;
- vector dimension checks;
- reindex hooks.

### packages/clustering

- deferred until v0.4;
- Bundle suggestion generation;
- semantic link graph clustering into Bundles;
- Bundle merge/split operations;
- confidence scoring;
- user correction application.

### packages/search

- deferred until v0.6;
- semantic search;
- ranking;
- search result explanation;
- hybrid search later.

### packages/sync

- deferred from the first release train unless a later ADR pulls it forward;
- event log;
- device state;
- conflict handling;
- encrypted cloud transport contracts later.

## Current data model assumptions

v0.1 data objects:

```text
ActiveSketchBuffer
Sketch
ProcessingStatus
LocalSettings
```

Later milestones introduce:

```text
Source
RawNote
Transcription
CleanedNote
EmbeddingRecord
SemanticSketch
SemanticLink
Bundle
Draft
KnowledgeItem
BundleSuggestion
Project
KnowledgeArea
KnowledgeAreaAssignment
SearchResult
CorrectionEvent
Device
SyncEvent
```

Vector records, once introduced, require:

- vector id;
- source id;
- note/chunk id;
- model id;
- model version;
- vector dimension;
- distance metric;
- content hash;
- created timestamp;
- source reference;
- domain tags;
- user/project scope when available.

## AI and model requirements

v0.1 has no required AI/model runtime.

Required later by milestone:

- v0.2: semantic embedding and cosine similarity.
- v0.3: semantic link candidate generation.
- v0.4: Bundle suggestion and clustering.
- v0.5: Draft generation.
- v0.6: KnowledgeItem embedding and semantic search.
- v0.7: KnowledgeArea suggestion.

Required much later:

- custom linguistic vector support;
- summarization;
- entity extraction;
- task extraction;
- task classification;
- goal decomposition;
- project extraction;
- RAG;
- user-adaptive scoring;
- AI-agent task packaging;
- result validation;
- OCR;
- image understanding;
- document parsing;
- web scraping.

## Local-first semantic sync technical direction

Confirmed for later architecture:

- SQLite remains the semantic DB candidate.
- Personal cloud storage is accepted as encrypted file/object transport.
- Sync must be changelog/snapshot based.
- Live SQLite files and WAL files must not be synchronized.
- ANN/vector indexes must be rebuilt locally.
- Raw source files must not be uploaded by default.
- Embedding model metadata is mandatory for every vector record.
- Sync must be deterministic and idempotent.
- Conflict handling is required even for a single-user product because multiple devices can edit offline.

Sync is not part of v0.1.

## Not yet selected

- Desktop framework.
- Exact embedding model.
- Vector index library.
- Speech recognition engine.
- Local LLM runtime.
- Cloud sync provider API strategy.
- Encryption implementation.
- Task storage schema.
- Agent execution runtime.

## Explicitly deferred from v0.1

- Rust-based mobile core engine.
- Voice capture unless explicitly pulled into the first app implementation.
- Local embeddings.
- Outbox.
- Semantic links.
- Graph persistence.
- Bundles.
- Draft generation.
- Knowledge Base.
- KnowledgeAreas.
- Semantic Map.
- Photo analysis.
- Document ingestion.
- Web URL scraping.
- Browser extension.
- GitHub ingestion.
- Full knowledge graph.
- Custom vector DB fork.
- Custom model training.
- Full 3D semantic globe.
- Automatic large project creation without user confirmation.
- Complex document hierarchy.
- Multi-agent processing.
- Device sync.
- Collaboration.
- Automatic file import.
- Full iOS + Android + macOS + Windows + Linux launch at once.

## Repository license configuration

- License file: `LICENSE.md`.
- License: PolyForm Noncommercial License 1.0.0.
- Required notice: `Required Notice: Copyright 2026 Aleksei Khozin (https://github.com/alekho77)`.
- Commercial use requires separate written permission or a commercial license from Aleksei Khozin.
- The repository is source-available / noncommercial, not OSI-approved open source.
- Future third-party dependency choices must be checked against this repository licensing model.

## Constraints

- Mobile OS background execution limits must be treated as a design constraint.
- Heavy indexing may run on desktop or home server later.
- Sync must not depend on live SQLite database files.
- ANN/HNSW/Faiss indexes must not be treated as sync source of truth.
- Raw binary media should not be synchronized by default.
- Vectors from incompatible models must not be mixed in one searchable space.
- Vector dimension and distance metric must match the selected model and index.
- Raw transcription and cleaned note must be stored separately once voice/transcription exists.
- Automatic Bundle/KnowledgeArea assignment must remain editable once introduced.
- User corrections must be stored as first-class events once introduced.
- Flutter UI code must not become the semantic engine boundary.
- Platform-specific mobile code must stay behind narrow Dart-facing adapters.

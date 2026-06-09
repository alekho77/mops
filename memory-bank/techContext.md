# Tech Context

## Confirmed technical direction

- Local-first architecture.
- Mobile-first capture interface.
- Offline-first personal memory.
- SQLite-backed semantic DB for MVP.
- Local vector search for MVP.
- Encrypted sync through user-controlled cloud storage.
- Vector index treated as a rebuildable cache.
- Original files remain outside vector DB.
- Explicit model and embedding versioning required.
- Embedding/custom linguistic models are separated from vector DB implementation.
- First MVP is mobile-only local UX v0.1: ActiveSketchBuffer -> Sketch -> SemanticSketch -> Bundle -> Draft -> KnowledgeItem -> KnowledgeArea, not full multimodal memory.
- Mobile app is the full v0.1 user-facing application.
- Desktop app is deferred from UX v0.1.
- Mobile and desktop product code should live in one monorepo.
- Independent low-level engine forks should live in separate repositories.
- Mobile MVP uses Flutter.
- Dart is the primary mobile application language.
- `apps/mobile` targets iOS and Android from one shared Flutter codebase.
- Rust is not part of the first mobile MVP by default.
- Rust remains a future option only for a measured portable local core need.

## MVP pipeline

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

UX v0.1 screens:

```text
Sketch Editor
  -> Inbox
  -> Outbox
  -> Knowledge Base
  -> Settings
```

UX v0.1 object chain:

```text
ActiveSketchBuffer
  -> Sketch
  -> SemanticSketch
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> KnowledgeArea
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

## MVP package responsibilities

### apps/mobile

- Flutter/Dart application.
- Shared iOS and Android codebase.
- full mobile-only UX v0.1;
- local device storage;
- accountless and registration-free startup;
- Sketch Editor as the startup screen;
- `ActiveSketchBuffer` autosave;
- voice capture;
- text capture;
- offline inbox;
- transcription status;
- cleaned note view;
- lightweight semantic search;
- Bundle suggestion review;
- manual correction;
- compact project pages;
- narrow native platform adapters where required.

### apps/desktop

Deferred from UX v0.1.

- full inbox review;
- heavy transcription/reprocessing;
- cleaned note generation;
- embedding jobs;
- vector index management;
- Bundle building;
- project pages;
- manual correction workflows;
- reindexing and migration tools later.

### packages/core

- use cases;
- domain entities;
- processing state machine;
- correction events;
- semantic link and Bundle rules;
- Draft lifecycle;
- KnowledgeArea/Bundle assignment rules.

### packages/db

- semantic DB schema;
- migrations;
- repositories;
- graph persistence for semantic links;
- Draft and KnowledgeItem persistence;
- vector-store adapter;
- full-text search adapter later.

### packages/ingestion

- voice ingestion contracts;
- text ingestion contracts;
- transcription normalization;
- cleaned note generation contracts;
- chunking later.

### packages/embeddings

- embedding provider interface;
- embedding job pipeline;
- model metadata;
- vector dimension checks;
- reindex hooks.

### packages/clustering

- Bundle suggestion generation;
- semantic link graph clustering into Bundles;
- Bundle merge/split operations;
- project detection;
- confidence scoring;
- user correction application.

### packages/search

- semantic search;
- ranking;
- search result explanation;
- hybrid search later.

### packages/sync

- event log;
- device state;
- conflict handling;
- encrypted cloud transport contracts later.

### packages/shared-types

- shared DTOs and contracts.

## Mobile stack

Selected for MVP:

- Flutter;
- Dart;
- one shared iOS/Android codebase in `apps/mobile`.

Expected validation after mobile code is introduced:

```text
flutter analyze
flutter test
real iOS device build
real Android device build
```

Native adapters are allowed for:

- permissions;
- file access;
- speech integration;
- background execution limits;
- local notifications;
- secure storage.

Do not put heavy semantic engine logic directly into Flutter UI code.

Rust is deferred from the first mobile MVP and may be introduced later only behind explicit interfaces for heavy local vector search, indexing, large file processing, or reusable cross-platform core logic.

## Current data model assumptions

Semantic DB stores:

- active sketch buffer;
- sources;
- raw notes;
- transcriptions;
- cleaned notes;
- summaries;
- embeddings;
- metadata;
- entities;
- relations;
- source references;
- device references;
- content hashes;
- model metadata;
- user corrections;
- Bundle suggestions;
- KnowledgeArea assignments;
- task classification events;
- goal decomposition events;
- execution feedback events.

Vector records require:

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

## Core MVP entities

```text
Source
RawNote
Transcription
CleanedNote
EmbeddingRecord
Sketch
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
ProcessingStatus
```

## Integration targets

Initial integration candidates:

- notes;
- voice notes;
- text notes;
- selected documents later;
- saved links later;
- cloud folders later;
- GitHub repositories later;
- task lists later;
- calendars later.

## AI and model requirements

Required model capabilities for the first MVP:

- speech-to-text;
- text cleanup;
- semantic embedding;
- semantic search;
- Bundle suggestion;
- KnowledgeArea suggestion;
- confidence scoring;
- correction handling.

Required model capabilities later:

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

Confirmed:

- SQLite remains the semantic DB candidate for MVP.
- Personal cloud storage is accepted as encrypted file/object transport.
- Sync must be changelog/snapshot based.
- Live SQLite files and WAL files must not be synchronized.
- ANN/vector indexes must be rebuilt locally.
- Raw source files must not be uploaded by default.
- Embedding model metadata is mandatory for every vector record.
- Sync must be deterministic and idempotent.
- Conflict handling is required even for a single-user product because multiple devices can edit offline.

Initial cloud targets:

- Google Drive;
- OneDrive;
- Dropbox;
- iCloud Drive;
- WebDAV/S3-compatible storage later.

Required implementation modules:

- sync changelog writer;
- sync changelog reader;
- encrypted snapshot writer;
- encrypted snapshot restore;
- device manifest manager;
- Source Resolver;
- semantic compaction job;
- vector index rebuild job;
- model migration/reindex job.

Open implementation choices:

- encryption implementation;
- cloud provider abstraction;
- SQLite vector extension or vector index library;
- conflict merge format;
- snapshot cadence;
- changelog compaction rules;
- key recovery/onboarding flow for new devices.

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

## Explicitly deferred from first MVP

- Rust-based mobile core engine.
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
- Raw transcription and cleaned note must be stored separately.
- Automatic Bundle/KnowledgeArea assignment must remain editable.
- User corrections must be stored as first-class events.
- Flutter UI code must not become the semantic engine boundary.
- Platform-specific mobile code must stay behind narrow Dart-facing adapters.

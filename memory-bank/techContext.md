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
- First MVP is Voice/Text Inbox, not full multimodal memory.
- Mobile app is capture-first.
- Desktop app is processing-first.
- Mobile and desktop product code should live in one monorepo.
- Independent low-level engine forks should live in separate repositories.

## MVP pipeline

```text
Voice/Text Inbox
  -> transcription
  -> cleaned note
  -> embedding
  -> semantic search
  -> suggested clusters
  -> manual correction
  -> project pages
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

- voice capture;
- text capture;
- offline inbox;
- transcription status;
- cleaned note view;
- lightweight semantic search;
- suggested cluster review;
- manual correction;
- compact project pages.

### apps/desktop

- full inbox review;
- heavy transcription/reprocessing;
- cleaned note generation;
- embedding jobs;
- vector index management;
- cluster building;
- project pages;
- manual correction workflows;
- reindexing and migration tools later.

### packages/core

- use cases;
- domain entities;
- processing state machine;
- correction events;
- project/cluster assignment rules.

### packages/db

- semantic DB schema;
- migrations;
- repositories;
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

- suggested cluster generation;
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

## Current data model assumptions

Semantic DB stores:

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
- cluster suggestions;
- project assignments;
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
SuggestedCluster
Project
ProjectAssignment
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
- cluster suggestion;
- project suggestion;
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

## Not yet selected

- Mobile framework.
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

- Photo analysis.
- Document ingestion.
- Web URL scraping.
- Browser extension.
- GitHub ingestion.
- Full knowledge graph.
- Custom vector DB fork.
- Custom model training.
- Full iOS + Android + macOS + Windows + Linux launch at once.

## Constraints

- Mobile OS background execution limits must be treated as a design constraint.
- Heavy indexing may run on desktop or home server later.
- Sync must not depend on live SQLite database files.
- ANN/HNSW/Faiss indexes must not be treated as sync source of truth.
- Raw binary media should not be synchronized by default.
- Vectors from incompatible models must not be mixed in one searchable space.
- Vector dimension and distance metric must match the selected model and index.
- Raw transcription and cleaned note must be stored separately.
- Automatic cluster/project assignment must remain editable.
- User corrections must be stored as first-class events.

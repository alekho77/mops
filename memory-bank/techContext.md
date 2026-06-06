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

## Current data model assumptions

Semantic DB stores:

- chunks;
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
- task classification events;
- goal decomposition events;
- execution feedback events.

Vector records require:

- vector id;
- source id;
- chunk id;
- model id;
- model version;
- vector dimension;
- distance metric;
- content hash;
- created timestamp;
- source reference;
- domain tags;
- user/project scope when available.

## Integration targets

Initial integration candidates:

- local files;
- mobile files;
- browser bookmarks;
- selected browser history;
- cloud folders;
- GitHub repositories;
- notes;
- voice notes;
- task lists;
- calendars later.

## AI and model requirements

Required model capabilities:

- speech-to-text;
- semantic embedding;
- custom linguistic vector support;
- summarization;
- entity extraction;
- task extraction;
- task classification;
- goal decomposition;
- project extraction;
- semantic search and RAG;
- user-adaptive scoring;
- AI-agent task packaging;
- result validation.

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

## Constraints

- Mobile OS background execution limits must be treated as a design constraint.
- Heavy indexing may run on desktop or home server later.
- Sync must not depend on live SQLite database files.
- ANN/HNSW/Faiss indexes must not be treated as sync source of truth.
- Raw binary media should not be synchronized by default.
- Vectors from incompatible models must not be mixed in one searchable space.
- Vector dimension and distance metric must match the selected model and index.

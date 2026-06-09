# MVP Application Split

## MVP pipeline

```text
Inbox Notes
  -> Embedding Pipeline
  -> Outbox Vector Space
  -> Semantic Links
  -> Note Clusters
  -> Draft Document Generation
  -> Knowledge Documents
  -> Vector DB
  -> Project Clusters
  -> Semantic Map
```

Core model:

```text
Inbox = input buffer
Outbox = semantic workbench
Vector DB = long-term memory
```

## Product split

```text
Mobile app = capture client
Desktop app = semantic memory engine
Shared core = common domain and processing contracts
```

## Mobile app responsibilities

### Voice/Text Inbox

- Fast voice note capture.
- Fast text note capture.
- Offline-first local inbox.
- Raw note list.
- Minimal friction capture flow.
- Note CRUD.
- Processing status display.

### Transcription

- Record audio.
- Start speech-to-text processing.
- Store raw transcription.
- Track processing status.

Statuses:

```text
recorded
transcribed
cleaned
embedded
failed
pending_remote_processing
```

### Cleaned note

- Show cleaned text when available.
- Preserve raw transcription separately.
- Allow manual text correction.

### Embedding

- Send cleaned text to the embedding pipeline.
- Store embedding status.
- Do not require the mobile app to own heavy reindexing.

### Outbox

- Show vectorized notes after Inbox processing.
- Show similar notes.
- Allow the user to inspect candidate links.
- Support manual note-to-cluster assignment where lightweight enough for mobile.

### Semantic search

- Search personal notes by meaning.
- Open the original captured note from a result.
- Support text query first; voice query later.

### Suggested clusters

- Show candidate cluster/project assignments.
- Show confidence when available.
- Allow confirm/reject/correct.

### Draft documents

- Show draft documents generated elsewhere when available.
- Allow lightweight draft review and manual text edits if the mobile surface supports it.
- Allow final confirmation when the user is comfortable doing so on mobile.

### Project pages

- Compact project page.
- Related notes.
- Recent thoughts.
- Search inside project.
- Add a new note directly into project context.

## Desktop app responsibilities

### Inbox processing

- Full inbox review.
- Filters for unprocessed notes, failed notes, low-confidence assignments, and unassigned notes.
- Batch processing tools.

### Transcription

- Heavy transcription jobs.
- Reprocess low-quality transcriptions.
- Support model/runtime changes later.

### Cleaned note generation

- Generate cleaned notes from raw transcriptions.
- Extract candidate ideas, tasks, questions, entities, technologies, and projects.
- Preserve all derived interpretations separately from raw input.

### Embedding engine

- Own the main local embedding pipeline for MVP.
- Maintain local vector index.
- Support reindexing.
- Track model id, model version, vector dimension, and distance metric.

### Outbox semantic workbench

- Move notes from Inbox to Outbox.
- Build Outbox vector space.
- Calculate note similarity with cosine similarity.
- Generate candidate semantic links.
- Persist confirmed and rejected links.
- Display vectorized notes, links, and cluster membership.

### Semantic search

- Full semantic search.
- Hybrid search later: keyword + vector + metadata.
- Search by project, date, source type, and processing status.

### Suggested clusters

- Build note clusters.
- Propose project names.
- Merge/split clusters.
- Track confidence.
- Store user decisions as correction events.

### Draft and Knowledge Documents

- Generate a coherent Draft Document from a selected Note Cluster.
- Support manual editing before finalization.
- Save confirmed drafts as Knowledge Documents.
- Embed Knowledge Documents.
- Search similar Knowledge Documents.
- Link Knowledge Documents to Project Clusters.

### Semantic Map

- Provide the MVP 2D map.
- Render notes as points.
- Render semantic links as lines.
- Render note clusters as groups.
- Use color or size for status, review state, or cluster density.

### Project pages

- Full project page.
- Project summary.
- Related notes.
- Open questions.
- Candidate tasks.
- Timeline.
- Linked clusters.
- Export later.

## Shared core packages

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

## Core entities

```text
Source
RawNote
CleanedNote
EmbeddingRecord
InboxNote
OutboxNote
SemanticLink
NoteCluster
DraftDocument
KnowledgeDocument
SearchResult
SuggestedCluster
Project
ProjectCluster
CorrectionEvent
Device
SyncEvent
ProcessingStatus
```

## Monorepo boundary

The main product monorepo contains:

- mobile app;
- desktop app;
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

## MVP exclusions

Do not include in first MVP:

- photo analysis;
- document ingestion;
- web URL scraping;
- browser extension;
- GitHub ingestion;
- knowledge graph;
- custom vector DB fork;
- custom model training;
- full 3D semantic globe;
- automatic large project creation without user confirmation;
- complex document hierarchy;
- multi-agent processing;
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

# MVP Application Split

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

### Semantic search

- Search personal notes by meaning.
- Open the original captured note from a result.
- Support text query first; voice query later.

### Suggested clusters

- Show candidate cluster/project assignments.
- Show confidence when available.
- Allow confirm/reject/correct.

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
SearchResult
SuggestedCluster
Project
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
- full iOS + Android + macOS + Windows + Linux launch at once.

## Design rule

Automatic classification must work as suggestion, not silent truth.

```text
system suggests
user confirms/rejects/corrects
correction is stored as training signal
```

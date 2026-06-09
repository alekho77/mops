# System Patterns

## Accepted system identity

MOPS is **Mind Orchestration & Planning System**.

It is a voice-first personal AI orchestration system, not only a task manager and not only a second brain.

## Core boundaries

- Individual-first.
- Local-first.
- Mobile-first capture.
- Privacy-first memory.
- Cloud as encrypted transport, not master backend.
- Semantic DB as durable source of truth.
- Vector index as rebuildable local cache.
- Raw files remain outside vector DB by default.
- Model Layer produces vectors; Vector DB Layer stores and searches vectors.
- Mobile app is the full local user-facing app for private `v0.x` and public `v1.x`.
- Desktop app is deferred until `v2.0+`.
- Shared product logic belongs in the main monorepo.
- Low-level model/vector DB forks belong in separate repositories.

## Release maturity pattern

Version ladder:

```text
v0.x  = private mobile-only pre-1.0 alpha/beta builds, not MVP
v1.0  = first public mobile-only MVP/product baseline
v1.x  = public mobile-only iteration line
v2.0+ = desktop expansion line
```

Desktop app, desktop-owned embedding pipeline, desktop-owned vector index, desktop-owned reindexing, desktop semantic workbench, and cross-device desktop workflows are forbidden in `v0.x` and `v1.x`.

## Incremental private mobile release train pattern

ADR-0010 supersedes the old v0.1 scope from ADR-0005 and ADR-0006. ADR-0011 classifies the whole `v0.x` train as private pre-1.0 alpha/beta builds. ADR-0013 keeps early Outbox and Knowledge Base surfaces list/card-first and makes the first Semantic Map read-only experimental visualization. ADR-0014 adds v0.2 Mobile Voice Capture using OS speech APIs.

Every `v0.x` milestone must be:

- local-first unless a later ADR explicitly changes that;
- buildable as Android APK or iOS/Xcode build;
- testable on a real phone with a manual acceptance scenario;
- narrow enough to ship without pulling in the whole semantic workbench;
- described as a private pre-MVP alpha/beta build, not as MVP.

Release train:

```text
v0.1 Mobile Capture + Inbox
v0.2 Mobile Voice Capture
v0.3 Semantic Outbox
v0.4 Semantic Links
v0.5 Bundles
v0.6 Drafts
v0.7 Knowledge Base
v0.8 KnowledgeAreas
v0.9 2D Semantic Map
```

## v0.1 capture/inbox pattern

v0.1 is a private mobile-only, local-only, accountless, and registration-free pre-1.0 build.

Required v0.1 chain:

```text
ActiveSketchBuffer
  -> Sketch
  -> Inbox
```

Required v0.1 screens:

```text
Sketch Editor
  -> Inbox
  -> Settings
```

The app starts on Sketch Editor. New input is stored in `ActiveSketchBuffer`, autosaved after every change, restored after restart, and converted into a `Sketch` only after an explicit Send to Inbox action.

v0.1 includes:

- Flutter mobile app shell;
- local device storage;
- `ActiveSketchBuffer` autosave and restart restoration;
- Sketch create/list/edit/delete/status tracking;
- Inbox;
- Settings;
- one-tap Send to Inbox with Undo;
- confirmations for clear, delete, clear Inbox, full local reset, and other destructive or batch actions.

v0.1 excludes:

- embeddings;
- cosine similarity;
- voice capture;
- Outbox;
- SemanticSketch;
- semantic links;
- graph persistence;
- Bundles;
- Drafts;
- KnowledgeItems;
- KnowledgeAreas;
- semantic search;
- Semantic Map;
- sync;
- desktop dependency.

## Target semantic workbench pattern

ADR-0005 remains accepted as the target semantic model:

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

This is not the v0.1 scope. It is delivered through v0.3-v0.9 after v0.2 Mobile Voice Capture.

Core rule:

```text
capture first
process later
review semantic links
assemble Draft records
store corrections
```

Core semantic roles:

```text
Inbox = Sketch input buffer
Outbox = SemanticSketch workbench
Vector DB = long-term KnowledgeItem memory
```

Outbox is user-visible working space, not a passive queue. Early Outbox is list/card-first: processed `SemanticSketch` records, details, similar Sketch references, and suggested Bundle cards. It must not start as a map-first graph editor with multi-select, Bundle areas, merged document editing, or bulk merge.

Fixed target terms:

- Sketch: raw note.
- SemanticSketch: vectorized sketch before finalization.
- Semantic Link: relationship between SemanticSketch records.
- Bundle: group of linked SemanticSketch records.
- Draft: editable coherent text assembled from a Bundle.
- KnowledgeItem: finalized document in the main semantic DB and vector space.
- KnowledgeArea: large cluster of KnowledgeItems.
- Semantic Map: visualization of Outbox or long-term memory.

## Early semantic mobile surface pattern

First Outbox:

- list processed `SemanticSketch` records;
- open processed Sketch details;
- show embedding/status metadata;
- show similar Sketch references;
- show suggested Bundle cards;
- exclude graph editor behavior, map-first UI, Bundle area editing, merged document editing, and bulk merge.

First Knowledge Base:

- list `KnowledgeItem` records;
- search;
- open;
- edit;
- delete with confirmation;
- exclude KnowledgeArea editing, map-first UI, visual graph editing, bulk operations, and complex delete propagation.

First Semantic Map:

- read-only experimental visualization;
- not the primary editor for Outbox, Bundles, KnowledgeItems, or KnowledgeAreas;
- no create/edit/delete/link/merge operations from the map.

## Flutter mobile pattern

The mobile product uses Flutter and Dart.

`apps/mobile` is one shared Flutter codebase for iOS and Android.

Flutter owns the user-facing surface for private `v0.x` builds and public `v1.x` releases. In v0.1 that means capture, Inbox, Settings, local persistence UI, and confirmations.

Flutter must not own heavy semantic processing or durable semantic engine rules directly.

Native iOS/Android code is allowed only as narrow platform adapters for:

- permissions;
- file access;
- speech integration;
- background execution limits;
- local notifications;
- secure storage.

Rust is deferred from the private pre-1.0 train and public v1.0 mobile baseline. It remains a future option only for measured heavy local processing or portable native core needs behind explicit interfaces.

## v0.2 voice capture pattern

v0.2 adds Mobile Voice Capture without changing the v0.1 text capture foundation.

First voice flow:

```text
record speech
  -> OS speech recognition
  -> insert recognized text at current cursor position
  -> ActiveSketchBuffer autosave
```

Rules:

- use iOS/Android OS speech APIs through narrow Dart-facing adapters;
- recognized speech becomes ordinary editable text in `ActiveSketchBuffer`;
- Settings exposes only speech/microphone permission and availability status;
- raw audio is not persisted by default;
- no local speech model, MOPS cloud speech backend, transcript cleanup, voice commands, background dictation, or semantic processing in v0.2;
- OS speech privacy and offline behavior depend on platform capabilities and user settings.

## Desktop v2.0+ semantic engine pattern

Desktop is deferred until `v2.0+`. It cannot own or be required for embeddings, vector indexes, reindexing, semantic workbench behavior, or cross-device workflows in `v0.x` or `v1.x`.

Candidate v2.0+ responsibilities:

- full inbox review;
- transcription reprocessing;
- cleaned note generation;
- embedding pipeline;
- local semantic DB management;
- vector index management;
- semantic search;
- Bundle building;
- project page generation;
- manual correction workflows;
- model/version migration support later.

Desktop may become the preferred place to debug storage, embeddings, clustering, and project extraction in v2.0+, but it must not be required for private v0.x builds or public v1.x mobile releases.

## Suggestion over silent automation pattern

MOPS must not silently treat AI classification as truth.

Automatic classification creates suggestions:

- candidate domain;
- candidate KnowledgeArea;
- candidate Bundle;
- confidence;
- alternative candidates when available.

User actions are first-class events once the relevant milestone exists:

- confirm suggestion;
- reject suggestion;
- move KnowledgeItem to another KnowledgeArea;
- create new KnowledgeArea;
- merge Bundles;
- split Bundles;
- generate Draft from a Bundle;
- edit Draft;
- confirm Draft as KnowledgeItem.

Corrections become training/adaptation signals.

## Capture undo and destructive action confirmation pattern

v0.1 treats capture send as a fast, reversible action:

- Send `ActiveSketchBuffer` to Inbox without confirmation;
- create a `Sketch` and clear the editor buffer;
- show Undo immediately after send;
- Undo restores the sent text to `ActiveSketchBuffer` and removes the just-created `Sketch`, unless rollback is unsafe because of a later conflicting edit/action.

v0.1 requires confirmation for:

- clear `ActiveSketchBuffer`;
- clear Inbox;
- delete Sketch;
- full local reset.

Later milestones must add confirmations for their own destructive, reset, and batch actions, including Outbox clear, link deletion, Bundle merge/split, Draft deletion, KnowledgeItem deletion, KnowledgeArea deletion, and saving to the Knowledge Base.

## Raw and interpreted memory separation pattern

Raw capture must not be overwritten by AI interpretation.

A captured thought can produce multiple derived records:

```text
RawNote
  -> Transcription
  -> CleanedNote
  -> Summary
  -> EmbeddingRecord
  -> BundleSuggestion
  -> KnowledgeAreaAssignment
  -> CorrectionEvent
```

Raw input remains available as source of truth. Cleaned notes, summaries, entities, Bundles, and KnowledgeArea assignments are derived interpretations.

## Personal MOPS agent pattern

Each user has a personal trainable MOPS agent.

The agent:

- listens to user input;
- stores source-linked semantic memory;
- learns from confirmations and corrections;
- classifies thoughts and tasks;
- extracts projects and goals;
- decomposes goals into executable tasks;
- prepares tasks for AI agents;
- tracks execution results;
- adjusts future behavior from feedback.

## Voice-first command pattern

Voice is a primary interface, not an optional input method.

All core flows should support voice commands:

- capture;
- search;
- task creation;
- task correction;
- goal decomposition;
- planning;
- review;
- agent delegation.

## Source-linked memory pattern

MOPS stores semantic representations and source references, not full raw documents inside vector memory.

Stored memory units include:

- semantic chunks;
- summaries;
- embeddings;
- metadata;
- entities;
- semantic tags;
- relations;
- source references;
- model metadata.

Source Link Checker remains responsible for checking source availability and asking the user for decisions on broken links.

## Model and vector store separation pattern

MOPS separates semantic vector production from vector storage and search.

Model Layer:

- produces embeddings or custom linguistic vectors;
- owns model id, version, vector dimension, and metric expectations;
- determines semantic representation quality.

Vector DB Layer:

- stores ready vectors;
- maintains search indexes;
- performs nearest-neighbor search;
- returns ids, scores, and metadata references.

MOPS Semantic Engine:

- decides what to parse, chunk, embed, store, and search;
- connects vector ids to semantic chunks and source references;
- reconstructs context from nearest-neighbor candidates;
- validates model/vector compatibility.

Vector search results are candidates, not semantic truth.

## Domain memory pattern

Memory is organized by semantic domain.

Initial domains:

- work;
- personal;
- projects;
- code;
- research;
- learning;
- finance;
- health;
- people;
- decisions.

Domains are not hard silos. Cross-domain relations are allowed.

## Project crystallization pattern

MOPS should extract projects from accumulated documents, notes, bookmarks, repositories, and thoughts.

Crystallization pipeline:

```text
raw memory
  -> topics
  -> candidate projects
  -> goals
  -> subgoals
  -> tasks
  -> atomic actions
```

## Adaptive classification pattern

Task classification should start with generic matrices and gradually adapt to the user.

Supported classification dimensions:

- urgent / important;
- impact / effort;
- MoSCoW;
- RICE;
- ICE;
- cost of delay;
- energy;
- context;
- risk;
- dependency;
- delegability;
- AI-agent readiness.

User corrections are training events.

## Agent task package pattern

Tasks delegated to AI agents must be converted into explicit work packages.

A package includes:

- goal;
- task statement;
- source context;
- relevant files or links;
- constraints;
- expected output;
- acceptance criteria;
- validation rules;
- permission scope;
- audit metadata.

## Feedback loop pattern

Execution history updates future behavior.

Tracked results:

- completed;
- failed;
- skipped;
- blocked;
- overdue;
- misclassified;
- incorrectly decomposed;
- successfully delegated;
- failed delegation.

Feedback affects prioritization, decomposition, planning, and delegation decisions.

## Individual local-first semantic sync pattern

MOPS is explicitly an individual product.

Out of scope:

- teams;
- shared workspaces;
- collaborative editing;
- realtime multi-user collaboration;
- enterprise access-control lists;
- organization accounts;
- server-owned source of truth.

Each personal device stores a full local copy of the semantic DB.

Personal cloud storage is used as encrypted sync and backup transport, not as the live database and not as master backend.

Sync source of truth:

- encrypted append-only changelog;
- encrypted snapshots;
- device manifests;
- semantic chunks;
- summaries;
- embeddings;
- metadata;
- entities;
- relations;
- source fingerprints;
- model metadata.

Never synchronize as source of truth:

- live `db.sqlite`;
- SQLite WAL files;
- ANN/HNSW/Faiss indexes;
- temporary caches;
- raw documents by default;
- media archives by default.

Vector index remains a rebuildable local cache.

## Semantic memory budget pattern

MOPS must not blindly index everything.

Initial storage budget target:

```text
100 semantic chunks/day
384-dimensional embeddings
FP16 persisted vectors
short text + summary + metadata
```

Five-year estimate:

```text
100 chunks/day x 365 x 5 = 182,500 chunks
182,500 x 384 x 2 bytes = 140,160,000 bytes approx 140 MB
full semantic DB with metadata approx hundreds of MB to 1-2 GB
```

INT8 storage would require a future ADR. It is not the current persisted vector format. FP32 can still be used inside model/runtime computation, but persisted vectors are FP16 by default per ADR-0009.

The budget is valid only when MOPS stores compact meaning, not full extracted documents.

Required quality controls:

- duplicate detection;
- similar chunk merge;
- stale relation pruning;
- orphaned source cleanup;
- low-value memory demotion;
- old cluster compression;
- changelog compaction;
- periodic snapshots.

## Source Link Checker pattern

Source Link Checker validates whether semantic/vector records still point to real user-owned files.

It does not automatically resolve, search, infer, or relink sources.

Tracked fields:

- document_id;
- device_id;
- path;
- content_hash;
- partial_hash;
- file_size;
- modified_at;
- last_seen_at;
- availability;
- source_type.

Expected states:

- available;
- modified;
- missing;
- permission_denied;
- needs_user_decision.

Check flow:

1. Check whether `path` exists.
2. If the file exists, verify size and hashes.
3. If size and hashes match, keep the source link `available`.
4. If the file exists but hashes do not match, mark the source link `modified` and ask the user to refresh semantic/vector information for that file.
5. If the file is missing, mark the source link `missing` and ask the user to either delete the related vector record from the vector DB or provide the new file path manually.
6. If the file cannot be read because of permissions, mark it `permission_denied` and ask the user to fix access or remove the source link.

Forbidden behavior:

- no automatic filesystem search;
- no watched-folder scan for moved files;
- no content-hash based relocation;
- no partial-hash based relocation;
- no semantic fingerprint matching;
- no embedding-similarity based source matching;
- no silent vector update or deletion.

Broken source links do not get repaired automatically and do not invalidate semantic memory.

## Repository licensing boundary

MOPS repository is source-available under PolyForm Noncommercial License 1.0.0.

Allowed without separate permission:

- reading source code;
- downloading source code;
- building applications;
- personal use;
- noncommercial use.

Not allowed without separate written permission or commercial license:

- commercial use;
- selling MOPS-based products;
- embedding MOPS into commercial products;
- using MOPS as the basis for paid services or commercial systems.

Required notice must remain attached to redistributed copies:

```text
Required Notice: Copyright 2026 Aleksei Khozin (https://github.com/alekho77)
```

This is a repository distribution boundary, not a runtime architecture boundary.

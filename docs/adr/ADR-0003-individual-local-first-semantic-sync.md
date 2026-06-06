# ADR-0003: Individual local-first semantic sync

Status: Accepted

## Context

MOPS is explicitly an individual product: one personal MOPS agent per user, across that user's own devices. The product does not target teams, shared workspaces, enterprise knowledge bases, collaborative editing, realtime multi-user flows, or corporate ACL models.

The system must keep a compact semantic memory on all personal devices. The database stores short semantic fragments, summaries, embeddings, metadata, relations, model metadata, and source references. It does not store raw documents, photos, videos, audio, Git repositories, or other large source files inside the vector database.

The user is expected to already have some personal cloud storage such as Google Drive, OneDrive, Dropbox, iCloud Drive, WebDAV, or S3-compatible storage. This storage can be used for sync and backup without requiring MOPS to operate a mandatory central backend.

## Decision

MOPS will use an individual local-first architecture:

- each personal device keeps a full local copy of the semantic DB;
- SQLite is the default semantic DB candidate for MVP;
- vector indexes are local rebuildable caches, not source of truth;
- raw source files remain outside the semantic/vector DB by default;
- source records store device/path references, fingerprints, hashes, and availability metadata;
- personal cloud storage is used as encrypted transport and backup, not as the master database;
- sync uses encrypted append-only changelogs, snapshots, and device manifests;
- live SQLite files, SQLite WAL files, ANN/HNSW/Faiss indexes, and temporary caches are not synchronized as source of truth;
- Source Resolver is responsible for checking and recovering broken source links;
- semantic cleanup, deduplication, compaction, and old cluster compression are required parts of the long-term design.

The initial sizing target is approximately:

```text
100 semantic chunks/day
384-dimensional embeddings
FP32 or INT8 vectors
short text + summary + metadata
```

For five years of heavy personal use:

```text
100 chunks/day × 365 × 5 = 182,500 chunks
384 FP32 raw vectors ≈ 280 MB
384 INT8 raw vectors ≈ 70 MB
full semantic DB with metadata ≈ hundreds of MB to ~1–2 GB
```

## Consequences

Positive consequences:

- MOPS can run without a mandatory backend controlled by the project.
- Personal cloud storage is enough for MVP sync and backup.
- A small free or low-cost cloud quota can support multi-year semantic memory.
- The system remains aligned with privacy-first and offline-first requirements.
- Every device can search personal memory locally.
- Vector indexes can be rebuilt after model/index changes.
- Broken source links do not destroy semantic memory.

Trade-offs and obligations:

- Sync must be implemented at the changelog/snapshot level, not by sharing a live database file.
- The weakest device defines the practical storage and runtime ceiling.
- Mobile OS background execution limits must be treated as a hard constraint.
- Conflict handling is still required even for a single-user product, because multiple devices may edit offline.
- Embedding model metadata and vector compatibility must be stored explicitly.
- Reindexing must support lazy or batched migration after embedding model changes.
- Source recovery requires a dedicated resolver using path, hash, partial hash, file metadata, and semantic fingerprinting.
- The system must prevent uncontrolled growth through semantic budget, deduplication, compaction, and cleanup.

Risks:

- 100 documents/day can produce far more than 100 chunks/day if full documents are ingested blindly.
- Cloud folder sync APIs do not provide database transactions across files.
- Directly syncing `db.sqlite`, WAL files, or vector indexes can corrupt state.
- Low-quality chunks, duplicates, stale relations, and overly generic summaries can degrade search quality before storage becomes a problem.
- If raw documents are accidentally synchronized by default, the storage and privacy model breaks.

# Memory-bank patch: local-first semantic sync

## Target: memory-bank/systemPatterns.md

Add:

```markdown
## Individual local-first semantic sync pattern

MOPS is explicitly an individual product.

Out of scope:

- teams;
- shared workspaces;
- collaborative editing;
- realtime multi-user collaboration;
- enterprise ACL;
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
```

Add:

```markdown
## Semantic memory budget pattern

MOPS must not blindly index everything.

Initial storage budget target:

```text
100 semantic chunks/day
384-dimensional embeddings
FP32 or INT8 vectors
short text + summary + metadata
```

Five-year estimate:

```text
100 chunks/day × 365 × 5 = 182,500 chunks
384 FP32 raw vectors ≈ 280 MB
384 INT8 raw vectors ≈ 70 MB
full semantic DB with metadata ≈ hundreds of MB to ~1–2 GB
```

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
```

Add:

```markdown
## Source Resolver recovery pattern

Source Resolver restores access from semantic records to original files.

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
- moved;
- modified;
- missing;
- device_offline;
- permission_denied;
- duplicate_found;
- needs_manual_resolution.

Recovery order:

1. Check exact path.
2. Check content hash.
3. Check partial hash and file size.
4. Search watched folders.
5. Match semantic fingerprint by title, key phrases, entities, chunk hashes, and embedding similarity.
6. Ask user only for unresolved ambiguous cases.

Broken source links do not invalidate semantic memory.
```

## Target: memory-bank/techContext.md

Add:

```markdown
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
```

## Target: memory-bank/activeContext.md

Add to `Recent decisions`:

```markdown
- MOPS is explicitly individual-first: no teams, shared workspaces, realtime collaboration, enterprise ACL, or server-owned source of truth.
- Each personal device should keep a full local copy of the semantic DB.
- Personal cloud storage is accepted as encrypted sync and backup transport, not as the live database or master backend.
- The semantic DB stores compact meaning: short chunks, summaries, embeddings, metadata, relations, entities, model metadata, and source references.
- Raw files remain outside the semantic/vector DB by default; records keep device/path references and fingerprints.
- Live SQLite files, WAL files, and ANN/vector indexes must not be synchronized as source of truth.
- Source Resolver is required for broken path checks, source recovery, availability tracking, and device-aware resolution.
- Long-term storage quality requires semantic budget, deduplication, compaction, stale relation cleanup, and cluster compression.
```

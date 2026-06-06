# Local-first semantic sync

## Product boundary

MOPS is an individual product.

One user owns one personal MOPS agent across their personal devices.

Out of scope:

- teams;
- shared workspaces;
- collaborative editing;
- realtime multi-user collaboration;
- enterprise ACL;
- organization accounts;
- server-owned source of truth.

## Local data model

Each device stores a full local copy of the semantic DB.

The semantic DB stores compact meaning, not raw files:

- semantic chunks;
- summaries;
- embeddings;
- metadata;
- entities;
- semantic tags;
- relations;
- model metadata;
- source references;
- device/path references;
- content hashes;
- partial hashes;
- availability state.

Raw files remain outside the semantic/vector DB by default.

## Storage roles

```text
Local SQLite semantic DB
  durable source of truth

Local vector index
  rebuildable search cache

Local files
  original user-owned sources

Cloud folder
  encrypted sync transport and backup
```

## Cloud folder layout

Cloud storage must not contain a live database file as source of truth.

Use immutable or append-only sync artifacts:

```text
MOPS/
  devices/
    <device-id>/
      changes/
        000001.jsonl.zst.enc
        000002.jsonl.zst.enc
      manifest.json.enc
  snapshots/
    <device-id>/
      2026-06-06.sqlite.zst.enc
  backups/
  blobs/
```

Allowed in cloud sync:

- encrypted changelog packets;
- encrypted snapshots;
- encrypted manifests;
- compact semantic records;
- embeddings;
- metadata;
- relation records;
- source fingerprints.

Not allowed as sync source of truth:

- live `db.sqlite`;
- SQLite WAL files;
- ANN/HNSW/Faiss indexes;
- temporary caches;
- raw documents by default;
- media archives by default.

## Source Resolver

Source Resolver resolves semantic records back to original files.

Tracked fields:

```text
document_id
device_id
path
content_hash
partial_hash
file_size
modified_at
last_seen_at
availability
source_type
```

Expected states:

```text
available
moved
modified
missing
device_offline
permission_denied
duplicate_found
needs_manual_resolution
```

Recovery strategies:

1. Check exact path.
2. Check content hash.
3. Check partial hash and file size.
4. Search watched folders.
5. Match semantic fingerprint from title, key phrases, entities, chunk hashes, and embedding similarity.
6. Ask user only for unresolved ambiguous cases.

Broken source links do not invalidate semantic memory.

## Vector/index boundary

The semantic DB stores vector records and model metadata.

The ANN/vector index is a local derived artifact.

Required vector metadata:

```text
vector_id
chunk_id
model_id
model_version
dimensions
quantization
distance_metric
normalization
created_at
```

Vectors produced by incompatible models must not be searched in the same vector space.

## Size target

MVP sizing target:

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

The database size target depends on semantic selection. Blind document ingestion can exceed this budget quickly.

## Required cleanup mechanisms

Long-term storage quality requires:

- duplicate detection;
- similar chunk merge;
- orphaned source cleanup;
- stale relation pruning;
- old cluster compression;
- low-value memory demotion;
- changelog compaction;
- periodic encrypted snapshots;
- local vector index rebuilds.

## Implementation constraints

- Sync must be deterministic and idempotent.
- Conflict handling is required for offline multi-device use.
- Mobile background execution is limited and must not be assumed reliable.
- Cloud provider APIs must be treated as object/file transports, not transactional databases.
- Encryption must be present before cloud sync is enabled.
- Source files must never be uploaded by default as part of semantic sync.

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

## Source Link Checker

Source Link Checker validates whether semantic/vector records still point to real user-owned files.

It does not automatically resolve, search, infer, or relink sources.

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
modified
missing
permission_denied
needs_user_decision
```

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

Broken source links do not get repaired automatically.

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
storage_dtype
distance_metric
normalization
created_at
```

Vectors produced by incompatible models must not be searched in the same vector space.

Persisted vector records use FP16 by default, as accepted in ADR-0009. FP32 can be used inside model/runtime computation, but vectors must be converted to FP16 before persistence unless a future ADR changes the storage format.

## Size target

Sizing target:

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

INT8 storage would require a future ADR. It is not the current persisted vector format.

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

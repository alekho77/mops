# MOPS

**MOPS** is a personal local-first AI layer for a user's digital life.

It is an individual product for one person and their personal devices. It is not a team platform, not an enterprise knowledge base, and not a cloud service where the server is the primary source of truth.

The core idea is to create a personal “Mops” — an AI agent that gradually accumulates, structures, and reuses the user's semantic memory.

## Project goal

MOPS helps the user:

- capture thoughts, notes, and voice fragments quickly;
- search personal context semantically, not only by keywords;
- connect notes, documents, projects, links, and ideas;
- restore the context of past decisions;
- automatically extract projects, topics, tasks, and semantic clusters;
- work with personal memory locally and privately.

## Core principle

MOPS does not store full documents inside the vector database.

The semantic DB stores only:

- short semantic fragments;
- summaries;
- embeddings;
- metadata;
- entities;
- semantic tags;
- relations between fragments;
- links to source documents;
- device/path references;
- fingerprints and hashes for source recovery.

The original files stay where they already are: on the phone, laptop, desktop, external drive, or in the user's cloud folder.

## Architecture

```text
Device A
  Local SQLite semantic DB
  Local vector index
  Local files
  Local source resolver

Device B
  Full replicated semantic DB
  Own local vector index
  Own local files
  Own source resolver

Cloud Drive
  Encrypted changelog
  Encrypted snapshots
  Device manifests
  Backups
```

## Local-first model

Each device stores a full copy of the semantic DB.

Cloud storage is used not as the primary backend, but as an encrypted transport for synchronization and backups.

Suitable options include:

- Google Drive;
- OneDrive;
- Dropbox;
- iCloud Drive;
- WebDAV/S3-compatible storage in the future.

## What is synchronized

- semantic chunks;
- summaries;
- embeddings;
- document metadata;
- source fingerprints;
- paths and device references;
- entities;
- relations;
- model metadata;
- encrypted snapshots;
- append-only changelog.

## What is not synchronized as source of truth

- live `db.sqlite`;
- SQLite WAL files;
- ANN/HNSW/Faiss indexes;
- temporary caches;
- raw documents by default;
- photos, videos, audio, and large binary files by default.

The vector index is treated as a derived local cache. It can be rebuilt on each device from the semantic DB.

## Source Resolver

A separate layer is responsible for restoring links to original documents.

It tracks:

- `document_id`;
- `device_id`;
- `path`;
- `content_hash`;
- `partial_hash`;
- `file_size`;
- `modified_at`;
- `last_seen_at`;
- `availability`.

If a file is moved, renamed, or temporarily unavailable, semantic memory does not break. Only access to the original source is affected, and it can be restored later.

## MVP

The first realistic MVP:

1. Mobile-first application.
2. Local SQLite semantic DB.
3. Local vector search.
4. Manual notes, voice notes, links, and selected documents.
5. Short semantic chunks instead of full document storage.
6. Encrypted synchronization through the user's personal cloud folder.
7. Source Resolver for link checking and recovery.
8. Basic semantic cleanup mechanism.

## Why mobile-first

The phone is the natural center of MOPS:

- it is always nearby;
- it contains voice, camera, notes, links, files, and notifications;
- it is well suited for quickly capturing thoughts;
- it becomes the persistent interface to personal memory.

Desktop machines and home servers can be used for heavy indexing, batch processing, and large archives.

## Data storage

MVP target:

```text
100 semantic chunks/day
384-dimensional embeddings
FP32 or INT8 vectors
short text + summary + metadata
```

Estimated size for 5 years:

```text
100 chunks/day × 365 × 5 = 182,500 chunks

384 FP32 embeddings ≈ 280 MB raw vectors
384 INT8 embeddings ≈ 70 MB raw vectors

Full semantic DB with metadata ≈ hundreds of MB to ~1–2 GB
```

This makes multi-year personal semantic memory possible even within a small personal cloud storage quota.

## Important constraints

MOPS should not become a blind indexer of everything.

Main risks:

- too many low-quality chunks;
- duplicates;
- stale relations;
- incompatible embedding models;
- broken file paths;
- reindexing after model changes;
- mobile OS background execution limits;
- changelog growth without compaction/snapshot mechanisms.

Therefore, MOPS should be a filter of personal meaning, not a data vacuum cleaner.

## Design principles

- Individual-first.
- Local-first.
- Mobile-first.
- Offline-first.
- Privacy-first.
- Cloud as transport, not master.
- Semantic DB as source of truth.
- Vector index as rebuildable cache.
- Raw files outside vector DB.
- Short meaning fragments instead of full document copies.
- Explicit model versioning.
- Encrypted sync from day one.

## Non-goals

MOPS is not intended for:

- team collaboration;
- enterprise knowledge bases;
- collaborative editing;
- realtime collaboration;
- shared workspaces;
- complex ACLs;
- storing all user files inside its own database;
- a mandatory cloud backend as the master server.

## Status

The project is in the architecture and MVP design stage.

The first focus is a compact, private, synchronized semantic memory for one user and their personal devices.

# Active Context

## Current stage

MOPS is in architecture and MVP definition stage.

## Recent decisions

- Accepted product name: **MOPS — Mind Orchestration & Planning System**.
- Accepted product identity: voice-first personal AI orchestration system.
- MOPS is not limited to task management or passive second brain behavior.
- Each user should have a personal trainable MOPS agent.
- The agent learns from user confirmations, corrections, manual task classification, goal decomposition edits, and execution results.
- Voice brain dump is a primary input flow.
- MOPS should index selected personal context: notes, documents, saved links, local/cloud files, and GitHub repositories.
- MOPS should automatically crystallize projects from accumulated thoughts and documents.
- MOPS should transform thoughts into goals, goals into subgoals, subgoals into tasks, and tasks into atomic actions.
- MOPS should prepare execution-ready packages for AI agents.
- Successful and failed task execution should feed back into future planning and classification.
- Model Layer must be separated from Vector DB Layer: models produce embeddings/custom linguistic vectors, while vector DB stores ready vectors, indexes them, and searches nearest neighbors.
- MOPS Semantic Engine coordinates parsing, chunking, embedding, vector storage, search, source references, and context reconstruction.
- Vector DB is not semantic source of truth; it is searchable infrastructure over source-linked memory records.
- First MVP is now selected as **Voice/Text Inbox MVP**.
- Accepted MVP pipeline: `Voice/Text Inbox -> transcription -> cleaned note -> embedding -> semantic search -> suggested clusters -> manual correction -> project pages`.
- Mobile app role: capture client for fast voice/text input, offline inbox, lightweight search, suggestion review, and manual correction.
- Desktop app role: semantic memory engine for transcription reprocessing, cleaned note generation, embeddings, clustering, search, and project pages.
- Product code should start as one monorepo containing `apps/mobile`, `apps/desktop`, and shared `packages/*`.
- Future custom vector DB, model forks, speech/OCR/vision engines, benchmarks, and native runtime experiments should live in separate repositories.
- Automatic classification must suggest, not silently decide. User correction is a first-class event.
- Raw transcription and cleaned note must be stored separately.
- First MVP excludes photo analysis, document ingestion, web scraping, browser extension, GitHub ingestion, full knowledge graph, custom vector DB fork, and custom model training.
- Repository licensing accepted: PolyForm Noncommercial License 1.0.0 in `LICENSE.md`.
- Repository is source-available / noncommercial, not OSI-approved open source.
- Personal and noncommercial use is allowed; commercial use requires separate written permission or a commercial license from Aleksei Khozin.
- Required notice: `Required Notice: Copyright 2026 Aleksei Khozin (https://github.com/alekho77)`.
- MOPS is explicitly individual-first: no teams, shared workspaces, realtime collaboration, enterprise access-control lists, or server-owned source of truth.
- Each personal device should keep a full local copy of the semantic DB.
- Personal cloud storage is accepted as encrypted sync and backup transport, not as the live database or master backend.
- The semantic DB stores compact meaning: short chunks, summaries, embeddings, metadata, relations, entities, model metadata, and source references.
- Raw files remain outside the semantic/vector DB by default; records keep device/path references and fingerprints.
- Live SQLite files, WAL files, and ANN/vector indexes must not be synchronized as source of truth.
- Source Resolver is required for broken path checks, source recovery, availability tracking, and device-aware resolution.
- Long-term storage quality requires semantic budget, deduplication, compaction, stale relation cleanup, and cluster compression.

## Current open choices

- Mobile framework.
- Desktop framework.
- Exact embedding model.
- Vector index library.
- Speech recognition engine.
- Local LLM runtime.
- Sync implementation details.

## Working constraint

Do not expand all capabilities at once. Ship the Voice/Text Inbox MVP first while preserving the broader MOPS orchestration architecture.

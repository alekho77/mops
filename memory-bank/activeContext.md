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
- First MVP is now selected as **MVP v0.1: Sketch/SemanticSketch semantic workbench**.
- Previous Voice/Text Inbox pipeline is retained as capture context, but the accepted MVP v0.1 product loop is now centered on Sketch, SemanticSketch, links, Bundles, Drafts, KnowledgeItems, and KnowledgeAreas.
- MVP v0.1 is clarified as `Sketch -> SemanticSketch -> Semantic Links -> Bundle -> Draft -> KnowledgeItem -> Vector DB -> KnowledgeArea -> Semantic Map`.
- UX v0.1 is mobile-only, local-only, accountless, and registration-free.
- UX v0.1 always starts on the new Sketch Editor backed by `ActiveSketchBuffer`.
- `ActiveSketchBuffer` is autosaved independently and is not an Inbox `Sketch` until the user confirms sending it to Inbox.
- UX v0.1 main screens are Sketch Editor, Inbox, Outbox, Knowledge Base, and Settings.
- UX v0.1 object chain is `ActiveSketchBuffer -> Sketch -> SemanticSketch -> Bundle -> Draft -> KnowledgeItem -> KnowledgeArea`.
- All destructive or state-moving actions require confirmation in v0.1: clear, delete, send to Outbox, merge, return to Inbox, save to Knowledge Base, and full local reset.
- Outbox is not just a queue; it is the semantic workbench for SemanticSketch records, candidate links, editable Bundles, and Draft generation before long-term storage.
- MVP terms are fixed: Sketch, SemanticSketch, Semantic Link, Bundle, Draft, KnowledgeItem, KnowledgeArea, Semantic Map.
- MVP v0.1 requires Sketch CRUD, manual Sketch-to-SemanticSketch processing, local embeddings, cosine-similarity links, manual link confirmation/deletion, graph persistence, Bundle build/edit flows, Draft generation/editing, final KnowledgeItem embedding/search, KnowledgeArea linking, and a 2D Outbox map.
- Deferred until after MVP v0.1: full 3D globe, unconfirmed automatic large project creation, complex document hierarchy, multi-agent processing, device sync, collaboration, and automatic file import.
- Mobile app role for UX v0.1: full local user-facing app for capture, Inbox, Outbox, Knowledge Base, Settings, lightweight search, suggestion review, and manual correction.
- Desktop app role: deferred semantic memory engine for later transcription reprocessing, cleaned note generation, embeddings, clustering, search, and project pages.
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
- Mobile MVP framework accepted: Flutter with Dart.
- `apps/mobile` should be one shared Flutter codebase for iOS and Android.
- Native iOS/Android code is limited to narrow platform adapters for permissions, file access, speech integration, background limits, local notifications, and secure storage.
- Rust is deferred from the first mobile MVP and remains only a possible future portable local engine for measured heavy-processing needs.
- Codex-generated Flutter changes must be validated with `flutter analyze`, `flutter test`, and real iOS/Android builds once mobile code exists.

## Current open choices

- Desktop framework.
- Exact embedding model.
- Vector index library.
- Speech recognition engine.
- Local LLM runtime.
- Sync implementation details.

## Working constraint

Do not expand all capabilities at once. Ship MVP v0.1 first while preserving the broader MOPS orchestration architecture.

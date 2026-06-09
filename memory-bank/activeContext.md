# Active Context

## Current stage

MOPS is in architecture and MVP definition stage.

## Recent decisions

- Accepted product name: **MOPS - Mind Orchestration & Planning System**.
- Accepted product identity: voice-first personal AI orchestration system.
- MOPS is not limited to task management or passive second brain behavior.
- Each user should have a personal trainable MOPS agent.
- Voice brain dump remains a primary input flow, but the first installable release is narrower than the full voice/semantic vision.
- Model Layer must be separated from Vector DB Layer: models produce embeddings/custom linguistic vectors, while vector DB stores ready vectors, indexes them, and searches nearest neighbors.
- MOPS Semantic Engine coordinates parsing, chunking, embedding, vector storage, search, source references, and context reconstruction.
- Vector DB is not semantic source of truth; it is searchable infrastructure over source-linked memory records.
- ADR-0010 supersedes the v0.1 release scope from ADR-0005 and ADR-0006.
- The full ADR-0005 semantic workbench model remains the target: `Sketch -> SemanticSketch -> Semantic Links -> Bundle -> Draft -> KnowledgeItem -> Vector DB -> KnowledgeArea -> Semantic Map`.
- Mobile release train is now staged as feature milestones from v0.1 through v0.8.
- Each `v0.x` release must be buildable as Android APK or iOS/Xcode build and have a manual phone acceptance scenario.
- v0.1 is **Mobile Capture + Inbox** only.
- v0.1 object chain is `ActiveSketchBuffer -> Sketch`.
- v0.1 main screens are Sketch Editor, Inbox, and Settings.
- v0.1 requires local-only/accountless startup, `ActiveSketchBuffer` autosave, `Sketch` CRUD, Inbox, Settings, confirmations, local persistence, and phone build paths.
- v0.1 explicitly excludes embeddings, cosine similarity, Outbox, SemanticSketch, semantic links, graph persistence, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, semantic search, sync, desktop dependency, and Semantic Map.
- v0.2 introduces Semantic Outbox: manual Sketch-to-SemanticSketch processing, local embeddings, embedding metadata, similar sketches by cosine similarity, and Outbox list/detail views.
- v0.3 introduces Semantic Links: candidate links, manual confirm/delete, correction events, and basic graph persistence without Bundle editing.
- v0.4 introduces Bundles: build Bundles from confirmed links, inspect/merge/split Bundles, and manual SemanticSketch-to-Bundle assignment.
- v0.5 introduces Drafts: generate editable Drafts from selected Bundles, edit Drafts, and confirm Draft as a KnowledgeItem candidate.
- v0.6 introduces Knowledge Base: persist KnowledgeItems, embed/search KnowledgeItems, and basic Knowledge Base list/detail/search.
- v0.7 introduces KnowledgeAreas: manual and suggested KnowledgeArea assignment, create/rename/delete areas, and correction events for area changes.
- v0.8 introduces a phone-testable 2D Semantic Map.
- Mobile MVP framework accepted: Flutter with Dart.
- `apps/mobile` should be one shared Flutter codebase for iOS and Android.
- Native iOS/Android code is limited to narrow platform adapters for permissions, file access, speech integration, background limits, local notifications, and secure storage.
- Rust is deferred from the first mobile MVP and remains only a possible future portable local engine for measured heavy-processing needs.
- Product code should start as one monorepo containing `apps/mobile`, `apps/desktop`, and shared `packages/*`.
- Future custom vector DB, model forks, speech/OCR/vision engines, benchmarks, and native runtime experiments should live in separate repositories.
- Automatic classification must suggest, not silently decide. User correction is a first-class event.
- Raw transcription and cleaned note must be stored separately.
- Repository licensing accepted: PolyForm Noncommercial License 1.0.0 in `LICENSE.md`.
- Repository is source-available / noncommercial, not OSI-approved open source.
- Personal and noncommercial use is allowed; commercial use requires separate written permission or a commercial license from Aleksei Khozin.
- Required notice: `Required Notice: Copyright 2026 Aleksei Khozin (https://github.com/alekho77)`.
- MOPS is explicitly individual-first: no teams, shared workspaces, realtime collaboration, enterprise access-control lists, or server-owned source of truth.
- Each personal device should keep a full local copy of the semantic DB when semantic DB milestones arrive.
- Personal cloud storage is accepted as encrypted sync and backup transport, not as the live database or master backend.
- Semantic DB and memory metadata are durable source of truth; vector indexes are rebuildable local caches.

## Current open choices

- Desktop framework.
- Exact embedding model.
- Vector index library.
- Speech recognition engine.
- Local LLM runtime.
- Sync implementation details.

## Working constraint

Do not expand all capabilities at once. Ship v0.1 Mobile Capture + Inbox first while preserving the broader MOPS orchestration architecture.

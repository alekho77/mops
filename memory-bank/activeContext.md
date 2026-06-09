# Active Context

## Current stage

MOPS is in architecture, private pre-1.0 mobile alpha/beta, and v1.0 mobile MVP definition stage.

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
- ADR-0011 supersedes release-maturity interpretation from ADR-0010 and desktop-scope portions of ADR-0002.
- ADR-0013 narrows first Outbox and Knowledge Base surfaces: Outbox is list/card-first with suggested Bundle cards, Knowledge Base is list/search/open/edit/delete, and the first Semantic Map is read-only experimental visualization.
- ADR-0014 chooses OS speech APIs for first voice capture, adds `v0.2 Mobile Voice Capture`, and shifts semantic milestones to `v0.3-v0.9`.
- Version ladder is fixed: `v0.x` = private mobile-only pre-1.0 alpha/beta builds; `v1.0` = first public mobile-only MVP/product baseline; `v1.x` = mobile-only public iteration line; `v2.0+` = desktop expansion.
- `v0.x` builds are not MVPs and are not public releases.
- Desktop app, desktop-owned embedding pipeline, desktop-owned vector index, desktop-owned reindexing, desktop semantic workbench, and cross-device desktop workflows are not allowed in `v0.x` or `v1.x`.
- Desktop starts no earlier than `v2.0`.
- The full ADR-0005 semantic workbench model remains the target: `Sketch -> SemanticSketch -> Semantic Links -> Bundle -> Draft -> KnowledgeItem -> Vector DB -> KnowledgeArea -> Semantic Map`.
- Private pre-1.0 mobile alpha/beta train is staged as feature milestones from v0.1 through v0.9.
- Each `v0.x` build must be buildable as Android APK or iOS/Xcode build and have a manual phone acceptance scenario.
- v0.1 is **Mobile Capture + Inbox** only.
- v0.1 object chain is `ActiveSketchBuffer -> Sketch`.
- v0.1 main screens are Sketch Editor, Inbox, and Settings.
- v0.1 requires local-only/accountless startup, `ActiveSketchBuffer` autosave, `Sketch` CRUD, Inbox, Settings, one-tap Send to Inbox with Undo, destructive/reset/batch confirmations, local persistence, and phone build paths.
- ADR-0012 supersedes the old Send to Inbox confirmation rule: capture sends are fast and reversible with Undo, while clear/delete/reset/batch operations still require confirmation.
- v0.1 explicitly excludes voice capture, embeddings, cosine similarity, Outbox, SemanticSketch, semantic links, graph persistence, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, semantic search, sync, desktop dependency, and Semantic Map.
- v0.2 introduces Mobile Voice Capture: OS speech API through narrow iOS/Android adapters, speech/microphone permission status, and recognized text inserted at the current cursor position in `ActiveSketchBuffer`.
- v0.3 introduces Semantic Outbox: manual Sketch-to-SemanticSketch processing, local embeddings, embedding metadata, similar sketches by cosine similarity, Outbox list/detail views, and suggested Bundle cards without graph editing.
- v0.4 introduces Semantic Links: candidate links, manual confirm/delete, correction events, and basic graph persistence without Bundle editing.
- v0.5 introduces Bundles: build Bundles from confirmed links, inspect Bundle cards, and adjust membership through list/card workflows without map-based editing, merged document editing, or bulk merge.
- v0.6 introduces Drafts: generate editable Drafts from selected Bundles, edit Drafts, and confirm Draft as a KnowledgeItem candidate.
- v0.7 introduces Knowledge Base: persist KnowledgeItems and support list/search/open/edit/delete with delete confirmation.
- v0.8 introduces KnowledgeAreas: manual and suggested KnowledgeArea assignment, create/rename/delete areas, and correction events for area changes.
- v0.9 introduces a phone-testable read-only 2D Semantic Map experiment.
- Mobile product framework accepted: Flutter with Dart.
- `apps/mobile` should be one shared Flutter codebase for iOS and Android.
- Native iOS/Android code is limited to narrow platform adapters for permissions, file access, speech integration, background limits, local notifications, and secure storage.
- Rust is deferred from the private pre-1.0 train and public v1.0 mobile baseline; it remains only a possible future portable local engine for measured heavy-processing needs.
- Product code should start as one monorepo containing `apps/mobile`, shared `packages/*`, and at most a placeholder for future `apps/desktop` v2.0+ work.
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

- Desktop framework for v2.0+.
- Exact embedding model.
- Vector index library.
- Future local speech model after the first OS speech API voice flow.
- Local LLM runtime.
- Sync implementation details.

## Working constraint

Do not expand all capabilities at once. Ship private v0.1 Mobile Capture + Inbox first, keep v1.x mobile-only, and defer desktop to v2.0+ while preserving the broader MOPS orchestration architecture.

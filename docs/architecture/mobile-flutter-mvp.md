# Mobile Flutter MVP Architecture

## Decision

The MOPS mobile MVP uses Flutter and Dart.

The mobile application is one shared codebase for iOS and Android under `apps/mobile`.

## Role in v0.1

The mobile app is the full local user-facing application for the active mobile release milestone.

For v0.1, the mobile app is limited to Mobile Capture + Inbox.

Responsibilities:

- text note capture;
- Sketch Editor startup screen;
- `ActiveSketchBuffer` autosave and restart restoration;
- offline local inbox;
- processing status display;
- `Sketch` create/list/edit/delete/status tracking;
- Settings.

Non-goals for first mobile MVP:

- local embeddings;
- Outbox;
- semantic links;
- graph persistence;
- Bundles;
- Drafts;
- KnowledgeItems;
- KnowledgeAreas;
- semantic search;
- Semantic Map;
- voice capture unless explicitly pulled into the first implementation;
- heavy indexing;
- full reindexing;
- model migration;
- custom vector DB implementation;
- local engine implementation in Rust;
- full desktop-grade semantic processing.

Desktop semantic processing is deferred from v0.1. If later introduced, it must remain a separate processing surface rather than a requirement for the first mobile-local release.

Later mobile milestones add Semantic Outbox, links, Bundles, Drafts, Knowledge Base, KnowledgeAreas, and the 2D Semantic Map as defined by ADR-0010.

## Code boundaries

```text
apps/mobile
  -> Flutter UI
  -> Dart application state
  -> capture workflows
  -> local inbox UI
  -> platform adapters
  -> shared package interfaces
```

Shared MOPS logic should be consumed through `packages/*` interfaces when available.

Flutter UI code must not own durable semantic engine rules directly.

## Platform adapters

Native iOS and Android adapters are allowed only for OS-specific capabilities:

- permissions;
- file access;
- speech integration;
- background execution limits;
- local notifications;
- secure storage.

Adapters must expose narrow Dart-facing interfaces.

## Deferred Rust boundary

Rust is not part of the first mobile MVP by default.

Rust can be introduced later only as a measured optimization or portability layer for:

- local vector search;
- local index management;
- large file processing;
- reusable cross-platform core engine logic;
- native modules behind Dart interfaces.

The first implementation should stay Flutter/Dart unless performance or portability requirements justify a separate native core.

## Validation

Expected validation once mobile code exists:

```text
flutter analyze
flutter test
real iOS device build
real Android device build
```

Codex-generated Flutter changes must pass analyzer, tests, and platform builds before being treated as accepted implementation.

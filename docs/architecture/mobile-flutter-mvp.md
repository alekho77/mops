# Mobile Flutter Product Architecture

## Decision

The MOPS mobile product uses Flutter and Dart.

The mobile application is one shared codebase for iOS and Android under `apps/mobile`.

## Role in v0.x and v1.x

The mobile app is the full local user-facing application for all private `v0.x` alpha/beta builds and the public `v1.x` mobile product line.

For private v0.1, the mobile app is limited to Mobile Capture + Inbox.

Responsibilities:

- text note capture;
- Sketch Editor startup screen;
- `ActiveSketchBuffer` autosave and restart restoration;
- offline local inbox;
- processing status display;
- `Sketch` create/list/edit/delete/status tracking;
- Settings.

Non-goals for the first private mobile build:

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
- desktop app or desktop-owned semantic processing.

Desktop semantic processing is deferred until v2.0 or later. It must not be required by private `v0.x` builds, the public `v1.0` mobile MVP/product baseline, or any `v1.x` mobile release.

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

Rust is not part of the private pre-1.0 mobile train or the public v1.0 mobile MVP/product baseline by default.

Rust can be introduced later only as a measured optimization or portability layer for:

- local vector search;
- local index management;
- large file processing;
- reusable cross-platform core engine logic;
- native modules behind Dart interfaces.

The first implementation should stay Flutter/Dart unless performance or portability requirements justify a separate native core in a later version.

## Validation

Expected validation once mobile code exists:

```text
flutter analyze
flutter test
real iOS device build
real Android device build
```

Codex-generated Flutter changes must pass analyzer, tests, and platform builds before being treated as accepted implementation.

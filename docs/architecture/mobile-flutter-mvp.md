# Mobile Flutter MVP Architecture

## Decision

The MOPS mobile MVP uses Flutter and Dart.

The mobile application is one shared codebase for iOS and Android under `apps/mobile`.

## Role in MVP

The mobile app is the full local user-facing application for UX v0.1.

Responsibilities:

- voice note capture;
- text note capture;
- offline local inbox;
- raw transcription display;
- cleaned note display;
- processing status display;
- lightweight semantic search;
- Bundle and KnowledgeArea suggestion review;
- manual correction of KnowledgeArea assignment;
- compact KnowledgeArea pages;
- Sketch Editor startup screen;
- `ActiveSketchBuffer` autosave;
- Outbox semantic workbench;
- Knowledge Base view;
- Settings.

Non-goals for first mobile MVP:

- heavy indexing;
- full reindexing;
- model migration;
- custom vector DB implementation;
- local engine implementation in Rust;
- full desktop-grade semantic processing.

Desktop semantic processing is deferred from UX v0.1. If later introduced, it must remain a separate processing surface rather than a requirement for the first mobile-local release.

## Code boundaries

```text
apps/mobile
  -> Flutter UI
  -> Dart application state
  -> capture workflows
  -> local inbox UI
  -> search and review UI
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

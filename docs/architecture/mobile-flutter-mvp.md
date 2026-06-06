# Mobile Flutter MVP Architecture

## Decision

The MOPS mobile MVP uses Flutter and Dart.

The mobile application is one shared codebase for iOS and Android under `apps/mobile`.

## Role in MVP

The mobile app is the capture-first client.

Responsibilities:

- voice note capture;
- text note capture;
- offline local inbox;
- raw transcription display;
- cleaned note display;
- processing status display;
- lightweight semantic search;
- cluster and project suggestion review;
- manual correction of project assignment;
- compact project pages.

Non-goals for first mobile MVP:

- heavy indexing;
- full reindexing;
- model migration;
- custom vector DB implementation;
- local engine implementation in Rust;
- replacing desktop semantic processing.

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

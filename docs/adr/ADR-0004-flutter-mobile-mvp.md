# ADR-0004: Flutter mobile MVP

Status: Accepted

ADR-0011 clarifies release maturity: private `v0.x` alpha/beta builds are not MVP releases, while `v1.0` is the first public mobile-only MVP/product baseline. This ADR applies Flutter to both the private pre-1.0 mobile train and the public v1.x mobile product line.

## Context

MOPS needs a mobile application for the first MVP that can run on iOS and Android without maintaining two separate native applications.

The mobile MVP is capture-first: voice input, text input, offline inbox, lightweight review, lightweight semantic search, suggestion review, and manual correction.

The project considered whether the mobile app must be written separately for each platform using native stacks, or whether one cross-platform codebase is acceptable.

Rust was discussed only as a possible future low-level engine language, not as the first mobile UI stack.

## Decision

Use Flutter for the MOPS mobile MVP.

Use Dart as the mobile application language.

Implement one shared mobile codebase for iOS and Android under `apps/mobile`.

Use platform-specific native adapters only where required by OS APIs:

- permissions;
- file access;
- speech integration;
- background execution constraints;
- local notifications;
- secure storage.

Do not introduce Rust into the first mobile MVP by default.

Keep Rust as a future option for a portable local core engine only if MOPS later needs heavy local vector search, indexing, file processing, or reusable cross-platform native logic.

## Consequences

Positive:

- One mobile codebase can target iOS and Android.
- MVP delivery avoids duplicated Swift and Kotlin UI implementations.
- Flutter is suitable for capture screens, inbox UI, search UI, project pages, and correction workflows.
- Dart becomes the primary mobile language for the MVP.
- Native code remains limited to narrow adapters.

Negative:

- The team must learn and maintain Dart/Flutter.
- Some mobile OS features still require platform-specific work.
- Background execution, permissions, speech, secure storage, and file access must be tested on real iOS and Android devices.
- Flutter UI is not a direct wrapper over native iOS/Android UI components.

Risks and obligations:

- `flutter analyze` and mobile tests must be part of the validation flow once the app exists.
- Codex-generated Flutter changes must be checked by analyzer, tests, and device builds.
- Heavy semantic processing must not be forced into Flutter UI code.
- Any future Rust engine must remain behind explicit mobile-facing interfaces and must not be introduced before there is a measured need.

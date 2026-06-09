# MOPS

MOPS is a personal local-first AI assistant for one user and their own devices.

This repository is intended for advanced users who want to inspect, build, and install MOPS from source.

## Status

MOPS is in architecture and MVP definition stage.

The first implementation target is a mobile-only local v0.1 release:

```text
ActiveSketchBuffer
  -> Sketch
  -> Inbox
```

Production mobile and desktop releases are not published yet.

## Mobile release train

Each `v0.x` release must be buildable as an Android APK or iOS/Xcode build and testable on a real phone.

| Version | Milestone | Scope |
| --- | --- | --- |
| v0.1 | Mobile Capture + Inbox | Flutter app shell, local-only startup, `ActiveSketchBuffer`, autosave, `Sketch` CRUD, Inbox, Settings, confirmations, local persistence. |
| v0.2 | Semantic Outbox | Manual `Sketch` to `SemanticSketch` processing, local embeddings, embedding metadata, similar sketches by cosine similarity, Outbox list/details. |
| v0.3 | Semantic Links | Candidate links, manual confirm/delete, correction events, basic graph persistence without Bundle editing. |
| v0.4 | Bundles | Build Bundles from confirmed links, inspect/merge/split Bundles, manual `SemanticSketch` to Bundle assignment. |
| v0.5 | Drafts | Generate editable Drafts from selected Bundles, edit Drafts, confirm Draft as a `KnowledgeItem` candidate. |
| v0.6 | Knowledge Base | Persist KnowledgeItems, embed/search KnowledgeItems, basic Knowledge Base list/detail/search. |
| v0.7 | KnowledgeAreas | Manual and suggested KnowledgeArea assignment, create/rename/delete areas, correction events for area changes. |
| v0.8 | 2D Semantic Map | Phone-testable 2D map for SemanticSketch records, links, Bundles, KnowledgeItems, and KnowledgeAreas. |

## User scope

MOPS is designed for personal use:

- voice notes;
- text notes;
- mobile-only local capture and Inbox management for v0.1;
- cleaned personal notes;
- semantic search;
- Bundle and KnowledgeArea suggestions;
- editable semantic links;
- draft document generation;
- manual correction of AI suggestions;
- private device-oriented memory.

MOPS is not a team workspace, enterprise knowledge base, collaborative editor, or mandatory cloud backend.

## Build from source

Mobile source is planned under `apps/mobile`.

Target platforms:

- iOS;
- Android.

Expected prerequisites after source code is present:

- Flutter SDK;
- Xcode for iOS builds;
- Android Studio and Android SDK for Android builds;
- a real mobile device for final validation.

Expected workflow:

```bash
cd apps/mobile
flutter pub get
flutter analyze
flutter test
flutter run
```

Release builds:

```bash
flutter build apk --release
flutter build ipa --release
```

If `apps/mobile` is absent in the checked-out revision, that revision does not contain runnable mobile app code yet.

Desktop support is planned, but the desktop framework and release process are not selected yet.

## Repository map

- `AGENTS.md` — instructions for AI agents.
- `LICENSE.md` — repository license.
- `docs/adr/` — accepted decisions.
- `docs/architecture/` — technical architecture notes.
- `memory-bank/` — working context for AI agents.
- `apps/mobile/` — planned mobile app.
- `apps/desktop/` — planned desktop app.
- `packages/` — planned shared packages.

## Documentation policy

`README.md` is the public entry point for users.

Architecture and product decisions belong in `docs/adr/` and `docs/architecture/`.

AI-agent working context belongs in `AGENTS.md` and `memory-bank/`.

## License

MOPS is source-available for personal and noncommercial use under the PolyForm Noncommercial License 1.0.0.

Commercial use requires separate written permission or a commercial license from Aleksei Khozin.

See `LICENSE.md` for the full license text.

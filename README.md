# MOPS

MOPS is a personal local-first AI assistant for one user and their own devices.

This repository is intended for advanced users who want to inspect, build, and install MOPS from source.

## Status

MOPS is in architecture, private pre-1.0 mobile alpha/beta, and v1.0 mobile MVP definition stage.

The first implementation target is a private mobile-only local v0.1 alpha/beta build:

```text
ActiveSketchBuffer
  -> Sketch
  -> Inbox
```

Production mobile and desktop releases are not published yet.

## Version ladder

| Version family | Maturity | Product boundary |
| --- | --- | --- |
| `v0.x` | Private pre-1.0 alpha/beta builds | Mobile-only phone-test builds. Not MVP. Not public releases. |
| `v1.0` | First public MVP/product baseline | Mobile-only public release. No desktop app or desktop processing dependency. |
| `v1.x` | Public mobile iteration line | Mobile-only improvements over v1.0. No desktop app or desktop processing dependency. |
| `v2.0+` | Desktop expansion line | First allowed family for desktop app, desktop semantic engine, heavier embedding/index/reindex tooling, and cross-device workflows. |

## Private pre-1.0 mobile train

Each `v0.x` build must be buildable as an Android APK or iOS/Xcode build and testable on a real phone.

| Version | Milestone | Scope |
| --- | --- | --- |
| v0.1 | Mobile Capture + Inbox | Flutter app shell, local-only startup, `ActiveSketchBuffer`, autosave, `Sketch` CRUD, Inbox, Settings, Undo for Send to Inbox, confirmations for destructive/reset/batch actions, local persistence. |
| v0.2 | Semantic Outbox | Manual `Sketch` to `SemanticSketch` processing, local embeddings, embedding metadata, similar sketches by cosine similarity, Outbox list/details, suggested Bundle cards. |
| v0.3 | Semantic Links | Candidate links, manual confirm/delete, correction events, basic graph persistence without Bundle editing. |
| v0.4 | Bundles | Build Bundles from confirmed links, inspect Bundle cards, adjust membership through list/card workflows. |
| v0.5 | Drafts | Generate editable Drafts from selected Bundles, edit Drafts, confirm Draft as a `KnowledgeItem` candidate. |
| v0.6 | Knowledge Base | Persist KnowledgeItems, list/search/open/edit/delete KnowledgeItems. |
| v0.7 | KnowledgeAreas | Manual and suggested KnowledgeArea assignment, create/rename/delete areas, correction events for area changes. |
| v0.8 | 2D Semantic Map | Phone-testable read-only experimental map for SemanticSketch records, links, Bundles, KnowledgeItems, and KnowledgeAreas. |

## User scope

MOPS is designed for personal use:

- voice notes;
- text notes;
- private mobile-only local capture and Inbox management for v0.1;
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

Desktop support is planned for v2.0 or later. The desktop framework and release process are not selected yet.

## Repository map

- `AGENTS.md` — instructions for AI agents.
- `LICENSE.md` — repository license.
- `docs/adr/` — accepted decisions.
- `docs/architecture/` — technical architecture notes.
- `memory-bank/` — working context for AI agents.
- `apps/mobile/` — planned mobile app.
- `apps/desktop/` — planned v2.0+ desktop app.
- `packages/` — planned shared packages.

## Documentation policy

`README.md` is the public entry point for users.

Architecture and product decisions belong in `docs/adr/` and `docs/architecture/`.

AI-agent working context belongs in `AGENTS.md` and `memory-bank/`.

## License

MOPS is source-available for personal and noncommercial use under the PolyForm Noncommercial License 1.0.0.

Commercial use requires separate written permission or a commercial license from Aleksei Khozin.

See `LICENSE.md` for the full license text.

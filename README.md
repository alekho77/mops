# MOPS

MOPS is a personal local-first AI assistant for one user and their own devices.

This repository is intended for advanced users who want to inspect, build, and install MOPS from source.

## Status

MOPS is in architecture and MVP definition stage.

The first implementation target is MVP v0.1: Sketch -> SemanticSketch -> Bundle -> Draft -> KnowledgeItem -> KnowledgeArea.

Production mobile and desktop releases are not published yet.

## User scope

MOPS is designed for personal use:

- voice notes;
- text notes;
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

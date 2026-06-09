# ADR-0014: First voice capture uses OS speech API

## Status: Accepted

## Supersedes

This ADR supersedes the open speech recognition engine choice for the first mobile voice capture flow.

ADR-0010 remains accepted for the private mobile pre-1.0 release train, with `v0.2` now assigned to Mobile Voice Capture and the semantic milestones shifted to `v0.3` through `v0.9`.

## Context

MOPS is voice-first, but `v0.1` is intentionally limited to text Mobile Capture + Inbox. Voice capture should still arrive in the early private `v0.x` train, just not in the first installable build.

The simplest first implementation is to use speech recognition already provided by the mobile OS. A bundled local speech model would add model packaging, runtime, performance, battery, and quality risks too early. A MOPS cloud speech backend would violate the local-first direction for the first voice flow.

## Decision

`v0.2` is Mobile Voice Capture.

The first voice capture engine is the platform OS speech API:

- iOS speech recognition through a narrow iOS adapter;
- Android speech recognition through a narrow Android adapter;
- Dart-facing Flutter interface hides platform differences.

The first voice capture flow is:

```text
record speech
  -> OS speech recognition
  -> insert recognized text at current cursor position
  -> ActiveSketchBuffer autosave
```

Recognized text is ordinary editable text in `ActiveSketchBuffer`. It follows the same autosave, restart restore, Send to Inbox, and Undo behavior as typed input.

`v0.2` Settings includes only speech/microphone permission and availability status needed for this flow.

The first voice capture flow does not include:

- local speech model;
- MOPS cloud speech backend;
- raw audio persistence by default;
- raw audio archive management;
- transcript cleanup;
- voice commands;
- background dictation;
- semantic processing.

OS speech privacy behavior depends on iOS/Android capabilities, user settings, installed language packs, and platform policy. MOPS must not represent OS speech recognition as a MOPS-owned local model.

## Consequences

Positive:

- Voice capture arrives early without expanding `v0.1`.
- Implementation stays small and platform-native.
- Captured speech immediately enters the existing `ActiveSketchBuffer -> Sketch -> Inbox` capture path.

Trade-offs:

- Speech quality and offline behavior depend on the OS and device configuration.
- Platform permission and availability differences must be surfaced in v0.2.
- Future local speech models remain a separate decision.

Obligations:

- Keep `v0.1` text-only.
- Shift Semantic Outbox and later semantic milestones to `v0.3` through `v0.9`.
- Keep the first voice adapter narrow and Dart-facing.
- Do not persist raw audio by default.

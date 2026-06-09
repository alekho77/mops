# ADR-0006: Mobile-only local UX v0.1

## Status: Accepted, release scope superseded by ADR-0010, Send to Inbox confirmation superseded by ADR-0012

ADR-0010 supersedes the v0.1 release scope in this ADR. The mobile-only, local-only, accountless UX principles remain accepted. The full semantic chain is delivered incrementally after v0.1.

ADR-0012 supersedes the v0.1 rule that sending `ActiveSketchBuffer` to Inbox requires confirmation. Send to Inbox is now a one-tap explicit action with Undo.

## Context

MOPS needs a first private phone-testable UX that is smaller than the broader multi-device MOPS architecture. It should prioritize fast capture and local privacy without requiring accounts, registration, cloud sync, semantic processing, or a desktop application.

The broader target object chain remains:

```text
ActiveSketchBuffer
  -> Sketch
  -> SemanticSketch
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> KnowledgeArea
```

For v0.1, only the capture and Inbox part of that chain is required.

## Decision

MOPS UX v0.1 is a private mobile-only local application build.

General principles:

1. No account.
2. No registration.
3. Only the mobile app is part of v0.1.
4. Storage is local on the device.
5. The app always starts on the new sketch editor.
6. Any user input is autosaved.
7. All destructive actions require confirmation.

The accepted v0.1 main screens are:

```text
Sketch Editor
  -> Inbox
  -> Settings
```

Navigation:

| Screen | Available transitions |
| --- | --- |
| Sketch Editor | Inbox, Settings |
| Inbox | Sketch Editor, Settings |
| Settings | back to previous screen |

## UX Chain

The accepted v0.1 user-facing chain is:

```text
New sketch
  -> Inbox
```

The accepted v0.1 object chain is:

```text
ActiveSketchBuffer
  -> Sketch
```

The core v0.1 UX decision is:

```text
Sketch Editor = fast capture
Inbox = raw sketch list
Settings = local storage, confirmations, reset, and app information
```

Outbox, SemanticSketch records, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, semantic search, and the Semantic Map enter the mobile UX in later ADR-0010 milestones.

## v0.1 Required Behavior

- Start directly on Sketch Editor.
- Persist `ActiveSketchBuffer` separately from Inbox `Sketch` records.
- Autosave `ActiveSketchBuffer` after edits.
- Restore `ActiveSketchBuffer` after app restart.
- Create a `Sketch` only after an explicit user Send to Inbox action.
- Clear `ActiveSketchBuffer` only after successful Send to Inbox or confirmed manual clear.
- Support create, list, edit, delete, and status-track operations for `Sketch` records.
- Send to Inbox without confirmation and provide Undo.
- Require confirmation for deletion, clearing, full local reset, and batch destructive actions.
- Keep local storage as the only v0.1 persistence layer.
- Provide Android APK and iOS/Xcode build paths for phone testing.

## Consequences

Positive:

- The first UX has no onboarding, account, or registration blocker.
- Local-only storage matches the privacy-first boundary.
- The startup screen optimizes for immediate capture.
- Autosave prevents accidental loss of unstructured thoughts.
- Confirmation rules reduce data-loss risk in early versions.

Trade-offs:

- Device sync is not part of UX v0.1.
- Desktop processing is not part of the private v0.1 build.
- Heavy semantic processing is deferred until later mobile release milestones.
- v0.1 does not validate embeddings, semantic search, graph editing, or draft generation.

Obligations:

- Do not describe v0.1 as requiring Outbox or Knowledge Base screens.
- Do not include embeddings, cosine links, graph persistence, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, or the Semantic Map in v0.1 acceptance criteria.
- Keep `ActiveSketchBuffer` and `Sketch` as the only required v0.1 product objects.

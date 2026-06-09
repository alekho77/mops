# ADR-0010: Incremental private mobile pre-1.0 release train

## Status: Accepted, release maturity superseded by ADR-0011, Send to Inbox confirmation superseded by ADR-0012

ADR-0011 supersedes the release-maturity interpretation of this ADR. The `v0.1` through `v0.8` sequence remains accepted as a private mobile pre-1.0 alpha/beta train, not as MVP or public release scope.

ADR-0012 supersedes the v0.1 rule that `Send to Inbox` requires confirmation. `Send to Inbox` is now a one-tap explicit action with Undo; confirmations remain required for destructive, reset, and batch operations.

## Supersedes

This ADR supersedes the v0.1 release scope portions of:

- ADR-0005: Sketch, SemanticSketch, and Semantic Workbench MVP.
- ADR-0006: Mobile-only local UX v0.1.

ADR-0005 remains accepted as canonical terminology and target semantic model. ADR-0006 remains accepted for local-only mobile UX principles. This ADR narrows what is required for each private phone-testable mobile pre-1.0 build.

## Context

ADR-0005 made the first installable build too large by grouping capture, embeddings, cosine links, graph persistence, Bundles, Draft generation, KnowledgeItems, KnowledgeAreas, and the 2D Semantic Map into v0.1.

That scope is closer to a small local Obsidian-like semantic graph plus vector database and local AI pipeline. MOPS needs smaller private phone-testable pre-1.0 builds where each `v0.x` can be built as an Android APK or iOS/Xcode build and validated on a real device.

## Decision

MOPS will use an incremental private mobile pre-1.0 release train. Every `v0.x` build must:

- be local-first and accountless unless a later ADR explicitly changes that;
- be buildable as an Android APK or iOS/Xcode build;
- have a manual phone acceptance scenario;
- keep vector indexes and AI output as infrastructure or suggestions, not semantic truth.

The first pre-1.0 build is intentionally limited to capture and Inbox.

`v0.x` builds are private alpha/beta builds. They are not MVPs and are not public releases.

## Private pre-1.0 release train

| Version | Name | Scope | Phone acceptance scenario |
| --- | --- | --- | --- |
| v0.1 | Mobile Capture + Inbox | Flutter app shell, local-only startup, `ActiveSketchBuffer`, autosave, `Sketch` CRUD, Inbox, Settings, confirmations for destructive/reset/batch actions, Undo for Send to Inbox, local persistence. No embeddings, Outbox, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, or Semantic Map. | Install on a phone, write a sketch, restart the app, confirm the buffer is restored, send it to Inbox without confirmation, verify Undo restores the buffer, edit/delete it, and verify data remains local. |
| v0.2 | Semantic Outbox | Manual `Sketch` to `SemanticSketch` processing, local embeddings, embedding metadata, similar sketches by cosine similarity, Outbox list/detail views. | Install on a phone, process sketches into Outbox, inspect similar sketches, and verify embedding status is visible. |
| v0.3 | Semantic Links | Candidate links, manual confirm/delete, rejected and confirmed link persistence, correction events, basic graph persistence without Bundle editing. | Install on a phone, review candidate links, confirm one, reject one, restart, and verify both decisions persist. |
| v0.4 | Bundles | Build Bundles from confirmed links, inspect/merge/split Bundles, manual `SemanticSketch` to Bundle assignment. | Install on a phone, create or adjust a Bundle, split or merge it, restart, and verify membership persists. |
| v0.5 | Drafts | Generate editable Drafts from selected Bundles, edit Drafts, confirm a Draft as a `KnowledgeItem` candidate. | Install on a phone, generate a Draft from a Bundle, edit it, confirm it, and verify the candidate is retained. |
| v0.6 | Knowledge Base | Persist `KnowledgeItem` records, embed/search KnowledgeItems, basic Knowledge Base list/detail/search. | Install on a phone, save a KnowledgeItem, search for it semantically, open details, and verify it remains after restart. |
| v0.7 | KnowledgeAreas | Manual and suggested `KnowledgeArea` assignment, create/rename/delete areas, correction events for area changes. | Install on a phone, assign a KnowledgeItem to an area, rename the area, move the item, and verify correction history is stored. |
| v0.8 | 2D Semantic Map | Phone-testable 2D map for `SemanticSketch` records, links, Bundles, KnowledgeItems, and KnowledgeAreas. | Install on a phone, open the map, pan/zoom, inspect points and groups, and verify map state reflects stored semantic records. |

## v0.1 boundary

Required in v0.1:

- mobile Flutter app shell;
- no account, no registration, local device storage only;
- startup on Sketch Editor;
- `ActiveSketchBuffer` autosave and restart restoration;
- create, list, edit, delete, and status-track `Sketch` records;
- Inbox screen;
- Settings screen for local storage, confirmations, data reset, and app information;
- one-tap explicit Send to Inbox with Undo;
- confirmation for clear, delete, clear Inbox, full local reset, and other destructive or batch actions;
- Android APK and iOS/Xcode build path.

Excluded from v0.1:

- local embeddings;
- cosine similarity;
- Outbox;
- `SemanticSketch`;
- semantic links;
- graph persistence;
- Bundles;
- Draft generation;
- KnowledgeItems;
- KnowledgeAreas;
- semantic search;
- 2D Semantic Map;
- sync;
- desktop app.

## Consequences

Positive:

- The first private mobile build becomes small enough to implement, install, and test quickly.
- Semantic workbench complexity is preserved as the target direction without blocking capture-first validation.
- Each later milestone adds one major user-facing capability.

Trade-offs:

- v0.1 does not prove local embeddings or semantic search.
- The full ADR-0005 loop arrives across several private pre-1.0 builds instead of the first installable build.
- Documentation must distinguish canonical semantic terminology from release scope.

Obligations:

- Do not describe v0.1 as requiring embeddings, cosine links, graph persistence, Bundles, Drafts, KnowledgeItems, KnowledgeAreas, or Semantic Map.
- Do not describe `v0.x` builds as MVP releases.
- Keep `ActiveSketchBuffer` and `Sketch` as the only v0.1 domain objects required by the user-facing product loop.
- Add semantic objects only in the release where they first become testable on a phone.

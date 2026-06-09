# ADR-0002: Voice/Text Inbox MVP and Application Split

## Status: Accepted, release and desktop scope superseded by ADR-0005, ADR-0010, and ADR-0011

ADR-0005 replaced the Voice/Text Inbox MVP with the Sketch/SemanticSketch target model. ADR-0010 then narrowed the first installable mobile release to v0.1 Mobile Capture + Inbox and staged semantic capabilities across later `v0.x` milestones. ADR-0014 introduces voice capture in v0.2 using OS speech APIs.

ADR-0011 reclassifies all `v0.x` milestones as private pre-1.0 mobile alpha/beta builds, defines `v1.0` as the first public mobile-only MVP/product baseline, and defers desktop app or desktop-owned semantic engine work until `v2.0+`.

## Context

MOPS was previously defined as a voice-first personal AI orchestration system with source-linked semantic memory, local-first storage, vector search, adaptive classification, and future AI-agent delegation.

The current design session narrowed the first shippable MVP to a smaller product slice:

```text
Voice/Text Inbox
  -> transcription
  -> cleaned note
  -> embedding
  -> semantic search
  -> suggested clusters
  -> manual correction
  -> project pages
```

The system must support chaotic, low-friction capture of short thoughts, ideas, and notes. The user should not manually organize content at capture time. Automatic organization is useful only if the user can inspect and correct it.

The former discussion also clarified that mobile and desktop have different platform constraints:

- Mobile platforms are harder for background processing, file access, permissions, audio, and local ML runtimes.
- Desktop platforms are better suited for heavy processing, indexing, debugging, local databases, and batch jobs.
- Product code should remain cohesive while low-level engines and external forks should not be mixed into the main product repository.

## Decision

The former first MVP direction was the **Voice/Text Inbox MVP**.

The former accepted MVP pipeline was:

```text
Voice/Text Inbox
  -> transcription
  -> cleaned note
  -> embedding
  -> semantic search
  -> suggested clusters
  -> manual correction
  -> project pages
```

Application roles are split as follows:

1. **Mobile app = capture client**
   - fast voice note capture;
   - fast text note capture;
   - local offline inbox;
   - speech-to-text entry point;
   - raw transcription storage;
   - lightweight cleaned note view;
   - lightweight semantic search;
   - suggested cluster review;
   - manual project correction;
   - compact project cards.

2. **Former desktop app = semantic memory engine**

   This was a former direction. Under ADR-0011, this desktop-owned semantic memory engine is not part of current `v0.x` private mobile builds or the `v1.x` mobile-only public product line. Desktop semantic engine ownership can be reconsidered only for `v2.0+`.

   - full inbox review;
   - heavy transcription and reprocessing;
   - cleaned note generation;
   - local semantic database;
   - embedding pipeline;
   - vector search;
   - hybrid search later;
   - cluster building;
   - project page generation;
   - manual correction workflows;
   - reindexing and model migration support.

3. **Shared product core lives in one monorepo**

```text
mops/
  apps/
    mobile/
    desktop/
  packages/
    core/
    db/
    ingestion/
    embeddings/
    clustering/
    search/
    sync/
    shared-types/
```

Mobile and desktop should share the same domain entities, storage contracts, ingestion contracts, embedding metadata, correction events, and project/cluster logic.

4. **Separate repositories are reserved for independent engines**

Potential future forks or standalone engines should not be placed inside the main product monorepo:

- custom vector database fork;
- custom embedding model fork;
- speech-to-text model fork;
- OCR or vision model fork;
- benchmark datasets;
- low-level native model/runtime experiments.

5. **Automation rule**

MOPS should suggest organization, not silently decide it.

Automatic cluster/project assignment must be represented as a suggestion with confidence and must support user confirmation, rejection, or correction.

6. **MVP exclusions**

The following capabilities are explicitly deferred from the first MVP:

- photo and image understanding;
- document ingestion;
- web URL scraping;
- browser extension;
- GitHub ingestion;
- full knowledge graph;
- custom vector DB fork;
- custom model training;
- all-platform launch at once.

## Consequences

Positive consequences:

- The MVP becomes narrow enough to implement and test.
- Mobile UX can focus on low-friction capture instead of heavy AI.
- Desktop may own complex processing only if this former direction is revived for `v2.0+`.
- Shared core prevents mobile and desktop logic from drifting apart.
- Manual correction creates training data for later adaptive behavior.
- The product remains compatible with the broader MOPS orchestration architecture.

Negative consequences and risks:

- The first MVP will not yet cover photos, documents, links, or full multimodal memory.
- Sync between mobile and desktop is still a major design problem.
- Mobile speech-to-text quality and correction UX remain critical risks.
- Automatic clustering may still create noisy or misleading suggestions.
- Desktop processing can become a bottleneck if a future `v2.0+` mobile/desktop workflow depends on it too strongly.
- Cross-platform mobile support should not be assumed to be cheap.

New obligations:

- Store raw and cleaned versions separately.
- Treat embeddings as search infrastructure, not semantic truth.
- Store user corrections as first-class events.
- Track processing status for each captured note.
- Keep project/cluster assignment editable.
- Keep framework choices separate from domain architecture.
- Start with one mobile platform if needed instead of forcing full iOS/Android parity immediately.

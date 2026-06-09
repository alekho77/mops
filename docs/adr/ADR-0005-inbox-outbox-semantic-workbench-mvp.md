# ADR-0005: Sketch, SemanticSketch, and Semantic Workbench MVP

## Status: Accepted, release scope superseded by ADR-0010 and release maturity superseded by ADR-0011

ADR-0010 supersedes the v0.1 release scope in this ADR. ADR-0011 clarifies that `v0.x` milestones are private pre-1.0 alpha/beta builds, not MVP releases. The terminology and target semantic workbench model remain accepted, but the full Sketch-to-Semantic-Map loop is delivered incrementally across the mobile train.

## Context

MOPS needs a small first product version that preserves the broader local-first semantic memory architecture without trying to ship every future capability at once.

The previous MVP direction was the Voice/Text Inbox MVP:

```text
Voice/Text Inbox
  -> transcription
  -> cleaned note
  -> embedding
  -> semantic search
  -> suggested Bundle and KnowledgeArea assignments
  -> manual correction
  -> project pages
```

The MVP is now clarified around a more explicit working model and terminology:

```text
Inbox = Sketch input buffer
Outbox = SemanticSketch workbench
Vector DB = long-term KnowledgeItem memory
```

The key clarification is that Outbox is not only a queue of notes waiting for storage. It is the user-visible semantic workspace where vectorized sketches, candidate links, bundles, and draft generation are reviewed before information becomes durable long-term knowledge.

Canonical terminology:

| Level | UI RU | Code term | Description |
| --- | --- | --- | --- |
| Inbox | Набросок | `Sketch` | Short raw text record. |
| Outbox | Обработанный набросок | `SemanticSketch` | Sketch after embedding processing. |
| Outbox cluster | Связка | `Bundle` | Group of related SemanticSketch records. |
| Generated document | Черновик | `Draft` | Coherent text assembled from a Bundle. |
| Vector DB document | Знание | `KnowledgeItem` | Finalized document in the long-term vector DB. |
| Vector DB cluster | Направление | `KnowledgeArea` | Large cluster of knowledge: project, topic, or semantic direction. |

## Decision

The target semantic workbench model is:

1. **Sketch**
   - Stores raw text notes and future voice-derived notes in the Inbox level.
   - Each Sketch has `id`, `text`, `created_at`, optional `tags`, and `status=inbox`.
   - Sketches are an unstructured stream of thoughts.

2. **SemanticSketch**
   - Stores vectorized sketches that are not finalized yet.
   - Sketches move from Inbox to Outbox after embedding.
   - Each processed sketch gets `status=outbox`.
   - SemanticSketch records live in the semantic workbench for reviewing notes, links, and bundles.

3. **Semantic Links**
   - Candidate relationships between SemanticSketch records.
   - Links are generated with cosine similarity over embeddings.
   - Users can confirm or delete links.
   - Confirmed and rejected links are stored as durable user correction events.

4. **Bundle**
   - Group of related SemanticSketch records built from the semantic link graph.
   - Users can merge bundles, split bundles, and manually attach a SemanticSketch to a bundle.
   - Bundles remain editable and must not be treated as final semantic truth.

5. **Draft**
   - A selected Bundle can be assembled into a coherent editable draft.
   - The user can manually edit the Draft.
   - Confirmation converts the Draft into a KnowledgeItem.

6. **KnowledgeItem**
   - Finalized documents stored in the main semantic DB and searchable vector space.
   - Each KnowledgeItem has its own embedding.
   - KnowledgeItems can be manually linked to a KnowledgeArea.

7. **KnowledgeArea**
   - Larger long-term clusters over KnowledgeItems.
   - New KnowledgeItems may strengthen an existing KnowledgeArea or create a candidate new one.
   - Automatic KnowledgeArea assignment remains a suggestion until confirmed by the user.

8. **Semantic Map**
   - 2D visualization for the MVP.
   - Points represent SemanticSketch records.
   - Lines represent semantic links.
   - Groups represent bundles.
   - Color or size can represent status, cluster density, or review state.

The accepted target flow is:

```text
Sketch
  -> Embedding Pipeline
  -> SemanticSketch
  -> Semantic Links
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> Vector DB
  -> KnowledgeArea
  -> Semantic Map
```

## Incremental Release Scope

The release scope is now defined by ADR-0010.

v0.1 is limited to Mobile Capture + Inbox:

- Flutter app shell;
- local-only and accountless startup;
- `ActiveSketchBuffer` autosave and restart restoration;
- create, list, edit, delete, and status-track Sketch records;
- Inbox and Settings;
- confirmations for destructive capture and Inbox actions;
- APK and iOS/Xcode build paths for phone testing.

The remaining semantic workbench capabilities are staged after v0.1:

- v0.2 introduces `SemanticSketch`, local embeddings, cosine similarity, and Outbox list/detail views;
- v0.3 introduces semantic links, link confirmation/deletion, correction events, and basic graph persistence;
- v0.4 introduces Bundles and Bundle editing;
- v0.5 introduces Draft generation and editing;
- v0.6 introduces KnowledgeItems and Knowledge Base search;
- v0.7 introduces KnowledgeAreas;
- v0.8 introduces the 2D Semantic Map.

## Former Oversized v0.1 Scope

The following scope is retained as the target semantic workbench capability set, not as the v0.1 requirement.

Target capabilities:

- create, list, edit, delete, and status-track Sketch records;
- manually process Sketch records into SemanticSketch records;
- compute local embeddings for sketches;
- list vectorized SemanticSketch records;
- show similar SemanticSketch records;
- find similar sketches with cosine similarity;
- confirm or delete semantic links manually;
- store the graph of semantic links;
- build Bundles from links;
- merge, split, and inspect Bundles;
- generate editable Draft records from selected Bundles;
- save confirmed Draft records as KnowledgeItems;
- embed KnowledgeItems;
- search similar KnowledgeItems;
- manually link KnowledgeItems to KnowledgeAreas;
- show a 2D Semantic Map of SemanticSketch records, Bundles, and links.

Deferred:

- full 3D globe;
- automatic creation of large projects without user confirmation;
- complex document hierarchy;
- multi-agent processing;
- device sync;
- collaborative work;
- automatic file import.

## Consequences

Positive:

- The target semantic workbench loop remains concrete and shippable across staged mobile releases.
- Outbox gives the user a place to inspect and correct SemanticSketch structure before long-term storage.
- Draft records create a clear bridge from chaotic sketches to durable knowledge.
- Manual confirmation remains central to Bundle and KnowledgeArea assignment.
- The target model still supports later voice capture, project crystallization, and agent orchestration.

Trade-offs:

- Graph persistence and cluster editing are required before the full semantic workbench is complete, but not in v0.1.
- Document generation enters in the Draft milestone, only from selected Bundles.
- The first semantic visualization remains a 2D map, not the future full 3D semantic globe.

Obligations:

- Keep raw Sketch records separate from vectorized SemanticSketch records and finalized KnowledgeItems.
- Store link confirmations, link deletions, bundle edits, and KnowledgeArea assignments as user correction events.
- Treat embeddings and vector search as candidate generators, not semantic truth.
- Keep vector indexes rebuildable from the semantic DB.
- Keep Outbox editable until the user confirms final KnowledgeItems.

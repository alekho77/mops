# ADR-0005: Sketch, SemanticSketch, and Semantic Workbench MVP

## Status: Accepted

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

The MVP v0.1 model is:

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

The accepted MVP flow is:

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

## MVP v0.1 Scope

Required:

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

- The first product loop becomes concrete and shippable.
- Outbox gives the user a place to inspect and correct SemanticSketch structure before long-term storage.
- Draft records create a clear bridge from chaotic sketches to durable knowledge.
- Manual confirmation remains central to Bundle and KnowledgeArea assignment.
- The MVP still supports later voice capture, project crystallization, and agent orchestration.

Trade-offs:

- The first MVP needs graph persistence and cluster editing earlier than a simple inbox-only app would.
- Document generation becomes part of MVP, but only from selected Bundles.
- The first visualization is a 2D map, not the future full 3D semantic globe.

Obligations:

- Keep raw Sketch records separate from vectorized SemanticSketch records and finalized KnowledgeItems.
- Store link confirmations, link deletions, bundle edits, and KnowledgeArea assignments as user correction events.
- Treat embeddings and vector search as candidate generators, not semantic truth.
- Keep vector indexes rebuildable from the semantic DB.
- Keep Outbox editable until the user confirms final KnowledgeItems.

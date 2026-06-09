# ADR-0005: Inbox, Outbox, and Semantic Workbench MVP

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
  -> suggested clusters
  -> manual correction
  -> project pages
```

The MVP is now clarified around a more explicit working model:

```text
Inbox = input buffer
Outbox = semantic workbench
Vector DB = long-term memory
```

The key clarification is that Outbox is not only a queue of notes waiting for storage. It is the user-visible semantic workspace where vectorized notes, candidate links, note clusters, and draft document generation are reviewed before information becomes durable long-term knowledge.

## Decision

The MVP v0.1 model is:

1. **Inbox**
   - Stores raw text notes and future voice-derived notes.
   - Each note has `id`, `text`, `created_at`, optional `tags`, and `status=inbox`.
   - Inbox is an unstructured stream of thoughts.

2. **Outbox**
   - Stores vectorized notes that are not finalized yet.
   - Notes move from Inbox to Outbox after embedding.
   - Each processed note gets `status=outbox`.
   - Outbox is the semantic workbench for reviewing notes, links, and clusters.

3. **Semantic Links**
   - Candidate relationships between notes.
   - Links are generated with cosine similarity over embeddings.
   - Users can confirm or delete links.
   - Confirmed and rejected links are stored as durable user correction events.

4. **Note Clusters**
   - Groups of related notes built from the semantic link graph.
   - Users can merge clusters, split clusters, and manually attach a note to a cluster.
   - Clusters remain editable and must not be treated as final semantic truth.

5. **Draft Documents**
   - A selected cluster can be assembled into a coherent editable draft.
   - The user can manually edit the draft.
   - Confirmation converts the draft into a Knowledge Document.

6. **Knowledge Documents**
   - Finalized documents stored in the main semantic DB and searchable vector space.
   - Each document has its own embedding.
   - Documents can be manually linked to a Project Cluster.

7. **Project Clusters**
   - Larger long-term clusters over Knowledge Documents.
   - New Knowledge Documents may strengthen an existing Project Cluster or create a candidate new one.
   - Automatic project assignment remains a suggestion until confirmed by the user.

8. **Semantic Map**
   - 2D visualization for the MVP.
   - Points represent notes.
   - Lines represent semantic links.
   - Groups represent note clusters.
   - Color or size can represent status, cluster density, or review state.

The accepted MVP flow is:

```text
Inbox Notes
  -> Embedding Pipeline
  -> Outbox Vector Space
  -> Semantic Links
  -> Note Clusters
  -> Draft Document Generation
  -> Knowledge Documents
  -> Vector DB
  -> Project Clusters
  -> Semantic Map
```

## MVP v0.1 Scope

Required:

- create, list, edit, delete, and status-track Inbox notes;
- manually move notes from Inbox to Outbox;
- compute local embeddings for notes;
- list vectorized Outbox notes;
- show similar notes;
- find similar notes with cosine similarity;
- confirm or delete semantic links manually;
- store the graph of semantic links;
- build note clusters from links;
- merge, split, and inspect clusters;
- generate editable Draft Documents from selected clusters;
- save confirmed drafts as Knowledge Documents;
- embed Knowledge Documents;
- search similar Knowledge Documents;
- manually link Knowledge Documents to projects;
- show a 2D Semantic Map of Outbox notes, clusters, and links.

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
- Outbox gives the user a place to inspect and correct semantic structure before long-term storage.
- Draft Documents create a clear bridge from chaotic notes to durable knowledge.
- Manual confirmation remains central to clustering and project assignment.
- The MVP still supports later voice capture, project crystallization, and agent orchestration.

Trade-offs:

- The first MVP needs graph persistence and cluster editing earlier than a simple inbox-only app would.
- Document generation becomes part of MVP, but only from selected note clusters.
- The first visualization is a 2D map, not the future full 3D semantic globe.

Obligations:

- Keep raw Inbox notes separate from vectorized Outbox notes and finalized Knowledge Documents.
- Store link confirmations, link deletions, cluster edits, and project assignments as user correction events.
- Treat embeddings and vector search as candidate generators, not semantic truth.
- Keep vector indexes rebuildable from the semantic DB.
- Keep Outbox editable until the user confirms final documents.

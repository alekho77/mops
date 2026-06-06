# ADR-0001: Separate Model Layer from Vector DB Layer

## Status: Accepted

## Context

MOPS uses semantic representations to connect user notes, documents, chunks, tasks, projects, and source references.

During the discussion, vector databases were clarified as infrastructure for storing, indexing, and searching existing vectors. They should not be treated as the component that creates semantic meaning.

Embedding or linguistic vector models produce vectors from source data. A vector database stores those vectors, indexes them, performs nearest-neighbor search, and returns ids or references to source-linked memory units.

MOPS needs to support multiple possible vector-producing models:

- external embedding models;
- local embedding models;
- custom linguistic vectors;
- future multimodal models.

MOPS also needs freedom to replace the vector storage/index implementation without changing semantic modeling logic.

## Decision

Separate vector production from vector storage/search.

MOPS architecture must distinguish three responsibilities:

1. Model Layer
   - Converts semantic input into vectors.
   - Owns linguistic, embedding, multimodal, and model-version concerns.
   - Determines semantic quality.

2. Vector DB Layer
   - Stores ready vectors.
   - Maintains indexes.
   - Searches nearest vectors by distance or similarity.
   - Returns vector ids, chunk ids, document ids, and metadata references.
   - Does not own semantic interpretation.

3. MOPS Semantic Engine
   - Decides what to parse, chunk, embed, store, search, and restore.
   - Connects vectors to source references.
   - Builds context from search results.
   - Coordinates model selection and vector store usage.

Required abstraction boundaries:

```ts
interface EmbeddingModel {
  embed(input: SemanticInput): Promise<Vector>
  embedBatch(inputs: SemanticInput[]): Promise<Vector[]>
}

interface VectorStore {
  upsert(items: VectorItem[]): Promise<void>
  search(queryVector: Vector, options: SearchOptions): Promise<SearchResult[]>
  delete(ids: string[]): Promise<void>
}
```

Canonical flow:

```text
Source Data
  -> Parser / Chunker
  -> EmbeddingModel
  -> VectorStore
  -> nearest vector results
  -> source-linked memory units
  -> MOPS Context Builder
```

## Consequences

Positive:

- Embedding models can be replaced without rewriting vector storage.
- Vector stores can be replaced without changing model logic.
- Custom linguistic vectors can coexist with standard embedding models.
- Vector index remains infrastructure, not semantic authority.
- Model metadata and embedding versions become explicit architectural concerns.

Negative:

- More interfaces must be maintained.
- Model/vector compatibility must be validated.
- Search quality depends on correct pairing of model, vector dimension, metric, and index.
- Migration is needed when embedding models change.

Risks:

- Mixing vectors from incompatible models may corrupt search quality.
- Treating vector DB results as semantic truth may hide model errors.
- Overfitting early abstractions to one vector DB or one embedding provider would weaken portability.

Obligations:

- Store model id, model version, vector dimension, distance metric, and creation timestamp with every vector.
- Keep vector index rebuildable.
- Keep source references outside the vector DB boundary.
- Treat nearest-neighbor results as candidates requiring contextual reconstruction.

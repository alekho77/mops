# Active Context

## Current stage

MOPS is in architecture and MVP definition stage.

## Recent decisions

- Accepted product name: **MOPS — Mind Orchestration & Planning System**.
- Accepted product identity: voice-first personal AI orchestration system.
- MOPS is not limited to task management or passive second brain behavior.
- Each user should have a personal trainable MOPS agent.
- The agent learns from user confirmations, corrections, manual task classification, goal decomposition edits, and execution results.
- Voice brain dump is a primary input flow.
- MOPS should index selected personal context: notes, documents, browser bookmarks, local/cloud files, and GitHub repositories.
- MOPS should automatically crystallize projects from accumulated thoughts and documents.
- MOPS should transform thoughts into goals, goals into subgoals, subgoals into tasks, and tasks into atomic actions.
- MOPS should prepare execution-ready packages for AI agents.
- Successful and failed task execution should feed back into future planning and classification.
- Three MVP slices are currently viable: Voice Task Manager MVP, Memory-first MVP, Agent-first MVP.
- Model Layer must be separated from Vector DB Layer: models produce embeddings/custom linguistic vectors, while vector DB stores ready vectors, indexes them, and searches nearest neighbors.
- MOPS Semantic Engine coordinates parsing, chunking, embedding, vector storage, search, source references, and context reconstruction.
- Vector DB is not semantic source of truth; it is searchable infrastructure over source-linked memory records.

## Current open choice

Select the first MVP slice:

1. Voice Task Manager MVP.
2. Memory-first MVP.
3. Agent-first MVP.

## Working constraint

Do not expand all capabilities at once. Keep the first MVP narrow enough to ship while preserving the full orchestration architecture.

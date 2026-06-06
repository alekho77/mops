# System Patterns

## Accepted system identity

MOPS is **Mind Orchestration & Planning System**.

It is a voice-first personal AI orchestration system, not only a task manager and not only a second brain.

## Core boundaries

- Individual-first.
- Local-first.
- Mobile-first capture.
- Privacy-first memory.
- Cloud as encrypted transport, not master backend.
- Semantic DB as durable source of truth.
- Vector index as rebuildable local cache.
- Raw files remain outside vector DB by default.

## Personal MOPS agent pattern

Each user has a personal trainable MOPS agent.

The agent:

- listens to user input;
- stores source-linked semantic memory;
- learns from confirmations and corrections;
- classifies thoughts and tasks;
- extracts projects and goals;
- decomposes goals into executable tasks;
- prepares tasks for AI agents;
- tracks execution results;
- adjusts future behavior from feedback.

## Voice-first command pattern

Voice is a primary interface, not an optional input method.

All core flows should support voice commands:

- capture;
- search;
- task creation;
- task correction;
- goal decomposition;
- planning;
- review;
- agent delegation.

## Source-linked memory pattern

MOPS stores semantic representations and source references, not full raw documents inside vector memory.

Stored memory units include:

- semantic chunks;
- summaries;
- embeddings;
- metadata;
- entities;
- semantic tags;
- relations;
- source references;
- model metadata.

Source Resolver remains responsible for restoring access to original files.

## Domain memory pattern

Memory is organized by semantic domain.

Initial domains:

- work;
- personal;
- projects;
- code;
- research;
- learning;
- finance;
- health;
- people;
- decisions.

Domains are not hard silos. Cross-domain relations are allowed.

## Project crystallization pattern

MOPS should extract projects from accumulated documents, notes, bookmarks, repositories, and thoughts.

Crystallization pipeline:

```text
raw memory
  -> topics
  -> candidate projects
  -> goals
  -> subgoals
  -> tasks
  -> atomic actions
```

## Adaptive classification pattern

Task classification should start with generic matrices and gradually adapt to the user.

Supported classification dimensions:

- urgent / important;
- impact / effort;
- MoSCoW;
- RICE;
- ICE;
- cost of delay;
- energy;
- context;
- risk;
- dependency;
- delegability;
- AI-agent readiness.

User corrections are training events.

## Agent task package pattern

Tasks delegated to AI agents must be converted into explicit work packages.

A package includes:

- goal;
- task statement;
- source context;
- relevant files or links;
- constraints;
- expected output;
- acceptance criteria;
- validation rules;
- permission scope;
- audit metadata.

## Feedback loop pattern

Execution history updates future behavior.

Tracked results:

- completed;
- failed;
- skipped;
- blocked;
- overdue;
- misclassified;
- incorrectly decomposed;
- successfully delegated;
- failed delegation.

Feedback affects prioritization, decomposition, planning, and delegation decisions.

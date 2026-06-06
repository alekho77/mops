# MOPS Core Concept Architecture

## Product definition

MOPS is **Mind Orchestration & Planning System**.

It is a voice-first personal AI orchestration system for one user and their personal devices.

Each user has a personal trainable MOPS agent that learns from their decisions, corrections, task classifications, and execution history.

## Core system layers

```text
Voice Interface
  -> Capture and Command Router
  -> Semantic Memory Layer
  -> Personal Adaptation Layer
  -> Goal and Task Orchestration Layer
  -> Agent Delegation Layer
  -> Review and Feedback Loop
```

## Voice Interface

Primary input and control surface.

Responsibilities:

- voice brain dump;
- voice commands;
- task creation and updates;
- planning and review dialogs;
- confirmation and correction flows;
- voice search over personal memory.

## Capture and Command Router

Converts user input into typed intents.

Supported intent groups:

- capture thought;
- create task;
- update task;
- search memory;
- classify task;
- create goal;
- decompose goal;
- prepare agent task;
- review progress;
- correct MOPS decision.

## Semantic Memory Layer

Durable source-linked memory.

Inputs:

- voice notes;
- text notes;
- browser bookmarks;
- selected browser history;
- documents from local devices;
- documents from cloud folders;
- GitHub repositories;
- project notes;
- task history;
- decisions and corrections.

Stored data:

- semantic chunks;
- summaries;
- embeddings;
- entities;
- tags;
- relations;
- source references;
- domain references;
- project references;
- model metadata.

Original files remain outside the vector DB.

Vector indexes are rebuildable caches.

## Personal Adaptation Layer

Learns from user behavior.

Training signals:

- task confirmation;
- task rejection;
- manual priority changes;
- manual urgent/important classification;
- project assignment changes;
- goal decomposition corrections;
- successful task execution;
- failed task execution;
- user feedback after review.

Predicted properties:

- task type;
- project;
- domain;
- urgency;
- importance;
- effort;
- impact;
- priority;
- deadline risk;
- delegation potential;
- AI-agent readiness;
- next action.

## Goal and Task Orchestration Layer

Transforms unstructured input into executable structure.

Pipeline:

```text
Raw thoughts
  -> candidate topics
  -> candidate projects
  -> goals
  -> subgoals
  -> tasks
  -> atomic actions
```

Task classification supports multiple matrices:

- urgent / important;
- impact / effort;
- MoSCoW;
- RICE;
- ICE;
- cost of delay;
- energy required;
- context;
- dependency on other people;
- delegation suitability;
- AI-agent suitability.

## Agent Delegation Layer

Prepares execution-ready work packages for AI agents.

Each agent task package should include:

- task statement;
- source context from memory;
- relevant documents and links;
- constraints;
- expected output;
- acceptance criteria;
- validation instructions;
- write permissions if needed;
- audit metadata.

## Review and Feedback Loop

Feeds execution results back into memory and adaptation.

Tracked signals:

- completed tasks;
- failed tasks;
- skipped tasks;
- overdue tasks;
- wrong classifications;
- bad decompositions;
- useful decompositions;
- successful agent outputs;
- failed agent outputs.

Outputs:

- daily review;
- weekly review;
- project review;
- correction suggestions;
- priority adjustments;
- model behavior updates.

## MVP candidate slices

### Voice Task Manager MVP

- voice brain dump;
- speech-to-text;
- task extraction;
- urgent/important classification;
- user confirmation;
- learning from corrections.

### Memory-first MVP

- import selected documents, notes, bookmarks, and GitHub repositories;
- vector indexing;
- semantic search;
- answers with source references;
- automatic project extraction.

### Agent-first MVP

- intent capture;
- goal decomposition;
- task decomposition;
- AI-agent task package generation;
- basic result validation.

## Hard boundaries

- No team workspace scope.
- No enterprise knowledge base scope.
- No cloud backend as mandatory master.
- No raw-file storage inside vector DB by default.
- No blind indexing of all user data.
- No autonomous destructive actions without explicit permission and audit trail.

# ADR-0001: Voice-First Personal Orchestration System

## Status: Accepted

## Context

MOPS started as a personal local-first semantic memory system with vector search, short semantic chunks, source references, encrypted sync, and mobile-first capture.

The product scope was expanded in the current design session:

- MOPS means **Mind Orchestration & Planning System**.
- Each user has a personal MOPS agent.
- The agent is trainable and adapts to the user's decisions.
- Voice is the primary interaction model.
- The system stores and indexes thoughts, notes, documents, bookmarks, repositories, and other personal context.
- The system must extract projects, goals, subgoals, tasks, and execution-ready actions from unstructured user input.
- The system should prepare tasks for AI agents and track successful vs failed execution.

This changes the product from a task manager or passive second brain into a personal orchestration layer.

## Decision

MOPS is defined as a **voice-first personal AI orchestration system** built around four core capabilities:

1. **Voice command and capture layer**
   - Full voice control for capture, planning, task updates, search, review, and confirmation.
   - Voice brain dump as the primary input path for daily thoughts and decisions.

2. **Unified vector memory layer**
   - Vector indexing for user documents, notes, browser bookmarks, selected files, cloud documents, and GitHub repositories.
   - Semantic memory remains source-linked: original files stay outside the vector DB.
   - Memory is organized by domains, projects, topics, people, goals, tasks, and decisions.

3. **Adaptive personal model layer**
   - The system learns from user confirmations, corrections, manual task classification, and daily decisions.
   - It predicts task category, priority, urgency, importance, effort, context, delegation potential, and AI-agent readiness.
   - User-specific behavior is preferred over generic productivity rules.

4. **Goal and task orchestration layer**
   - Raw thoughts are converted into candidate projects and goals.
   - Goals are decomposed into subgoals, tasks, and atomic actions.
   - Tasks can be prepared for AI agents with context, expected output, and acceptance criteria.
   - Execution results are tracked and used for future corrections.

The first MVP should be selected from one of three viable slices:

1. **Voice Task Manager MVP**
   - Voice brain dump.
   - Automatic task extraction.
   - Basic urgent/important classification.
   - User confirmation and correction loop.

2. **Memory-first MVP**
   - Import documents, notes, bookmarks, and GitHub repositories.
   - Vector memory and semantic search.
   - Answers with source references.
   - Automatic project and goal extraction.

3. **Agent-first MVP**
   - Text or voice intent input.
   - Thought → goal → subgoal → task decomposition.
   - AI-agent task preparation with context and done criteria.
   - Basic result validation loop.

## Consequences

Positive consequences:

- The product has a clear identity beyond a task manager: a personal AI orchestration system.
- The MOPS brand supports a personal companion model: every user trains their own MOPS.
- Voice-first capture matches the mobile-first direction.
- Vector memory becomes useful as operational context, not only passive search.
- Task classification and planning become adaptive to the user.
- Goal decomposition creates a bridge from personal memory to AI-agent execution.

Negative consequences and risks:

- Scope is much larger than a classic task manager or second brain.
- Voice-first UX requires high-quality speech recognition, correction, and confirmation flows.
- Adaptive behavior needs careful feedback design to avoid wrong automation.
- Automatic project extraction can create noise if memory quality is low.
- AI-agent delegation requires explicit permissions, audit logs, and result validation.
- The system must avoid becoming a blind indexer of all user data.
- Privacy and local-first constraints become harder as integrations grow.

New obligations:

- Keep source-linked memory as a hard architectural boundary.
- Treat user confirmation and correction as first-class training events.
- Version model behavior, embeddings, task classifiers, and decomposition rules.
- Maintain auditability for automated changes and agent delegation.
- Design every automation path with manual override and rollback.
- Keep MVP slices narrow enough to ship independently.

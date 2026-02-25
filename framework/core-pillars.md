---
title: "Core Pillars"
description: "The four architectural pillars that enable safe, scalable AI-assisted software delivery"
authors:
  - dpavancini
lastUpdated: "2026-02-24"
---

## Overview

The Agentic Development Framework rests on four architectural pillars that work together to enable safe, scalable AI-assisted software delivery. Each pillar addresses a different dimension of the challenge: what agents know, where they run, how they are governed, and how work is routed.

## Pillar 1: Context-First Architecture

[[context-engineering|Context Engineering]] is the foundation of everything else. In agentic development, context is code — it is the primary input that determines the quality, correctness, and speed of every agent execution.

### Context Clarity as the Primary Constraint

Traditional development is constrained by developer time. Agentic development is constrained by context clarity. When agents have precise, complete, and well-structured context, they execute reliably and fast. When context is vague or incomplete, they produce hallucinations, miss edge cases, or loop unproductively.

This means the [[context-window]] is not just a technical limitation of language models — it is an architectural constraint that shapes how you decompose work, structure specifications, and organize knowledge.

### The Context Index

The Context Index is the structured knowledge base that agents draw from during execution. It includes:

- **Live Specs** — Machine-readable specifications for current work items
- **System Constitution** — Architectural principles, coding standards, security policies
- **Codebase context** — Type definitions, API schemas, test suites, documentation
- **Historical context** — Past decisions, resolved blockers, lessons learned

The Context Index serves as the agent's persistent knowledge base. The richer and more structured it is, the more autonomously the agent can operate. For a full treatment of the Context Index, retrieval patterns, and hygiene practices, see [Context Management](/en/handbook/infrastructure/context-management).

Organizations that adopt [[rag|RAG]] patterns to surface relevant context dynamically can further reduce the burden on the Context Architect and increase the proportion of tasks that agents handle autonomously.

## Pillar 2: Ephemeral Infrastructure

Every agent execution happens inside an Ephemeral Workbench — an isolated, secure, disposable environment that contains everything the agent needs and nothing it does not.

### Workbenches as Isolated MicroVMs

An Ephemeral Workbench is a sandboxed execution environment provisioned on demand and destroyed after use. Each workbench:

- Contains a clean copy of the relevant codebase and dependencies
- Has access only to the context and tools specified in the task's Live Spec
- Cannot modify shared state, production systems, or other agents' workbenches
- Produces artifacts (code changes, test results, logs) that are extracted before teardown

This model eliminates an entire class of risks: agents cannot accidentally corrupt shared environments, leak secrets across tasks, or create persistent side effects.

### Security by Design

Ephemeral infrastructure makes security a structural property rather than a policy overlay:

- **Blast radius containment** — If an agent does something wrong, the damage is limited to a disposable environment
- **No persistent access** — Agents never hold long-lived credentials or sessions
- **Auditable by default** — Every workbench execution produces a complete, reproducible log
- **[[prompt-injection]] resistance** — Isolated environments limit what an attacker can achieve even if they compromise an agent's input

## Pillar 3: Gate-Based Governance

Agentic development replaces informal code review with a structured system of automated and human-in-the-loop checkpoints.

### Continuous Automated Gates

These gates run on every agent execution without human involvement:

- **Security scans** — Static analysis, dependency vulnerability checks, secret detection
- **Eval Harness** — The spec-defined acceptance criteria, executed as automated tests
- **Architectural conformance** — Checks that changes follow established patterns and do not introduce forbidden dependencies
- **Performance baselines** — Automated benchmarks that catch regressions

The Eval Harness is the centerpiece. It transforms acceptance criteria from a document that humans read into executable checks that agents must pass. This is the mechanism that makes autonomous execution trustworthy.

### Mandatory HITL Gates

Some decisions are too consequential for full automation. Mandatory human-in-the-loop gates include:

- **Security-critical changes** — Authentication flows, authorization logic, data encryption
- **Architectural decisions** — New service boundaries, database schema changes, API contracts
- **High-blast-radius deployments** — Changes that affect all users or critical infrastructure
- **Novel patterns** — First-time implementations of approaches not covered by existing specs

The key insight is that [[hallucination]] risk is not uniform across all tasks. Gate-Based Governance allocates human attention where it matters most, rather than spreading it thinly across everything.

## Pillar 4: Hybrid Engineering

Hybrid Engineering is the practice of routing each task to the most efficient resource — whether that is an autonomous agent, an assisted agent, or a human engineer.

### Auto-Agents for Standard Tasks

Tasks that are well-defined, have existing patterns, and carry low risk are ideal for fully autonomous execution. Examples include:

- Implementing CRUD endpoints from API specs
- Writing unit tests for existing functions
- Applying refactoring patterns across a codebase
- Generating documentation from code and type definitions

### Human Engineers for Complex Work

Tasks that involve high ambiguity, novel architecture, or significant judgment calls remain with human engineers. Examples include:

- Designing new system architectures
- Resolving conflicting requirements
- Making security trade-off decisions
- Handling novel problem domains with no existing patterns

### The Routing Decision

The Orchestration System routes tasks based on a combination of factors:

- **Spec completeness** — How well-defined are the acceptance criteria?
- **Pattern coverage** — Does the Context Index contain relevant precedents?
- **Blast radius** — What is the potential impact of a wrong implementation?
- **Novelty** — Is this a variation of a known pattern or something entirely new?

Over time, as the Context Index grows and specs become more precise, the proportion of auto-agent tasks increases. This is how agentic teams scale: not by hiring more engineers, but by expanding the boundary of what agents can handle autonomously.

## Next Steps

These four pillars provide the structural foundation. The next page covers [Spec-Driven Development](/en/handbook/framework/spec-driven-development), the practice that replaces informal user stories with machine-readable specifications — the primary input that makes these pillars operational.

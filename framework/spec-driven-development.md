---
title: "Spec-Driven Development"
description: "How machine-readable specifications replace user stories as the unit of work in agentic development"
authors:
  - dpavancini
lastUpdated: "2026-02-24"
---

## Overview

Spec-Driven Development replaces informal user stories and ticket descriptions with machine-readable specifications that serve as deterministic contracts between humans and agents. It is the practice that makes the four Core Pillars operational — without precise specs, context is incomplete, governance gates have nothing to check against, and routing decisions lack the information they need.

## What is a Spec?

A spec is a machine-readable, deterministic contract that defines:

- **What** needs to be built (behavioral requirements)
- **Why** it matters (business context and constraints)
- **How to verify** it works (executable acceptance criteria)

Unlike a user story ("As a user, I want..."), a spec leaves no room for interpretation. It is precise enough that an agent can implement it without asking clarifying questions and verify its own work against the acceptance criteria.

## The Three Minimum Assets

Every Live Spec must contain at least three assets:

1. **Behavioral Contract** — A precise description of the expected behavior, including inputs, outputs, edge cases, and error handling. This is what the agent implements.

2. **System Constitution** — The [[system-prompt]] and constraints that govern how the agent operates. This includes coding standards, architectural patterns, security policies, and any domain-specific rules. It defines the boundaries of acceptable solutions.

3. **Actionable Task Map** — A decomposed, ordered list of implementation steps that the agent can follow. Each step references the relevant section of the Behavioral Contract and includes its own acceptance criteria.

## Similarities to TDD

Spec-Driven Development shares DNA with Test-Driven Development (TDD). Both approaches define the expected behavior before writing the implementation. The key differences:

- **Scope** — TDD specs are function-level; Live Specs cover entire features or workflows
- **Audience** — TDD tests are written for test runners; Live Specs are written for agents and humans
- **Richness** — Live Specs include architectural context, rationale, and decomposition that goes beyond what a test file captures

Teams already practicing TDD will find the transition to Spec-Driven Development natural. The discipline of "define the expected behavior first" is the same — the format and scope expand.

## Alternative Frameworks

Several frameworks have emerged to structure spec-driven workflows:

- **BMAD Method** — A comprehensive framework for defining agent tasks with structured prompts and behavioral specifications
- **GitHub Spec Kit** — GitHub's approach to structuring specifications for Copilot-driven workflows
- **Tessl** — A platform-level approach to spec-driven agent orchestration

Each takes a different angle, but all share the core principle: agents need structured, machine-readable input to produce reliable output.

## Challenges

Adopting spec-driven workflows is not without friction:

- **Perception of lost velocity** — Writing detailed specs feels slower than "just coding." Teams must internalize that the spec is the work, not overhead before the work.
- **Maintenance trap** — Specs that are not kept in sync with the codebase become misleading. Stale specs are worse than no specs because agents follow them faithfully.
- **Translation gap** — Business stakeholders think in outcomes; agents need precise technical contracts. Bridging this gap is the Context Architect's core skill.
- **Context degradation** — As systems grow, the total context required to spec a change can exceed what fits in a single context window. Modular spec design and RAG-based context retrieval mitigate this.
- **Semantic ambiguity** — Natural language is inherently ambiguous. Even well-written specs can be interpreted differently by different agents or model versions. Executable acceptance criteria are the antidote.

## The Live Spec Concept

A Live Spec is not a static document. It is a version-controlled, modular blueprint that evolves alongside the codebase:

- **Version-controlled** — Specs live in the repository alongside the code they describe, with full Git history
- **Modular** — Large features are decomposed into multiple specs that reference each other
- **Executable** — Acceptance criteria are written as automated checks, not prose descriptions
- **Evolving** — Specs are updated as requirements change, implementation reveals new edge cases, or the system architecture shifts

This approach is an evolution of prompt-driven development, moving from ad-hoc prompts to structured, persistent specifications that accumulate institutional knowledge.

## The Continuous Development Loop

The four Core Pillars and Spec-Driven Development come together in the Continuous Development Loop (CDL) — the agentic evolution of CI/CD that automates the entire lifecycle from spec to production. The CDL extends the traditional build-test-deploy pipeline with spec injection, automated context assembly, the Eval Harness as the primary quality gate, and knowledge-centric feedback loops that enrich the Context Index after every execution cycle.

For a detailed breakdown of CDL phases, their CI/CD equivalents, and key architectural enhancements, see [From Agile to Agentic](/en/handbook/framework/from-agile-to-agentic).

## Next Steps

With the architectural pillars and spec-driven practices defined, the next page explores how these concepts map to familiar Agile ceremonies and roles. [From Agile to Agentic](/en/handbook/framework/from-agile-to-agentic) covers the comparison table, the transition strategy, and the evolution of coding agents that makes this shift possible.

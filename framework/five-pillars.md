---
title: "Core Pillars and Spec-Driven Development"
description: "The four architectural pillars of Agentic Development and how specs replace user stories as the unit of work"
authors:
  - dpavancini
lastUpdated: "2026-02-23"
---

## Overview

The Agentic Development Framework rests on four architectural pillars that work together to enable safe, scalable AI-assisted software delivery. These pillars are complemented by Spec-Driven Development, a practice that replaces informal user stories with machine-readable specifications, and the Continuous Development Loop (CDL), an evolution of CI/CD designed for agent-driven workflows.

## Pillar 1: Context-First Architecture

[[context-engineering|Context Engineering]] is the foundation of everything else. In agentic development, context is code -- it is the primary input that determines the quality, correctness, and speed of every agent execution.

### Context Clarity as the Primary Constraint

Traditional development is constrained by developer time. Agentic development is constrained by context clarity. When agents have precise, complete, and well-structured context, they execute reliably and fast. When context is vague or incomplete, they produce hallucinations, miss edge cases, or loop unproductively.

This means the [[context-window]] is not just a technical limitation of language models -- it is an architectural constraint that shapes how you decompose work, structure specifications, and organize knowledge.

### The Context Index

The Context Index is the structured knowledge base that agents draw from during execution. It includes:

- **Live Specs** -- Machine-readable specifications for current work items
- **System Constitution** -- Architectural principles, coding standards, security policies
- **Codebase context** -- Type definitions, API schemas, test suites, documentation
- **Historical context** -- Past decisions, resolved blockers, lessons learned

Think of the Context Index as the agent's institutional memory. The richer and more structured it is, the more autonomously the agent can operate.

Organizations that adopt [[rag|RAG]] patterns to surface relevant context dynamically can further reduce the burden on the Context Architect and increase the proportion of tasks that agents handle autonomously.

## Pillar 2: Ephemeral Infrastructure

Every agent execution happens inside an Ephemeral Workbench -- an isolated, secure, disposable environment that contains everything the agent needs and nothing it does not.

### Workbenches as Isolated MicroVMs

An Ephemeral Workbench is a sandboxed execution environment provisioned on demand and destroyed after use. Each workbench:

- Contains a clean copy of the relevant codebase and dependencies
- Has access only to the context and tools specified in the task's Live Spec
- Cannot modify shared state, production systems, or other agents' workbenches
- Produces artifacts (code changes, test results, logs) that are extracted before teardown

This model eliminates an entire class of risks: agents cannot accidentally corrupt shared environments, leak secrets across tasks, or create persistent side effects.

### Security by Design

Ephemeral infrastructure makes security a structural property rather than a policy overlay:

- **Blast radius containment** -- If an agent does something wrong, the damage is limited to a disposable environment
- **No persistent access** -- Agents never hold long-lived credentials or sessions
- **Auditable by default** -- Every workbench execution produces a complete, reproducible log
- **[[prompt-injection]] resistance** -- Isolated environments limit what an attacker can achieve even if they compromise an agent's input

## Pillar 3: Gate-Based Governance

Agentic development replaces informal code review with a structured system of automated and human-in-the-loop checkpoints.

### Continuous Automated Gates

These gates run on every agent execution without human involvement:

- **Security scans** -- Static analysis, dependency vulnerability checks, secret detection
- **Eval Harness** -- The spec-defined acceptance criteria, executed as automated tests
- **Architectural conformance** -- Checks that changes follow established patterns and do not introduce forbidden dependencies
- **Performance baselines** -- Automated benchmarks that catch regressions

The Eval Harness is the centerpiece. It transforms acceptance criteria from a document that humans read into executable checks that agents must pass. This is the mechanism that makes autonomous execution trustworthy.

### Mandatory HITL Gates

Some decisions are too consequential for full automation. Mandatory human-in-the-loop gates include:

- **Security-critical changes** -- Authentication flows, authorization logic, data encryption
- **Architectural decisions** -- New service boundaries, database schema changes, API contracts
- **High-blast-radius deployments** -- Changes that affect all users or critical infrastructure
- **Novel patterns** -- First-time implementations of approaches not covered by existing specs

The key insight is that [[hallucination]] risk is not uniform across all tasks. Gate-Based Governance allocates human attention where it matters most, rather than spreading it thinly across everything.

## Pillar 4: Hybrid Engineering

Hybrid Engineering is the practice of routing each task to the most efficient resource -- whether that is an autonomous agent, an assisted agent, or a human engineer.

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

- **Spec completeness** -- How well-defined are the acceptance criteria?
- **Pattern coverage** -- Does the Context Index contain relevant precedents?
- **Blast radius** -- What is the potential impact of a wrong implementation?
- **Novelty** -- Is this a variation of a known pattern or something entirely new?

Over time, as the Context Index grows and specs become more precise, the proportion of auto-agent tasks increases. This is how agentic teams scale: not by hiring more engineers, but by expanding the boundary of what agents can handle autonomously.

## Spec-Driven Development

Spec-Driven Development replaces informal user stories and ticket descriptions with machine-readable specifications that serve as deterministic contracts between humans and agents.

### What is a Spec?

A spec is a machine-readable, deterministic contract that defines:

- **What** needs to be built (behavioral requirements)
- **Why** it matters (business context and constraints)
- **How to verify** it works (executable acceptance criteria)

Unlike a user story ("As a user, I want..."), a spec leaves no room for interpretation. It is precise enough that an agent can implement it without asking clarifying questions and verify its own work against the acceptance criteria.

### The Three Minimum Assets

Every Live Spec must contain at least three assets:

1. **Behavioral Contract** -- A precise description of the expected behavior, including inputs, outputs, edge cases, and error handling. This is what the agent implements.

2. **System Constitution** -- The [[system-prompt]] and constraints that govern how the agent operates. This includes coding standards, architectural patterns, security policies, and any domain-specific rules. It defines the boundaries of acceptable solutions.

3. **Actionable Task Map** -- A decomposed, ordered list of implementation steps that the agent can follow. Each step references the relevant section of the Behavioral Contract and includes its own acceptance criteria.

### Similarities to TDD

Spec-Driven Development shares DNA with Test-Driven Development (TDD). Both approaches define the expected behavior before writing the implementation. The key differences:

- **Scope** -- TDD specs are function-level; Live Specs cover entire features or workflows
- **Audience** -- TDD tests are written for test runners; Live Specs are written for agents and humans
- **Richness** -- Live Specs include architectural context, rationale, and decomposition that goes beyond what a test file captures

Teams already practicing TDD will find the transition to Spec-Driven Development natural. The discipline of "define the expected behavior first" is the same -- the format and scope expand.

### Alternative Frameworks

Several frameworks have emerged to structure spec-driven workflows:

- **BMAD Method** -- A comprehensive framework for defining agent tasks with structured prompts and behavioral specifications
- **GitHub Spec Kit** -- GitHub's approach to structuring specifications for Copilot-driven workflows
- **Tessl** -- A platform-level approach to spec-driven agent orchestration

Each takes a different angle, but all share the core principle: agents need structured, machine-readable input to produce reliable output.

### Challenges of Spec-Driven Development

Adopting spec-driven workflows is not without friction:

- **Perception of lost velocity** -- Writing detailed specs feels slower than "just coding." Teams must internalize that the spec is the work, not overhead before the work.
- **Maintenance trap** -- Specs that are not kept in sync with the codebase become misleading. Stale specs are worse than no specs because agents follow them faithfully.
- **Translation gap** -- Business stakeholders think in outcomes; agents need precise technical contracts. Bridging this gap is the Context Architect's core skill.
- **Context degradation** -- As systems grow, the total context required to spec a change can exceed what fits in a single context window. Modular spec design and RAG-based context retrieval mitigate this.
- **Semantic ambiguity** -- Natural language is inherently ambiguous. Even well-written specs can be interpreted differently by different agents or model versions. Executable acceptance criteria are the antidote.

### The Live Spec Concept

A Live Spec is not a static document. It is a version-controlled, modular blueprint that evolves alongside the codebase:

- **Version-controlled** -- Specs live in the repository alongside the code they describe, with full Git history
- **Modular** -- Large features are decomposed into multiple specs that reference each other
- **Executable** -- Acceptance criteria are written as automated checks, not prose descriptions
- **Evolving** -- Specs are updated as requirements change, implementation reveals new edge cases, or the system architecture shifts

This approach is an evolution of [[prompt-driven-development]], moving from ad-hoc prompts to structured, persistent specifications that accumulate institutional knowledge.

## The Continuous Development Loop

The Continuous Development Loop (CDL) is the agentic evolution of CI/CD. Where CI/CD automates build, test, and deploy, CDL automates the entire development lifecycle from spec to production.

### Mapping CDL Phases to CI/CD

| CDL Phase | CI/CD Equivalent | What Changes |
|-----------|-----------------|--------------|
| Spec Injection | Ticket creation / branch start | Machine-readable spec replaces informal ticket |
| Context Assembly | Developer reads docs / codebase | Agent automatically gathers relevant context from Context Index |
| Autonomous Execution | Developer writes code | Agent implements in isolated Workbench |
| Eval Harness | CI test suite | Expanded to include behavioral, security, and architectural checks |
| Gate Review | Code review / PR approval | Structured automated + HITL gates replace ad-hoc review |
| Context Update | Knowledge base / docs update | Automatic enrichment of Context Index from execution results |
| Deployment | CD pipeline | Unchanged -- existing deployment infrastructure works as-is |

### Key Architectural Enhancements

The CDL introduces three capabilities that CI/CD lacks:

1. **Integrated Security** -- Security is not a separate pipeline stage but a continuous check woven into every phase. From spec validation (does this spec introduce security risks?) through execution (does the agent's code pass security scans?) to deployment (are runtime security policies satisfied?).

2. **Eval Harness as the New CI** -- The Eval Harness replaces the traditional CI test suite as the primary quality gate. It includes not just unit and integration tests but also behavioral verification, [[plan-and-execute]] trace validation, and architectural conformance checks.

3. **Knowledge-Centric Feedback Loops** -- Every CDL cycle produces structured feedback that enriches the Context Index. Failed evaluations, operator interventions, and successful patterns all become searchable, reusable knowledge. This is the mechanism that enables continuous improvement in [[llmops]] maturity.

## Next Steps

These pillars and practices provide the architectural foundation. The next page, [From Agile to Agentic](/en/handbook/framework/from-agile-to-agentic), explores how these concepts map to familiar Agile ceremonies and roles, and how teams can make the transition.

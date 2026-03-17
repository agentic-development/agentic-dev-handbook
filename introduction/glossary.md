---
title: "Framework Glossary"
description: "Quick-reference definitions for key terms used throughout the handbook"
authors:
  - dpavancini
lastUpdated: "2026-02-25"
---

This glossary defines the key terms introduced in the handbook. Each entry includes a short definition, a link to the full glossary entry on the site, and a reference to the handbook page where the concept is explained in detail.

## Terms

**[[agentops-dashboard|AgentOps Dashboard]]** — The real-time monitoring interface that tracks agent execution status, pipeline health, and compute costs across a squad. The Flow Manager reviews it during the Daily Flow Sync.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

**[[blocker-flag|Blocker Flag]]** — A signal raised by an agent when it encounters ambiguity or a constraint it cannot resolve, triggering escalation to a human operator who executes an Agent Recovery.
Handbook: [The Orchestration Layer](/en/handbook/framework/orchestration-layer)

**[[context-engineering|Context Engineering]]** — The practice of designing, organizing, and managing the information that flows into an LLM's context window. Takes a systems-level view of how system prompts, documents, history, and tools are assembled and prioritized.
Handbook: [Context Management](/en/handbook/infrastructure/context-management)

**[[context-index|Context Index]]** — The structured knowledge base that agents draw from during execution. Contains Live Specs, architectural rules, Golden Samples, domain glossaries, and historical context, organized for machine consumption.
Handbook: [Context Management](/en/handbook/infrastructure/context-management)

**[[context-packet|Context Packet]]** — A bundled collection of specifications, architectural rules, Golden Samples, and domain context assembled for a specific task. Created during weekly Specification Engineering Blocks and consumed during daily agent execution.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

**[[continuous-development-loop|Continuous Development Loop]]** — The agentic evolution of CI/CD that automates the entire lifecycle from spec to production. Extends the traditional pipeline with spec injection, automated context assembly, Eval Harness gates, and knowledge feedback loops.
Handbook: [From Agile to Agentic](/en/handbook/framework/from-agile-to-agentic)

**[[human-owned-core|Human-Owned Core]]** — Code that is too architecturally sensitive or security-critical for agent execution. Written exclusively by human Agent Operators, it includes authentication paths, core algorithms, and foundational abstractions.
Handbook: [The Hybrid Squad](/en/handbook/team-model/hybrid-squad)

**[[eval-harness|Evaluation Harness]]** — The automated test suite that validates every agent output before it reaches a human reviewer. Combines functional tests, security scans, architectural conformance checks, and LLM-as-a-Judge evaluations.
Handbook: [Evaluation Harness](/en/handbook/infrastructure/evaluation-harness)

**[[golden-samples|Golden Samples]]** — Curated reference implementations that agents use as templates for new code. A well-chosen golden sample teaches an agent more than pages of written instructions by demonstrating expected patterns through concrete example.
Handbook: [The Hybrid Squad](/en/handbook/team-model/hybrid-squad)

**[[human-in-the-loop|Human-in-the-Loop]]** — A design principle where human judgment is required at critical decision points before an agent proceeds with consequential actions. Enables graduated autonomy through tiered permission models.
Handbook: [The Orchestration Layer](/en/handbook/framework/orchestration-layer)

**[[live-spec|Live Spec]]** — A machine-readable, deterministic contract that defines what to build, why it matters, and how to verify it works. Contains a Behavioral Contract, System Constitution, and Actionable Task Map. Evolves alongside the codebase under version control.
Handbook: [Spec-Driven Development](/en/handbook/framework/spec-driven-development)

**[[agent-recovery|Agent Recovery]]** — The intervention workflow where an Agent Operator diagnoses a stuck agent, injects missing context, and resumes execution. Follows a three-step process: Diagnose, Inject, Resume.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

**[[spec-driven-development|Spec-Driven Development]]** — The practice of replacing informal user stories with machine-readable specifications that serve as deterministic contracts between humans and agents. Makes the core pillars operational by providing precise inputs for context, governance, and routing.
Handbook: [Spec-Driven Development](/en/handbook/framework/spec-driven-development)

**[[token-budget|Token Budget]]** — The maximum compute spend authorized per time period, functioning as a hard constraint that prevents runaway agent loops. Enforced at the Orchestration Layer and tracked on the AgentOps Dashboard.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

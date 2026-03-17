---
title: "Roles in Agentic Development"
description: "Six role definitions for the Agentic Development team — from Context Architect to Principal Systems Architect"
authors:
  - dpavancini
lastUpdated: "2026-02-23"
---

## Overview

Agentic development does not eliminate engineering roles — it transforms them. Traditional titles like "Product Manager" and "QA Engineer" persist in name but shift dramatically in practice. This page defines six roles purpose-built for the Hybrid Squad, maps them from their traditional counterparts, and details the day-to-day responsibilities and skills each one requires.

## Role Evolution

The shift from traditional to agentic roles follows a consistent pattern: human effort moves away from direct execution and toward specification, orchestration, and evaluation.

| Traditional Role | Agentic Role | Key Shift |
|---|---|---|
| Product Manager | Context Architect | From writing user stories to engineering machine-readable specs |
| Software Engineer | Agent Operator / Reviewer | From writing code to orchestrating agents and auditing output |
| QA Engineer | Evaluation Engineer | From finding bugs manually to designing automated constraint systems |
| Scrum Master | Flow Manager / AgentOps | From facilitating ceremonies to managing AI pipeline throughput |
| DevOps / Platform Engineer | Agent Platform Engineer | From CI/CD pipelines to sandboxed agent execution environments |
| Tech Lead | Principal Systems Architect | From hands-on coding to defining architectural laws and golden samples |

Notice that every shift moves the human further upstream — closer to intent, constraints, and governance, further from line-by-line execution.

## 1. Context Architect

The Context Architect is the strategic orchestrator of the hybrid workforce. Where a traditional Product Manager translates business needs into user stories for human developers, the Context Architect translates them into machine-readable specifications that AI agents can execute without ambiguity.

### Core Duties

- **Drafting Live Specs** — Producing structured specification documents that include precise acceptance criteria, edge cases, input/output contracts, and explicit context references. These specs are the primary input to agent execution — their quality directly determines output quality. This is the practice of [[context-engineering]] applied at the team level.
- **Curating the Context Index** — Maintaining the organized knowledge base that agents draw from during execution: API schemas, domain glossaries, decision logs, architectural decision records, and prior implementation examples. A well-curated Context Index reduces agent hallucination and drift.
- **Configuring HITL gates** — Defining which tasks require human approval before merging and which can proceed autonomously. This involves assessing risk per task type and setting thresholds: low-risk maintenance work flows automatically, while high-risk feature work requires Agent Operator review.

### Key Skills

- **Logical architecture** — The ability to decompose complex business requirements into structured, unambiguous specifications. This is closer to systems analysis than traditional product management.
- **Data organization** — Expertise in structuring knowledge for machine consumption: taxonomies, tagging systems, retrieval-optimized documentation.
- **Stakeholder diplomacy** — Translating between business stakeholders who think in outcomes and technical systems that require precise inputs. The Context Architect bridges the gap between human intent and machine execution.

## 2. Agent Operator

The Agent Operator is the high-leverage technical lead for the AI swarm. They do not spend their days writing feature code from scratch. Instead, they configure, launch, monitor, and audit agent runs — intervening only when agents get stuck or produce substandard output.

### Core Duties

- **Unblocking stuck agents** — Diagnosing why an agent entered a loop, misinterpreted a spec, or failed to produce passing tests. The Agent Operator provides corrective context, adjusts parameters, and re-launches. This is the "Agent Recovery" — the most time-sensitive part of the role.
- **Writing custom scripts and tooling** — Building the automation that connects agents to the development environment: custom MCP servers, context injection scripts, and monitoring dashboards.
- **Writing critical "Human-Owned Core" code** — Some code is too important or too architecturally sensitive for agent execution. The Agent Operator writes these components by hand — security-critical paths, core algorithms, and foundational abstractions that everything else depends on.

### Key Skills

- **Code auditing** — The ability to rapidly review agent-generated code for correctness, security vulnerabilities, performance issues, and architectural compliance. This requires reading code faster than writing it.
- **Linux/system debugging** — Agents operate in sandboxed environments. When something breaks at the infrastructure level — file permissions, network policies, resource limits — the Agent Operator needs to diagnose and fix it.
- **Technical prompt engineering** — Crafting the prompts, system instructions, and context packages that direct agent behavior. This is not about clever wording — it is about providing the right information in the right structure for the model to reason effectively.

## 3. Evaluation Engineer

The Evaluation Engineer represents the most dramatic role transformation in the agentic team. Traditional QA engineers find bugs by manually testing software. Evaluation Engineers shift QA from finding bugs to designing the constraints that prevent them from being created in the first place.

### Core Duties

- **Building Dockerized test environments** — Creating isolated, reproducible environments where agents execute and their output is validated. These environments mirror production conditions while maintaining strict isolation for safety.
- **Writing test cases before agent starts** — Defining the evaluation criteria upfront, not after the fact. The Evaluation Engineer produces test harnesses that the agent must satisfy, turning QA from a reactive checkpoint into a proactive specification.
- **Developing LLM-as-a-Judge rubrics** — Creating structured evaluation frameworks where a secondary LLM assesses agent output against quality criteria: code style adherence, security patterns, documentation completeness, and architectural compliance. These rubrics automate the aspects of code review that do not require human judgment.

### Key Skills

- **Python/TypeScript** — Primary languages for writing evaluation harnesses, test generators, and automation scripts.
- **Containerization** — Deep expertise in Docker and container orchestration for building the isolated environments where agents execute safely.
- **Statistical analysis** — Evaluating agent performance requires statistical thinking: tracking pass rates over time, identifying regression patterns, measuring confidence intervals on quality metrics, and distinguishing signal from noise in evaluation results.

## 4. Flow Manager

The Flow Manager is the "Air Traffic Controller" of the Hybrid Squad. Where a traditional Scrum Master facilitates human ceremonies and removes blockers, the Flow Manager ensures that the entire pipeline — from spec creation through agent execution to human review — flows without bottlenecks.

### Core Duties

- **Pipeline observability** — Monitoring the end-to-end flow of work through the squad: how many specs are in refinement, how many agents are executing, how many PRs await review, and where queues are building. The Flow Manager spots bottlenecks before they cascade.
- **AI compute budget management (Agent FinOps)** — Tracking and optimizing the cost of agent execution. Every agent run consumes API tokens, compute resources, and infrastructure time. The Flow Manager balances throughput against cost, ensuring the squad delivers maximum value per dollar spent. This connects directly to [[llmops]] practices applied at the team level.
- **Load-balancing reviews** — Distributing review work across Agent Operators to prevent any single person from becoming a bottleneck. When review queues grow, the Flow Manager redistributes work or escalates capacity issues.

### Key Skills

- **Lean/Kanban theory** — Understanding flow-based work management: work-in-progress limits, cycle time measurement, throughput optimization, and bottleneck theory. These concepts translate directly to managing hybrid human-agent pipelines.
- **Data literacy** — Comfort with dashboards, metrics, and trend analysis. The Flow Manager lives in observability tools and needs to quickly interpret what the data is saying about squad performance.
- **Financial awareness** — Understanding the cost model of AI agent execution: token pricing, compute costs, and the ROI calculation that justifies agent usage for different task types.

## 5. Agent Platform Engineer

The Agent Platform Engineer builds and maintains the Workbench Runtime — the sandboxed execution environment where agents operate. This role is the agentic equivalent of a DevOps/Platform Engineer, but focused on agent infrastructure rather than application infrastructure.

### Core Duties

- **Isolated microVM infrastructure** — Building the secure, sandboxed environments where agents execute code. Each agent run operates in its own isolated environment with controlled access to the filesystem, network, and system resources. This prevents agents from affecting production systems or each other.
- **JSON Function Definitions** — Defining the tool interfaces that agents use to interact with the development environment. Every tool call an agent can make — file reads, command execution, API calls — must be explicitly defined, documented, and secured.
- **Network egress governance** — Controlling what agents can access over the network. By default, agents should have minimal network access. The Agent Platform Engineer defines and enforces allowlists for API endpoints, package registries, and documentation sources.

### Key Skills

- **Kernel-level virtualization** — Deep understanding of Linux namespaces, cgroups, seccomp profiles, and container runtimes. Agent sandboxing requires fine-grained control over what processes can do at the OS level.
- **eBPF** — Expertise in extended Berkeley Packet Filter for runtime observability and security enforcement. eBPF provides the low-overhead monitoring needed to trace agent behavior without impacting execution performance.
- **API design** — Designing the tool interfaces that agents consume. These APIs must be precise, well-documented, and fail-safe — because the consumer is an AI model that cannot ask clarifying questions when an interface is ambiguous.

## 6. Principal Systems Architect

The Principal Systems Architect defines the "Laws of Physics" for the codebase. While a traditional Tech Lead might split time between coding and technical guidance, the Principal Systems Architect concentrates almost entirely on constraint definition — creating the rules and reference materials that ensure agent-generated code is structurally sound.

### Core Duties

- **Bounded contexts (DDD)** — Defining clear domain boundaries using Domain-Driven Design principles. Each bounded context has its own language, models, and rules. Agents operating within a bounded context inherit those constraints, which prevents them from creating cross-domain coupling or violating separation of concerns.
- **Automated architecture constraint tests** — Writing executable tests that verify architectural rules: dependency direction checks, layer violation detection, naming convention enforcement, and API contract validation. These tests run as part of the agent execution pipeline, catching violations before code reaches review.
- **Curating Golden Samples** — Selecting and maintaining reference implementations that demonstrate the correct way to build each type of component in the codebase. Golden Samples are the most efficient form of instruction for AI agents — a well-chosen example communicates patterns, style, and intent more effectively than pages of written rules.

### Key Skills

- **DDD mastery** — Fluency in Domain-Driven Design: bounded contexts, aggregates, domain events, anti-corruption layers, and context mapping. These concepts provide the vocabulary for defining the boundaries that agents must respect.
- **Static analysis tooling** — Expertise in tools like ArchUnit, dependency-cruiser, ESLint custom rules, and similar frameworks that encode architectural rules as executable checks.
- **Legacy refactoring** — Experience with incremental modernization of large codebases. The Principal Systems Architect must define migration paths that agents can execute safely, one bounded context at a time.

## Sizing the Squad

Not every team needs all six roles from day one. Squad composition scales with the maturity of your agentic adoption:

- **Starting out (1-2 agents):** A single engineer acts as both Agent Operator and Context Architect. The existing tech lead takes on Principal Systems Architect responsibilities. No dedicated Flow Manager or Platform Engineer needed yet.
- **Growing (3-5 agents):** Dedicated Context Architect and Agent Operator roles emerge. An engineer with DevOps experience begins building the Workbench Runtime. QA starts transitioning to the Evaluation Engineer model.
- **At scale (5+ agents):** All six roles are staffed. The Flow Manager becomes essential as pipeline complexity grows. The Agent Platform Engineer maintains increasingly sophisticated sandboxing infrastructure.

The key principle: add roles as pain appears, not in anticipation. Let bottlenecks tell you when a new role is needed.

## What Comes Next

With roles and team structure defined, the next step is building the infrastructure these teams operate on — the sandboxed execution environments, observability systems, and governance frameworks that make safe, scalable agent execution possible.

---
title: "From Agile to Agentic"
description: "How the Agentic Development framework evolves Agile practices for AI-native software teams"
authors:
  - dpavancini
lastUpdated: "2026-02-23"
---

## Overview

Agile moved software development from rigid, plan-driven processes to iterative, people-centric delivery. [[agentic-engineering|Agentic Development]] is the next evolution, preserving Agile's emphasis on feedback and adaptability while fundamentally changing what constrains delivery, who (or what) does the work, and how quality is assured.

## Why Agile Alone Is Not Enough

Agile was designed for a world where human developers are the execution bottleneck. Its ceremonies, roles, and artifacts all optimize for coordinating human effort across time-boxed iterations. That model breaks down when autonomous agents can execute well-defined tasks in minutes rather than days.

The core tensions:

- **Sprints assume human pace.** Two-week iterations make sense when implementation takes days. When agents can implement a feature in minutes, sprint boundaries become artificial constraints rather than useful planning horizons.
- **User stories assume human interpretation.** "As a user, I want..." relies on a developer's judgment to fill in gaps. Agents need machine-readable precision, not narrative flexibility.
- **Manual code review does not scale.** When agents produce code at volume, human reviewers become the bottleneck. Automated evaluation must handle the bulk of quality assurance.

Agentic development does not abandon Agile —it evolves it. The values of customer collaboration, responding to change, and working software over documentation remain. The practices change to match a new execution model.

## Key Differences

### From Time Constraint to Context Constraint

In Agile, the primary constraint is time. Teams plan what they can deliver within a sprint, and velocity measures story points per iteration.

In Agentic Development, the primary constraint is context. Delivery speed is determined by how clearly intent is specified and how completely the Context Index covers the problem domain. When context is excellent, agents deliver at machine speed. When context is poor, agents stall, hallucinate, or produce work that fails validation.

This shift changes what teams optimize for. Instead of removing blockers from developer calendars, leaders focus on improving spec quality, enriching the Context Index, and reducing ambiguity in task definitions.

### Continuous Governance and Quality Control

Agile typically relies on post-implementation quality gates: code review, QA testing, staging environments. These happen after a developer has written the code.

Agentic development moves quality control before and during execution:

- **Pre-code automated evals** verify that the spec itself is well-formed, complete, and free of contradictions before any agent touches the codebase
- **In-flight evaluation** runs continuously during agent execution, catching issues as they emerge rather than after the fact
- **Automated gate progression** replaces manual review for standard tasks, with human review reserved for high-risk changes

The result is a quality model that scales with agent throughput rather than being bottlenecked by human reviewer availability.

### New Role of the Human

Agile positions developers as craftspeople who write code, participate in ceremonies, and make implementation decisions. Agentic development shifts the human role from doer to director:

- **Context Architects** define what needs to happen and why, rather than implementing it themselves
- **Agent Operators** oversee execution and intervene on exceptions, rather than handling every task personally
- **Technical Reviewers** validate architectural fit and security, rather than reviewing every line of code

This does not mean less skill is required. Directing agents effectively demands deep technical understanding —you need to know what good looks like to specify it precisely and validate it efficiently.

### Ephemeral Infrastructure

Agile teams typically share development environments, staging servers, and CI pipelines. Agentic teams provision isolated, disposable workbenches for each task. This changes infrastructure from a shared, persistent resource to an on-demand, ephemeral one.

The implication: infrastructure becomes a per-task cost rather than a fixed team cost, and environment configuration becomes part of the spec rather than a separate DevOps concern.

## The Comparison Table

| Dimension | Waterfall | Agile | Agentic |
|-----------|-----------|-------|---------|
| **Primary Constraint** | Scope (fixed requirements) | Time (sprint boundaries) | Context (spec clarity) |
| **Unit of Work** | Requirement document | User story | Live Spec |
| **Execution Cycle** | Sequential phases | Time-boxed sprints (1-4 weeks) | Continuous spec-to-deployment loop |
| **Primary Artifact** | Specification document | Working software increment | Validated code + enriched Context Index |
| **Quality Control** | Phase-gate reviews | Sprint review + retrospective | Automated Eval Harness + HITL gates |
| **Role of Human** | Author and executor | Craftsperson and collaborator | Director and validator |
| **Infrastructure** | Shared, long-lived environments | Shared CI/CD pipelines | Ephemeral, per-task workbenches |

## From CI/CD to the Continuous Development Loop

The Continuous Development Loop (CDL) extends CI/CD to cover the full agentic lifecycle. Where CI/CD automates from code commit to deployment, CDL automates from spec creation to deployment and back.

### Phase Mapping

| CDL Phase | CI/CD Equivalent | Key Enhancement |
|-----------|-----------------|-----------------|
| Spec Injection | Branch creation / ticket start | Structured, machine-readable spec replaces informal ticket |
| Context Assembly | Developer onboarding to task | Automated retrieval from Context Index |
| Autonomous Execution | Code writing | Agent-driven implementation in isolated workbench |
| Eval Harness | CI test suite | Behavioral, security, and architectural checks beyond unit tests |
| Gate Review | Pull request review | Automated gates for standard tasks; HITL for high-risk |
| Context Update | Documentation update | Automatic Context Index enrichment from execution results |
| Deployment | CD pipeline | Standard deployment infrastructure, unchanged |

### Key Architectural Enhancements

Three capabilities distinguish CDL from traditional CI/CD:

1. **Security as a continuous thread.** Rather than a separate security review stage, security checks run at every phase —from spec validation through execution to deployment. This catches security issues at the point of introduction rather than after the fact.

2. **The Eval Harness replaces CI as the primary quality gate.** Traditional CI runs unit tests and linters. The Eval Harness runs behavioral verification, architectural conformance checks, performance baselines, and security scans as a unified evaluation suite.

3. **Knowledge-centric feedback loops.** Every CDL cycle produces structured data that feeds back into the Context Index. Failed evaluations become documented patterns. Operator interventions become context updates. Successful executions reinforce proven approaches. The system learns continuously.

## The Emergence of Coding Agents

The transition from Agile to Agentic is enabled by a rapid evolution in AI coding tools, moving from passive assistants to autonomous agents.

### From Copilots to Autonomous Teams

The trajectory of AI coding tools follows a clear progression:

1. **Code completion** (2021-2023) —Inline code suggestion tools. GitHub Copilot, TabNine, and others suggest the next line or block of code. The human remains in full control.

2. **Interactive assistants** (2023-2024) —Chat-based interfaces like ChatGPT, Claude, and Cursor's composer mode. Developers describe what they want in natural language and iterate on the output. Still human-driven, but higher leverage.

3. **Coding agents** (2024-2025) —Tools like Claude Code, GitHub Copilot agent mode, and Cursor's background agents that can plan multi-step implementations, execute them across files, run tests, and iterate on failures. The human defines the task; the agent handles execution.

4. **Autonomous teams** (2025-present) —Multi-agent systems where specialized agents handle different aspects of development (planning, implementation, testing, review) with human oversight at strategic checkpoints. This is where [[vibe-coding]] evolves from a solo activity into an orchestrated workflow.

### Generalist Agent Capabilities

Modern coding agents are generalists. A single agent can:

- Read and understand entire codebases
- Plan multi-step implementations before writing code
- Write code across multiple files and languages
- Run and interpret test suites
- Debug failures and iterate on solutions
- Interact with development tools (Git, package managers, build systems, APIs)
- Follow project-specific conventions defined in context files

This generalist capability is what makes the [[software-factory]] model viable. You do not need a different tool for each task —a single, well-contextualized agent can handle the full range of development work within its competence boundary.

### Platform Comparison

The coding agent landscape is evolving rapidly. As of early 2026, the major platforms include:

| Platform | Approach | Strengths |
|----------|----------|-----------|
| **Claude Code** (Anthropic) | CLI-native agent with deep tool use | Multi-file changes, complex refactoring, terminal integration |
| **GitHub Copilot / Codex** (Microsoft) | IDE-integrated + cloud agent | GitHub ecosystem integration, async background execution |
| **Gemini Code Assist / Jules** (Google) | IDE plugin + autonomous agent | Multi-model support, Google Cloud integration |
| **Open Source** (Aider, OpenCode, SWE-agent) | Community-driven CLI agents | Transparency, customizability, model flexibility |

The choice of platform matters less than the practices around it. A team with excellent specs, a rich Context Index, and disciplined [[human-in-the-loop]] governance will outperform a team with a "better" agent but poor context and ad-hoc workflows.

## Making the Transition

Teams moving from Agile to Agentic do not need to change everything at once. A practical path:

1. **Start with specs.** Pick one team or one project and require machine-readable specs instead of (or in addition to) user stories. Measure whether agent output quality improves.

2. **Introduce the Eval Harness.** Add automated behavioral checks alongside existing CI. Run them on both human and agent code to establish baselines.

3. **Pilot autonomous execution.** Route well-defined, low-risk tasks to agents. Keep human review in the loop initially to build confidence.

4. **Build the Context Index.** Capture lessons learned from agent executions, operator interventions, and review feedback in a structured, searchable format.

5. **Expand the boundary.** As confidence grows, route more task types to agents and shift human effort toward spec quality, architecture, and strategic decisions.

The goal is not to replace Agile ceremonies overnight. It is to evolve them incrementally as agents take on more of the execution workload, freeing humans to focus on the work that only humans can do.

## Next Steps

With the framework defined, the next chapter covers the [Team Model](/en/handbook/team-model) —how to structure roles, skills, and responsibilities in an agentic development organization.

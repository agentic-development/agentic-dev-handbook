---
title: "The Evaluation Harness"
description: "Automated testing, quality gates, and governance mechanisms that validate agent-generated code"
authors:
  - dpavancini
lastUpdated: "2026-02-23"
---

## Overview

The Evaluation Harness is the automated test suite that runs continuously during agent execution, validating every output before it reaches a human reviewer. It combines functional tests, security scans, architectural conformance checks, and LLM-as-a-Judge evaluations into a unified quality gate. No agent-generated code is presented to a human until it passes the Eval Harness.

## Why the Eval Harness Exists

In traditional development, quality assurance happens after implementation: a developer writes code, then tests and reviewers check it. In agentic development, this sequence is inverted. The Eval Harness defines the acceptance criteria before the agent starts, runs validation continuously during execution, and gates the output before it enters the review queue.

This inversion serves two purposes:

1. **It makes autonomous execution trustworthy.** Humans cannot review every line an agent produces at the speed agents produce it. The Eval Harness acts as an automated first reviewer that catches the majority of issues, so humans can focus their attention on the cases that require judgment.
2. **It creates a feedback loop.** When an agent's output fails evaluation, the harness returns structured feedback that the agent uses to correct its work and retry. This loop continues until the output passes or the execution budget is exhausted.

## Two Types of Validation

Not all validation is the same. The Eval Harness distinguishes between two fundamentally different types of checks, each suited to different aspects of code quality.

### Deterministic Validation

Deterministic validation produces binary pass/fail results based on strict, unambiguous rules. There is no gray area -- the check either passes or it does not.

Examples of deterministic checks:

- **Forbidden imports** -- The agent's code imports a module that is banned by the project's architectural rules. The check blocks the PR automatically.
- **Type safety** -- The TypeScript compiler reports type errors. The code does not compile.
- **Security scans** -- Static analysis detects a known vulnerability pattern, such as SQL injection or hardcoded credentials.
- **Test suite passage** -- Unit and integration tests must all pass. A single failure blocks the output.
- **Linting and formatting** -- Code must conform to the project's style rules. Non-conforming code is rejected.
- **Dependency policy** -- New dependencies must be on the approved list. Unapproved packages are flagged.

Deterministic checks are fast, reliable, and cheap. They should cover every rule that can be expressed as a binary condition. When a rule can be made deterministic, it must be -- do not leave deterministic decisions to probabilistic evaluation.

### Probabilistic Evaluation

Probabilistic evaluation uses an LLM-as-a-Judge to assess non-deterministic aspects of agent output that cannot be reduced to binary rules. These evaluations produce scores, grades, or ranked assessments rather than simple pass/fail results.

Examples of probabilistic evaluations:

- **Code quality grading** -- A judge model assesses whether the generated code follows idiomatic patterns, uses meaningful variable names, and maintains appropriate abstraction levels.
- **Documentation quality** -- The judge evaluates whether generated documentation is clear, accurate, and complete enough for the target audience.
- **Architectural alignment** -- For cases that go beyond what static analysis can check, the judge assesses whether the implementation aligns with the project's architectural intent.
- **Tone and style** -- For agent-generated communications (commit messages, PR descriptions, user-facing text), the judge evaluates whether the tone matches organizational standards.

Probabilistic evaluations are configured with rubrics -- structured scoring criteria that guide the judge model. A rubric defines the dimensions being evaluated, the scale, and the threshold for passing.

### Combining Both Types

The most effective Eval Harness combines both validation types in a layered approach:

1. **Deterministic checks run first.** They are fast and cheap. If code fails a deterministic check, there is no point running expensive probabilistic evaluations.
2. **Probabilistic evaluations run second.** They assess the aspects that survive deterministic filtering -- the qualities that require judgment rather than rules.

This layering optimizes for both reliability and cost. Deterministic checks catch the easy problems quickly; probabilistic evaluations catch the subtle ones.

### Implementation Pitfall: Over-Reliance on Probabilistic Evaluation

A common and dangerous mistake is relying solely on probabilistic LLM evaluations for mission-critical logic. LLM judges are powerful but non-deterministic -- they can produce different assessments on the same input across runs, and they can be fooled by confidently-written but incorrect code.

The rule of thumb: if a check can be expressed as a deterministic rule, it must be deterministic. Reserve probabilistic evaluation for qualities that genuinely require judgment. Never use an LLM judge as the sole gate for security-critical, compliance-critical, or safety-critical validation. Combine deterministic code constraints with probabilistic reasoning checks to get both reliability and nuance.

## Governance

The Eval Harness is one component of a broader governance system that ensures agent-generated code meets organizational standards for security, compliance, and quality.

### [[human-in-the-loop|Human-in-the-Loop]] Gates

Some decisions are too consequential for full automation, regardless of how sophisticated the Eval Harness becomes. Mandatory HITL gates include:

- **Security-critical changes** -- Authentication flows, authorization logic, encryption implementations, and any code that handles sensitive data.
- **Architectural decisions** -- New service boundaries, database schema changes, API contract modifications, and technology selection.
- **High-blast-radius deployments** -- Changes that affect all users, critical infrastructure, or financial systems.
- **Novel patterns** -- First-time implementations of approaches not covered by existing specs or Golden Samples.

The Eval Harness flags these changes for human review automatically based on rules defined by the Context Architect and Principal Systems Architect. The key insight is that not all code changes carry equal risk. HITL gates allocate human attention where it matters most.

### [[guardrails|Guardrails]] Integration

The Eval Harness integrates with the broader guardrails system to enforce constraints at multiple levels:

- **Input guardrails** -- Validate the spec and context before the agent starts execution. Reject malformed specs, flag missing context, and check for known conflict patterns.
- **Runtime guardrails** -- Monitor the agent during execution. Intervene if the agent attempts forbidden operations, exceeds resource limits, or enters unproductive loops.
- **Output guardrails** -- Validate the final artifacts before they leave the workbench. This is where the Eval Harness's deterministic and probabilistic checks run.

## Observability

Governance without visibility is governance in name only. The Eval Harness produces structured execution traces that make every agent decision auditable and debuggable.

### Execution Traces

Every agent run produces a trace that links the full reasoning chain:

- **Thought** -- What the agent decided to do and why (the model's reasoning)
- **Action** -- What tool call, code change, or command the agent executed
- **Result** -- What happened: the tool's output, test results, or error messages

These traces are stored alongside the agent's artifacts and are accessible during code review. When a reviewer wants to understand why the agent made a particular implementation choice, the trace provides the full reasoning path.

### [[llmops|LLMOps]] Dashboards

Aggregate execution traces into operational dashboards that surface:

- **Pass rates** -- What percentage of agent runs pass the Eval Harness on the first attempt? Declining pass rates signal context degradation or spec quality issues.
- **Failure categories** -- Which types of checks fail most frequently? This identifies where to invest in better context, better specs, or better tools.
- **Cycle times** -- How long does it take from task dispatch to Eval Harness passage? Increasing cycle times may indicate that tasks are too complex for single-agent execution.
- **Retry patterns** -- How many evaluation loops does the average task require? Tasks that consistently need many retries may need better specs or different decomposition.

### FinOps and Token Budget Management

Every agent run consumes API tokens, compute resources, and infrastructure time. Without budget controls, a single stuck agent can burn through significant resources before anyone notices.

The Eval Harness integrates token budget management through circuit breakers:

- **Per-task budgets** -- Each task has a maximum token budget. When the budget is exhausted, the agent stops and the task is escalated to a human.
- **Per-loop limits** -- The maximum number of evaluation-retry loops is capped. An agent that cannot pass the harness in a reasonable number of attempts is unlikely to succeed with more attempts.
- **Cost tracking** -- Every agent run logs its total cost: model tokens, tool invocations, compute time. This data feeds into the Flow Manager's FinOps dashboards for team-level budget management.
- **Circuit breakers** -- If an agent's cost-per-task exceeds a configurable threshold, the circuit breaker trips and halts execution. This prevents runaway costs from agents stuck in unproductive loops.

Budget controls are not just about cost savings. They are a quality signal. Tasks that consistently exhaust their budgets are tasks where something is wrong -- the spec is unclear, the context is insufficient, or the task is beyond the current capability of autonomous execution.

## Building Your Eval Harness

### Start Simple

The minimum viable Eval Harness consists of:

1. **Your existing test suite** -- Unit tests, integration tests, and end-to-end tests you already have.
2. **A linter and formatter** -- Enforce code style deterministically.
3. **A security scanner** -- Run static analysis for known vulnerability patterns.
4. **A pass/fail gate** -- Block agent output that fails any of the above.

This baseline catches the most common agent errors and establishes the habit of automated validation. You can add probabilistic evaluations, architectural checks, and sophisticated governance later.

### [[test-generation-workflow|Test Generation]] Integration

Agents can help build the harness that evaluates their own output. A powerful pattern is to have one agent generate tests from the spec's Behavioral Contract and a different agent generate the implementation. The test-writing agent and the implementation agent operate independently, reducing the risk that both make the same mistake.

This approach also scales the Eval Harness organically. As the team writes more specs with more detailed Behavioral Contracts, the test coverage of the harness grows automatically.

### Maturity Progression

As your agentic practice matures, the Eval Harness evolves:

1. **Level 1: Existing checks** -- Run your current test suite and linters against agent output.
2. **Level 2: Spec-derived checks** -- Automatically generate tests from Live Spec acceptance criteria.
3. **Level 3: LLM-as-a-Judge** -- Add probabilistic evaluations for code quality, documentation, and architectural alignment.
4. **Level 4: Continuous governance** -- Integrate HITL gates, cost controls, and observability into a unified governance pipeline.
5. **Level 5: Self-improving** -- The harness uses execution data to identify gaps in its own coverage and suggests new checks to the Evaluation Engineer.

Each level builds on the previous one. Do not skip ahead -- a sophisticated LLM judge is useless if your basic test suite does not run reliably.

## What Comes Next

The Agent Workbench, Context Management, and Evaluation Harness together form the operational infrastructure of agentic development. With this infrastructure in place, the next step is building the team that operates it -- the roles, skills, and organizational structures covered in the Team Model chapter.

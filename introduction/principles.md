---
title: "Principles of Agentic Development"
description: "Ten principles that define agentic development as a discipline"
authors:
  - dpavancini
lastUpdated: "2026-02-24"
---

## Overview

There is widespread confusion in the market between "shipping code" and "developing software." LLMs can generate syntactically correct code at scale — that problem is solved. But developing software — understanding requirements, managing complexity, ensuring reliability, governing change — remains a deeply human challenge.

[[agentic-engineering|Agentic development]] addresses this challenge through structured collaboration between humans and agents. The following ten principles define what we mean by agentic development and guide every chapter in this handbook.

## At a Glance

1. **Agents build the code; humans direct the system.**
2. **Context is the bottleneck, not typing speed.**
3. **Code becomes increasingly ephemeral; specs endure.**
4. **Software engineering best practices expand, not disappear.**
5. **Specs must be structured and machine-readable.**
6. **The entire SDLC is redesigned for human-agent collaboration.**
7. **New expertise paths replace old ones.**
8. **Teams explicitly include both agents and humans.**
9. **Enterprise governance is built in, not bolted on.**
10. **Vibe coding is not agentic development.**

## What We Believe

### 1. Agents Build the Code; Humans Direct the System

The human developer shifts from writing code to directing, reviewing, and orchestrating the agents that produce it.

### 2. Context Is the Bottleneck

In the agentic era, the constraint is no longer typing speed — it is [[context-engineering|context]]. Building a well-structured backlog that agents can consume is the biggest challenge teams will face. The Framework chapter explores this through Context-First Architecture.

### 3. Code Becomes Increasingly Ephemeral

As agent capabilities mature, code becomes increasingly ephemeral — re-compilable from specifications with minimal human intervention. The spec, not the code, becomes the durable artifact. The Infrastructure chapter covers this through the Ephemeral Workbench pattern.

### 4. Best Practices Expand, Not Disappear

Agentic development does not replace software engineering best practices — it expands them. Testing, code review, and architecture matter more, not less, when agents are producing code at scale. The Operations chapter details how traditional practices evolve through agentic ceremonies.

## What We Do

### 5. Specs Must Be Structured and Machine-Readable

Agentic development expects well-designed, machine-readable specifications. Agents need structured, unambiguous inputs to produce reliable outputs. The Framework chapter's Spec-Driven Development pillar covers this in depth.

### 6. The Entire SDLC Is Redesigned

Agentic development rethinks the complete Software Development Lifecycle — from project scoping and work planning to execution and deployment. Every phase is redesigned for human-agent collaboration. See the Framework chapter's comparison of Waterfall, Agile, and Agentic approaches.

### 7. New Expertise Paths Replace Old Ones

The agentic era will deeply affect how engineers build expertise. New skills emerge — [[context-engineering]], specification writing, agent orchestration — while traditional coding fluency becomes table stakes. The Team Model chapter defines six new roles built around these skills.

## How We Structure

### 8. Teams Include Both Agents and Humans

Software will be developed by teams that explicitly include both agents and humans. This is not a metaphor — it is a design principle that affects tooling, workflows, and team structure. The Team Model chapter introduces the Hybrid Squad as the foundational unit.

### 9. Enterprise Governance Is Built In

Enterprise software development requires accountability and governance by design, even when agents produce the code. Human sign-off — explicit or implicit — is required at every critical juncture. The Framework chapter's Gate-Based Governance pillar and the Operations chapter's ceremonies make this concrete.

### 10. Vibe Coding Is Not Agentic Development

[[vibe-coding]] is primarily an individual method of coding — rapid, exploratory, and unstructured. Agentic development is a team discipline with governance, repeatability, and accountability built in.

## What's Next

These principles are the foundation — the rest of the handbook makes them concrete. Continue to [Who Is This For?](/en/handbook/introduction/who-is-this-for) to see how this handbook serves different roles, or jump directly into the [Framework](/en/handbook/framework) chapter to explore the five pillars that put these principles into practice.

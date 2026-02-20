---
title: "Your First Agentic Workflow"
description: "A step-by-step walkthrough of completing a task with an AI coding agent"
authors:
  - dpavancini
lastUpdated: "2026-02-19"
---

## Overview

This page walks you through a complete agentic development workflow — from defining a task to reviewing the result. We'll use a simple example: adding a utility function with tests.

## The Workflow

Agentic development follows a predictable cycle:

1. **Define** — Clearly state what you want
2. **Context** — Ensure the agent has what it needs
3. **Execute** — Let the agent work
4. **Review** — Verify the output
5. **Iterate** — Refine if needed

## Step 1: Define the Task

Good task definitions are specific and bounded. Compare:

**Vague:** "Make the app faster"

**Specific:** "Add a `debounce` utility function in `src/utils/debounce.ts` that accepts a function and delay in milliseconds, returns a debounced version. Include unit tests."

The more precise your request, the better the result.

## Step 2: Provide Context

Point the agent to relevant files:

- Existing utility functions (for style consistency)
- The test framework configuration
- Type conventions used in the project

Most tools detect context automatically, but explicit references help.

## Step 3: Let the Agent Work

Start the task and observe. A well-configured agent will:

1. Read related files to understand conventions
2. Write the implementation
3. Write tests
4. Run the tests to verify
5. Fix any issues

Resist the urge to interrupt mid-flow. Let the agent complete its cycle, then review.

## Step 4: Review the Output

Check the agent's work like you would review a colleague's pull request:

- **Correctness** — Does it do what you asked?
- **Style** — Does it match project conventions?
- **Edge cases** — Are boundary conditions handled?
- **Tests** — Are they meaningful, not just passing?
- **No extras** — Did it add unnecessary code or dependencies?

## Step 5: Iterate

If something needs adjustment, be specific:

**Instead of:** "Fix it"

**Try:** "The debounce function should cancel pending calls when the component unmounts. Add a `cancel` method to the returned function."

## Common Patterns

As you gain experience, you'll recognize patterns that work well with agents. Explore the [Patterns](/en/patterns) section for a curated collection, including:

- [Prompt Chaining](/en/patterns) — Breaking complex tasks into sequential steps
- Test-Driven Development — Writing tests first, then having the agent implement

## Tips for Success

- **Start small** — Build confidence with simple tasks before complex ones
- **Be explicit** — Agents follow instructions literally; ambiguity leads to surprises
- **Trust but verify** — Always review generated code before merging
- **Learn the tools** — Each AI tool has unique strengths; learn to leverage them
- **Iterate fast** — Quick feedback loops produce better results than lengthy specifications

## What's Next

You're ready to start using agentic development in your projects. Explore the rest of this handbook for deeper dives into specific topics, or browse the [Templates](/en/templates) for ready-to-use prompts.

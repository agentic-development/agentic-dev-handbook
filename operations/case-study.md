---
title: "Case Study: From Spec to Production"
description: "An end-to-end example of agentic development routines applied to an e-commerce recommender system"
authors:
  - dpavancini
lastUpdated: "2026-02-24"
---

## Overview

This page walks through a complete example of how the governance routines defined in the previous page work together in practice. The scenario: a team building a Recommender System feature for an e-commerce platform.

## Feature Charter Definition (Quarterly)

During Strategic Spec Definition, the Context Architect authors three [[feature-charter]] documents for the "Personalized Recommendations" initiative:

1. **User Behavior Tracking** — Capture browsing, search, and purchase events.
2. **Recommendation Engine** — Build the ML pipeline that produces recommendations.
3. **Frontend Integration** — Display recommendations in the product UI.

Each Feature Charter defines its architectural vision, decomposition map, and governance configuration. The Recommendation Engine charter is classified as high-risk (novel architecture, ML pipeline complexity) and configured with mandatory HITL gates at every stage.

## Live Spec Task (Weekly)

During the Specification Engineering Block, the Context Architect produces a Context Packet for the first Live Spec in the User Behavior Tracking charter's decomposition map: "Implement event capture for product page views."

The Context Packet includes:

- **Live Spec** — Acceptance criteria: capture page view events with product ID, user session, timestamp, and viewport data. Edge cases: anonymous users, bot traffic filtering, concurrent sessions.
- **Architectural Rules** — Events must flow through the existing event bus. No direct database writes from the capture layer. Events conform to the platform's CloudEvents schema.
- **Golden Sample** — A reference implementation of the existing "add to cart" event capture, showing the correct patterns, naming conventions, and test structure.

## Agent Execution (Daily)

The Agent Operator feeds the Context Packet to a Feature Agent. The Claude Code prompt follows this structure:

```
You are implementing event capture for product page views.

Context Packet:
- Live Spec: [attached]
- Architectural Rules: Events flow through EventBus. No direct DB writes.
  CloudEvents schema required.
- Golden Sample: See add-to-cart event implementation at
  src/events/add-to-cart.ts

Requirements:
1. Implement ProductPageViewEvent following the CloudEvents schema
2. Register the event handler with the EventBus
3. Include bot traffic filtering using the existing BotDetector service
4. Write unit tests covering all acceptance criteria and edge cases

Constraints:
- Do not modify any files outside src/events/ and src/events/__tests__/
- Follow the naming conventions shown in the Golden Sample
- All tests must pass before submitting the PR
```

## Iterative Feedback Loop

The agent produces an initial implementation. The Evaluation Harness runs automated tests. Two tests fail: the bot filtering logic does not handle the edge case where a user agent string is missing, and the CloudEvents schema validation rejects the event because the `source` field uses the wrong format.

The agent iterates: it fixes the null user-agent handling, corrects the `source` field format, and resubmits. All tests pass.

## Agent Recovery

On the second task — "Implement search query event capture" — the agent gets stuck. It attempts to import a module from the recommendation engine domain, violating the bounded context boundary. The Evaluation Harness flags an architectural violation, but the agent cannot resolve it because it lacks context about the correct inter-domain communication pattern.

The Agent Operator intervenes:

1. **Diagnoses** the issue: the agent's context does not include the inter-domain messaging contract.
2. **Injects** the missing context: the AsyncMessageBus interface definition and an example of cross-domain event publishing.
3. **Resumes** the agent, which now correctly publishes the search event through the message bus instead of importing from the recommendation domain.

## Task Completion

The Agent Operator reviews the final PR. The code follows the Golden Sample patterns, passes all automated tests, and respects architectural boundaries. The PR is approved and merged. The Flow Manager updates the AgentOps Dashboard, marking the task complete and logging the token consumption against the weekly budget.

Total elapsed time from Context Packet to merged PR: 47 minutes for two tasks, including one Agent Recovery. The equivalent work would have taken a human developer approximately 1.5-2 days.

## Key Takeaways

This example illustrates several principles from the framework:

- **Context quality determines execution quality.** The first task succeeded on the second iteration because the Context Packet was thorough. The second task required an Agent Recovery because the inter-domain communication pattern was missing from context.
- **The Evaluation Harness catches errors the agent cannot self-diagnose.** Architectural violations are invisible to the agent without explicit constraints. Automated checks caught what the agent could not.
- **Agent Recoveries are diagnostic, not punitive.** The operator did not rewrite the code — they identified the missing context, injected it, and let the agent complete the work.
- **The full lifecycle connects.** Quarterly planning produced the Feature Charters, weekly specification engineering produced the context packets, and daily execution consumed them. Each cadence feeds into the next.

## What Comes Next

The next page covers the metrics and success tracking frameworks that measure whether these routines are actually working — from leverage ratios and flow efficiency to the economic metrics that justify the investment in agentic infrastructure.

---
title: "Setting Up Your Environment"
description: "How to configure your development environment for agentic workflows"
authors:
  - dpavancini
lastUpdated: "2026-02-19"
---

## Overview

Before you can work with AI coding agents, you need a properly configured development environment. This page walks you through the essentials.

## Choose Your AI Coding Tool

Several tools support agentic development workflows:

- **Claude Code** — Anthropic's CLI agent for terminal-based development
- **Cursor** — AI-native code editor built on VS Code
- **GitHub Copilot** — AI pair programmer integrated into VS Code and JetBrains
- **Windsurf** — Codeium's AI-powered IDE

Each tool has different strengths. Claude Code excels at multi-file changes and complex refactoring. Cursor offers tight IDE integration. Pick the one that fits your workflow best — or use multiple for different tasks.

## Essential Setup Steps

### 1. Version Control

Agentic workflows make frequent changes. A solid Git setup is non-negotiable:

- Initialize or clone your repository
- Create a feature branch for agentic work
- Configure `.gitignore` properly
- Consider using git worktrees for parallel agent sessions

### 2. Project Context

AI agents work best with context. Provide it through:

- **README.md** — Project overview, setup instructions, architecture
- **CLAUDE.md / .cursorrules** — Agent-specific instructions and conventions
- **Type definitions** — TypeScript types, API schemas, database models
- **Test suites** — Existing tests tell agents about expected behavior

### 3. Environment Configuration

```bash
# Example: Setting up Claude Code
npm install -g @anthropic-ai/claude-code
cd your-project
claude
```

### 4. Safety Net

Always have safeguards in place:

- **Pre-commit hooks** — Lint and format before committing
- **CI/CD pipeline** — Automated tests catch regressions
- **Code review** — Human review of agent-generated code
- **Branch protection** — Prevent direct pushes to main

## Verification Checklist

Before starting agentic work, verify:

- [ ] Git repository initialized with clean working tree
- [ ] AI tool installed and authenticated
- [ ] Project context files in place (README, type definitions)
- [ ] Linting and formatting configured
- [ ] Test suite runs successfully
- [ ] CI/CD pipeline active

## Next Steps

With your environment ready, move on to [Your First Agentic Workflow](/en/handbook/getting-started/your-first-agentic-workflow) to put it all into practice.

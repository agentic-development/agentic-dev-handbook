# Contributing to the Handbook

Thank you for your interest in contributing to the Agentic Development Handbook! This guide explains how to add or edit content.

## File Structure

```
content/handbook/
  _meta.json              # Root: defines chapter ordering
  introduction/
    _meta.json            # Chapter: defines page ordering
    what-is-agentic-development.md
    why-it-matters.md
    how-to-use-this-handbook.md
  getting-started/
    _meta.json
    setting-up-your-environment.md
    your-first-agentic-workflow.md
  assets/
    images/               # Shared images
```

## Adding a New Page

1. Create a `.md` file in the appropriate chapter directory
2. Add frontmatter at the top:

```yaml
---
title: "Your Page Title"
description: "A short description of the page"
authors:
  - your-github-username
lastUpdated: "2026-02-19"
---
```

3. Add the page slug (filename without `.md`) to the chapter's `_meta.json`:

```json
{
  "pages": [
    "existing-page",
    "your-new-page"
  ]
}
```

## Adding a New Chapter

1. Create a new directory under `content/handbook/`
2. Add a `_meta.json` inside with the page order:

```json
{
  "pages": [
    "first-page",
    "second-page"
  ]
}
```

3. Add the chapter to the root `_meta.json`:

```json
{
  "chapters": [
    { "slug": "introduction", "title": "Introduction" },
    { "slug": "your-chapter", "title": "Your Chapter Title", "description": "Optional description" }
  ]
}
```

## Writing Guidelines

- Use **Markdown** with standard formatting (headings, lists, code blocks, links)
- Start with an `## Overview` section
- Use `##` for main sections and `###` for subsections (these appear in the table of contents)
- Keep paragraphs focused and concise
- Include code examples where relevant using fenced code blocks

## Images

Place images in `assets/images/` and reference them with relative paths:

```markdown
![Alt text](../assets/images/my-image.png)
```

## Cross-Links

Link to other site content using absolute paths:

```markdown
See the [Prompt Chaining](/en/patterns/prompt-chaining) pattern for more details.
```

## Submitting Changes

1. Fork the handbook repository
2. Create a feature branch: `git checkout -b add-my-page`
3. Make your changes
4. Submit a pull request with a clear description

## Questions?

Open an issue in the handbook repository or reach out to the maintainers.

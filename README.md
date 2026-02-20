# Agentic Development Handbook

Content repository for the [agentic-dev.org](https://agentic-dev.org) handbook — a practical guide to building software with AI agents.

## Structure

```
_meta.json                    # Chapter ordering
introduction/                 # What is Agentic Development
  what-is-agentic-development.md
  why-it-matters.md
  how-to-use-this-handbook.md
getting-started/              # Setup and first workflow
  setting-up-your-environment.md
  your-first-agentic-workflow.md
assets/images/                # Shared images
```

Each directory is a chapter. Each `.md` file is a page. Ordering is controlled by `_meta.json` files.

## Usage

This repo is consumed as a git submodule by [agentic-development](https://github.com/agentic-development/agentic-development) at `content/handbook/`.

```bash
# Clone the parent repo with submodule
git clone --recurse-submodules https://github.com/agentic-development/agentic-development.git

# Or initialize after cloning
git submodule update --init
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add pages, chapters, and images.

## License

MIT

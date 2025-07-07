# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build & Development Commands

```bash
# Install dependencies
poetry install

# Run interpreter
poetry run interpreter

# Run tests
poetry run pytest -s -x

# Format code (uses pre-commit hooks)
black .
isort .

# Run a single test
poetry run pytest tests/test_cli.py::test_name -s
```

## Architecture Overview

Open Interpreter follows a modular architecture with clear separation of concerns:

### Core Components
- **interpreter/interpreter.py**: Main Interpreter class that orchestrates all functionality
- **interpreter/tools/**: Tool implementations (Bash, Computer, Edit) that execute user commands
- **interpreter/cli.py**: Command-line interface entry point
- **interpreter/server.py**: FastAPI server for API access

### Key Design Patterns

1. **Tool System**: Abstract base class pattern for extensible tool support. Each tool (Bash, Computer, Edit) inherits from `BaseTool` and implements execution logic.

2. **Profile System**: Configuration management via Profile class. Default profile stored at `~/.openinterpreter/default_profile.py`.

3. **Async Architecture**: Async-first design for streaming responses. The interpreter supports both sync and async usage patterns.

4. **Multi-LLM Support**: Uses litellm for unified LLM interface with native Anthropic support for computer-use features.

### Critical Implementation Notes

- **Simple Bash Mode**: Currently forces simple bash implementation (see tools/__init__.py line with `force_simple_bash = True`)
- **Lazy Loading**: Uses `__getattr__` in __init__.py for performance optimization
- **Anthropic Integration**: Special handling for computer-use and prompt-caching betas when using Anthropic models
- **Environment Detection**: Automatic TTY detection for interactive mode vs script execution

## Development Philosophy

From CONTRIBUTING.md - this project values:
- Minimalism and tight scoping
- Simplicity over feature complexity
- Accessibility for new users
- Skepticism toward new extensions/integrations

When making changes, prefer simple solutions that maintain the project's approachable nature.
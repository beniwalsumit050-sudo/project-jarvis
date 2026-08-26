# Contributing to Project JARVIS

Welcome to Project JARVIS! This guide outlines the development workflow, coding standards, branch conventions, and quality expectations for all contributors.

---

## 1. Development Principles & Phased Discipline

Project JARVIS follows a structured 13-stage roadmap (Phase 0 through Phase 12, culminating in JARVIS V1.0). To ensure stability and architectural cohesion:

- **Follow Phase Boundaries**: Implement features strictly according to the currently active phase. Avoid introducing components from later phases prematurely.
- **Async-First**: All core I/O, tool executions, and event dispatches must be non-blocking (`async`/`await`).
- **Strict Typing**: All new Python code must feature full type annotations adhering to `mypy` strict mode.
- **Safety First**: Every new tool or automated capability must declare its risk tier and integrate with the safety gateway.

---

## 2. Git & Branching Strategy

### 2.1 Branch Naming Conventions
- Features: `feat/phase-<N>-<short-description>` (e.g., `feat/phase-1-cli-repl`, `feat/phase-3-filesystem-tools`)
- Bug Fixes: `fix/<issue-description>` (e.g., `fix/memory-leak-audio-stream`)
- Documentation: `docs/<topic-description>` (e.g., `docs/update-architecture`)
- Refactoring: `refactor/<subsystem>` (e.g., `refactor/event-bus`)

### 2.2 Commit Message Format
We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <short summary>

[optional body explaining rationale]

[optional footer(s)]
```

**Common Types:**
- `feat`: A new feature or capability.
- `fix`: A bug fix.
- `docs`: Documentation changes only.
- `refactor`: Code changes that neither fix a bug nor add a feature.
- `test`: Adding or updating test cases.
- `chore`: Tooling, configuration, or dependency updates.

**Examples:**
- `feat(cli): implement interactive shell loop with rich formatting`
- `docs(blueprint): initialize phase 0 documentation suite`
- `fix(tools): handle file not found error gracefully in read_file`

---

## 3. Code Standards & Style Guide

- **Formatting & Linting**: Use `ruff` for code formatting and linting.
  ```bash
  ruff check .
  ruff format .
  ```
- **Type Checking**: Use `mypy` for static analysis.
  ```bash
  mypy jarvis/
  ```
- **Docstrings & Comments**: All public classes, functions, and tool schemas must include clear Google-style or standard Python docstrings describing arguments, return types, and exceptions.
- **Dependencies**: Keep external dependencies lean and justified. Use standard library alternatives where feasible.

---

## 4. Pull Request (PR) Workflow

1. **Branch Out**: Create your branch from `main`.
2. **Develop & Test**: Implement changes and add corresponding automated unit/integration tests under `tests/`.
3. **Verify Locally**: Ensure all tests pass and linters report clean:
   ```bash
   pytest
   ruff check .
   mypy jarvis/
   ```
4. **Submit PR**: Open a Pull Request with a clear description of:
   - What phase/milestone this PR addresses.
   - Summary of changes made.
   - Verification steps taken.
5. **Code Review**: Address reviewer feedback and ensure CI checks pass before merging.

---

## 5. Security & Safety Review Checklist

When adding a tool or capability:
- [ ] Is the tool categorized into the appropriate Risk Tier (0–3)?
- [ ] Are path arguments sanitized to prevent path traversal?
- [ ] Are secrets/credentials protected from being emitted to stdout or log files?
- [ ] Does the tool handle timeouts and subprocess errors cleanly?
- [ ] Is an audit log entry emitted upon tool execution?

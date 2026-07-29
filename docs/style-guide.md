# Documentation Style Guide

This document defines the documentation standards used throughout the Phantom Ecosystem project.

Its purpose is to ensure consistency, readability, and long-term maintainability across every document contained in this repository.

---

# General Principles

Documentation should describe the current implementation of the ecosystem.

The documentation is intended to be:

- Accurate
- Consistent
- Versionable
- Easy to navigate
- Independent from the implementation repositories

Architecture documentation must never become a product manual or marketing material.

---

# Source of Truth

The architecture documentation reflects the actual implementation of the ecosystem.

Whenever discrepancies exist between implementation and documentation, the implementation is considered the source of truth until the documentation is updated.

No undocumented behavior should be described.

Future ideas belong in `ROADMAP.md`, not in architecture documents.

---

# Writing Style

Documentation should be written using clear technical English.

Preferred style:

- Objective
- Concise
- Descriptive
- Implementation-focused

Avoid:

- Marketing language
- Promotional wording
- Personal opinions
- Speculation
- Ambiguous statements

Example:

✔

> Phantom Correlation subscribes to Redis Pub/Sub channels.

✘

> Phantom Correlation intelligently orchestrates every module in the ecosystem.

---

# Terminology

Use consistent terminology throughout the project.

| Preferred | Avoid |
|-----------|-------|
| Module | Bot |
| Ecosystem | Platform (unless appropriate) |
| Service | Process |
| Event | Message (unless referring to Redis messages) |
| Correlation Engine | Brain |
| Deployment | Installation |
| Documentation | Manual |

---

# Markdown Guidelines

Use a single H1 (`#`) per document.

Structure headings hierarchically.

Example:

```markdown
# Title

## Section

### Subsection
```

Avoid skipping heading levels.

---

# Document Structure

Technical documents should follow this structure whenever applicable.

```text
Title

Purpose

Overview

Architecture

Components

Communication

Configuration

Design Decisions

Limitations

Related Documentation
```

Not every document requires every section.

---

# Module Documentation

Every detailed module document under `docs/modules/` should use the same primary section order:

```text
Purpose
Overview
Responsibilities
Scope
Architecture Role
Internal Components
Workflow
Inputs
Outputs
Hive Mind Participation
Communication
Dependencies
Configuration and State
Runtime Characteristics
Failure Behavior
Security Considerations
Current Limitations
Summary
Related Documentation
```

Module-specific facts belong inside these shared sections. Additional headings should be introduced only when the same information cannot be represented clearly within the standard structure.

Module READMEs under `modules/<module>/` should remain concise and use the shared README structure:

```text
Overview
Status
Responsibilities
Repository Structure
Documentation
Roadmap
License
```

Detailed implementation-aligned explanations belong in `docs/modules/`.

---

# Repository References

Always reference documents using repository-relative paths.

Example:

```
docs/architecture.md
```

Do not use absolute URLs for internal documentation.

---

# Diagrams

Every diagram should represent the actual implementation.

Diagrams must never describe planned functionality.

Recommended diagram types:

- Architecture
- Deployment
- Communication
- Event Flow
- Component Relationships

Whenever the architecture changes, related diagrams should be updated accordingly.

---

# Tables

Use tables for:

- Module catalogs
- Configuration summaries
- Responsibilities
- Feature comparisons

Avoid large paragraphs when tabular information improves readability.

---

# Code Blocks

Always specify the language when possible.

Example:

```yaml
services:
```

```json
{
  "event_type": ""
}
```

```bash
docker compose up
```

---

# Notes

Use blockquotes for informational notes.

Example:

> This repository intentionally does not contain source code.

Use notes only when they provide important context.

---

# Scope

Each document should have a clearly defined responsibility.

Avoid duplicating information across multiple documents.

Instead, reference the appropriate document.

Example:

- Redis communication → `docs/hive-mind.md`
- Deployment → `docs/deployment.md`
- Security → `docs/security-model.md`

---

# Naming Conventions

Document names use lowercase with hyphens.

Examples:

```
architecture.md
deployment.md
event-model.md
security-model.md
```

Module documentation:

```
phantom-core.md
phantom-source.md
phantom-js.md
```

---

# Versioning

Architecture documentation evolves alongside the ecosystem.

Significant documentation changes should be reflected in:

```
CHANGELOG.md
```

Future ideas should be documented in:

```
ROADMAP.md
```

---

# Documentation Philosophy

Documentation should describe the ecosystem exactly as it exists.

It should never:

- Infer behavior
- Describe unimplemented features
- Speculate about future functionality
- Duplicate information unnecessarily

When implementation changes, documentation should be updated accordingly.

Accuracy is always preferred over completeness.
# Contributing to Phantom Ecosystem Documentation

## Purpose

Thank you for your interest in contributing to the Phantom Ecosystem documentation.

This repository documents the public architecture, design principles, deployment model, communication patterns, and module responsibilities of the Phantom Ecosystem.

The implementation source code remains private.

Contributions should focus exclusively on improving the accuracy, clarity, consistency, and maintainability of the public documentation.

---

## Contribution Scope

Contributions are welcome in the following areas:

- Documentation corrections
- Grammar and spelling improvements
- Terminology consistency
- Broken link fixes
- Diagram improvements
- Formatting corrections
- Clarification of documented architecture
- Improvements to module documentation
- Improvements to repository navigation

Contributions must remain aligned with the documented implementation.

---

## Out of Scope

The following contributions are not accepted through this repository:

- Private source code
- Internal service implementations
- Exploit code
- Vulnerability payloads
- Operational secrets
- Credentials
- Private infrastructure details
- Undocumented assumptions about the ecosystem
- Architectural changes not reflected in the real deployment

This repository must not be used to speculate about private implementation details.

---

## Documentation Principles

All contributions should follow the architectural and editorial principles of the repository.

### Accuracy

Documentation must describe the current implementation.

Do not document planned features as if they already exist.

Future capabilities should be clearly identified as proposed or planned.

---

### No Speculation

Do not infer internal behavior that is not publicly documented.

When implementation details are intentionally omitted, preserve that boundary.

---

### Consistency

Use the terminology defined by the repository.

Preferred architectural roles are:

- Producer
- Consumer
- Coordinator

Avoid replacing these terms with alternatives unless the architecture documentation is updated consistently.

---

### Clarity

Use direct and precise language.

Avoid unnecessary marketing language, exaggerated claims, and ambiguous statements.

Documentation should explain the architecture without overstating its capabilities.

---

### Security Awareness

Never include:

- Passwords
- API keys
- Webhook URLs
- Tokens
- Private IP addresses
- Internal credentials
- Sensitive environment variables
- Operational secrets

Example values must be clearly fictional and non-sensitive.

---

## Repository Structure

Documentation contributions should follow the existing repository structure.

```text
docs/
├── architecture.md
├── deployment.md
├── event-model.md
├── hive-mind.md
├── index.md
├── infrastructure.md
├── modules.md
├── philosophy.md
├── security-model.md
├── style-guide.md
└── modules/
```

Module-specific documentation belongs in:

```text
docs/modules/
```

Infrastructure examples belong in:

```text
infrastructure/
```

Research references belong in:

```text
research/
```

Images and diagrams belong in:

```text
assets/
```

or:

```text
docs/diagrams/
```

depending on their purpose.

---

## Before Making Changes

Before preparing a contribution:

1. Review the existing documentation.
2. Read `docs/style-guide.md`.
3. Confirm that the proposed change reflects the documented implementation.
4. Check whether the same information already exists elsewhere.
5. Avoid duplicating content across multiple files.

Architectural concepts should normally be defined once and referenced from related documents.

---

## Creating a Branch

Create a dedicated branch for each contribution.

Recommended branch naming:

```text
docs/update-architecture
docs/fix-broken-links
docs/improve-module-description
docs/add-event-diagram
```

Keep each branch focused on one logical change.

---

## Commit Messages

Use clear and descriptive commit messages.

Examples:

```text
docs: clarify Redis Pub/Sub behavior
docs: fix links in module documentation
docs: add correlation flow diagram
docs: standardize producer and consumer terminology
```

Avoid vague messages such as:

```text
update
changes
fix stuff
documentation
```

---

## Pull Requests

Each pull request should include:

- A clear title
- A concise description of the change
- The reason the change is needed
- The files affected
- Any architectural assumptions involved

For diagram changes, include a preview when possible.

Pull requests should remain small and focused.

Large unrelated changes should be separated into multiple pull requests.

---

## Documentation Style

Documentation must be written in English unless a specific section explicitly requires another language.

Use:

- Clear section headings
- Short paragraphs
- Consistent terminology
- Fenced code blocks
- Relative links for repository documents
- Plain technical language

Avoid:

- Excessive emojis
- Promotional language
- Unverified claims
- Unnecessary repetition
- Large blocks of unexplained text

---

## Markdown Guidelines

Use standard Markdown syntax.

### Headings

Use a single level-one heading per document.

```markdown
# Document Title
```

Use level-two headings for main sections.

```markdown
## Main Section
```

Use level-three headings for subsections.

```markdown
### Subsection
```

Do not skip heading levels unnecessarily.

---

### Code Blocks

Specify the language when applicable.

```markdown
```yaml
services:
  redis:
    image: redis:alpine
```
```

Use `text` for architecture flows and non-executable examples.

```markdown
```text
Producer
   ↓
Redis Pub/Sub
   ↓
Coordinator
```
```

---

### Links

Use relative links for internal documentation.

```markdown
[Architecture](../docs/architecture.md)
```

Check all links before submitting a contribution.

---

## Module Documentation

New module documentation should follow the established template.

Required sections include:

1. Purpose
2. Overview
3. Responsibilities
4. Scope
5. Architecture Role
6. Hive Mind Participation
7. Communication
8. Runtime Characteristics
9. State Management
10. Failure Behavior
11. Security Considerations
12. Design Principles
13. Current Limitations
14. Summary
15. Related Documentation

Do not claim that a module participates in the Hive Mind unless this is reflected in the current implementation.

---

## Diagram Contributions

Diagrams should:

- Match the current architecture
- Use consistent service names
- Distinguish infrastructure from communication and application layers
- Avoid representing future components as implemented
- Remain readable at repository preview size
- Include an editable source file when possible

Recommended formats:

- Mermaid
- Draw.io
- SVG
- PNG for rendered previews

Sensitive infrastructure information must not appear in public diagrams.

---

## Validation Checklist

Before submitting a contribution, verify:

- The information is accurate.
- No private implementation details were exposed.
- No credentials or secrets were included.
- Terminology matches the rest of the repository.
- Internal links work.
- Headings follow the style guide.
- The change does not duplicate existing documentation.
- Planned features are clearly identified as future work.
- The documentation reflects the current deployment.

---

## Reporting Documentation Issues

Documentation issues may include:

- Inaccurate architecture descriptions
- Broken links
- Missing references
- Contradictory terminology
- Outdated deployment information
- Incorrect module roles
- Diagram inconsistencies

Security-sensitive issues should not be reported publicly.

Follow the instructions in:

```text
SECURITY.md
```

for responsible security reporting.

---

## Review Process

Contributions are reviewed for:

- Technical accuracy
- Architectural consistency
- Security implications
- Documentation quality
- Scope alignment

A contribution may be rejected if it introduces speculation, exposes private details, or describes functionality that is not currently implemented.

---

## License

By contributing to this repository, you agree that your contribution will be distributed under the repository license.

See:

```text
LICENSE
```

for details.

---

## Summary

Contributions to the Phantom Ecosystem documentation should improve accuracy, clarity, and consistency while preserving the separation between public architecture documentation and private implementation details.

Every contribution should reflect the real system, avoid speculation, and follow the established documentation standards.
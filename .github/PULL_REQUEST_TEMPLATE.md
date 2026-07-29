# Pull Request

## Summary

Provide a concise description of the changes introduced by this pull request.

Explain what was changed and why the change is necessary.

---

## Type of Change

Select the category that best describes this contribution.

- [ ] Documentation correction
- [ ] Documentation clarification
- [ ] New documentation
- [ ] Diagram update
- [ ] Broken link fix
- [ ] Terminology standardization
- [ ] Architecture Decision Record
- [ ] Repository maintenance
- [ ] Other

---

## Files Changed

List the main files affected by this pull request.

```text
docs/example.md
docs/modules/example.md
```

---

## Architectural Impact

Does this contribution modify or reinterpret the documented architecture?

- [ ] No architectural impact
- [ ] Clarifies an existing architectural decision
- [ ] Updates an existing architectural description
- [ ] Introduces a proposed architectural change
- [ ] Requires a new Architecture Decision Record

Describe the impact below.

```text
No architectural impact.
```

---

## Implementation Status

Confirm how the documented behavior relates to the real implementation.

- [ ] The change documents functionality that is currently implemented.
- [ ] The change describes future work and is clearly marked as planned.
- [ ] The change contains no assumptions about private implementation details.
- [ ] The change has been verified against the current deployment.

---

## Documentation Consistency

Confirm that the contribution follows the repository terminology and documentation standards.

- [ ] I reviewed `docs/style-guide.md`.
- [ ] I used the established terms `Producer`, `Consumer`, and `Coordinator`.
- [ ] I avoided duplicating information already documented elsewhere.
- [ ] I used relative links for internal documentation.
- [ ] I checked the affected links.
- [ ] I followed the existing heading structure.
- [ ] I used English for public documentation.

---

## Security Review

Confirm that no sensitive information is included.

- [ ] No passwords are included.
- [ ] No API keys or tokens are included.
- [ ] No webhook URLs are included.
- [ ] No private IP addresses are included.
- [ ] No credentials are included.
- [ ] No private source code is included.
- [ ] No confidential infrastructure details are included.
- [ ] Example values are fictional and non-sensitive.

---

## Diagram Review

Complete this section only when the pull request modifies diagrams.

- [ ] The diagram represents the current architecture.
- [ ] Service names match the documentation.
- [ ] Future components are clearly marked.
- [ ] The diagram is readable in the GitHub preview.
- [ ] An editable source is included when applicable.
- [ ] The diagram contains no sensitive infrastructure information.

Not applicable:

- [ ] This pull request does not modify diagrams.

---

## ADR Review

Complete this section only when adding or modifying an Architecture Decision Record.

- [ ] The ADR has a unique sequential number.
- [ ] The ADR includes a status.
- [ ] The ADR explains the context.
- [ ] The ADR records the decision.
- [ ] The ADR explains positive and negative consequences.
- [ ] The ADR does not modify an accepted decision retroactively.
- [ ] A new ADR supersedes the previous decision when applicable.

Not applicable:

- [ ] This pull request does not modify an ADR.

---

## Validation

Describe how the contribution was reviewed.

Examples:

- Markdown preview checked locally
- GitHub Mermaid rendering verified
- Internal links manually tested
- Terminology compared with `docs/architecture.md`
- Deployment information compared with `docker-compose.yml`

```text
Describe the validation performed.
```

---

## Related Issues

Reference any related issues.

```text
Closes #
Related to #
```

Write `Not applicable` when no issue exists.

---

## Additional Notes

Include any information that may help reviewers understand the contribution.

```text
No additional notes.
```

---

## Final Checklist

- [ ] The pull request has a clear title.
- [ ] The change is focused on one logical topic.
- [ ] The documentation reflects the real implementation.
- [ ] Planned features are not presented as implemented.
- [ ] No speculative private behavior was added.
- [ ] No sensitive information was exposed.
- [ ] The contribution follows `CONTRIBUTING.md`.
- [ ] The contribution follows the repository license.
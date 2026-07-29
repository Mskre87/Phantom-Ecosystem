# Research

The `research/` directory is reserved for sanitized public research notes, technical references, and supporting material related to the domains covered by the Phantom Ecosystem.

It does not contain the private implementation, operational findings, active target data, exploit procedures, or internal research tooling used by the running ecosystem.

---

## Purpose

This area provides a public location for material that supports the architectural and technical documentation without exposing sensitive operational information.

Suitable content includes:

- Public technical references
- Sanitized research notes
- High-level technology evaluations
- Public papers and standards
- Non-operational tool references
- General lessons that do not reveal active targets or private methods

---

## Directory Structure

```text
research/
├── ai/
├── binaries/
├── mobile/
├── papers/
├── references/
├── tools/
└── web3/
```

| Directory | Intended content |
|-----------|------------------|
| `ai/` | Sanitized notes and public references related to AI and machine-learning security. |
| `binaries/` | Public material related to binary analysis and reverse-engineering concepts. |
| `mobile/` | Public references related to Android and mobile application security. |
| `papers/` | Public papers, bibliographic notes, and reading summaries. |
| `references/` | General standards, documentation, and external technical references. |
| `tools/` | Public documentation and non-operational notes about relevant security tools. |
| `web3/` | Public references related to smart-contract and blockchain security. |

---

## Public Repository Boundary

The following content must not be added to this directory:

- Private source code
- Live credentials or tokens
- Operational webhook URLs
- Private target lists or active scope files
- Unpublished vulnerability reports
- Raw findings from the running services
- Exploit chains or operational attack instructions
- Internal automation scripts
- Private research methodologies
- Files copied from the isolated implementation repositories

The public documentation repository remains separate from the functioning Phantom services and their private operational environment.

---

## Current Status

The category structure is prepared for future sanitized material. No operational research entries are currently published in this repository.

For implemented architecture and module behavior, refer to:

- [`../docs/index.md`](../docs/index.md)
- [`../docs/architecture.md`](../docs/architecture.md)
- [`../docs/modules.md`](../docs/modules.md)
- [`../SECURITY.md`](../SECURITY.md)

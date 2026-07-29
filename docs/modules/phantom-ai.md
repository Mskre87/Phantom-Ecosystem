<p align="center">
  <img src="../../assets/icons/phantom-ai.png" width="120" alt="Phantom AI">
</p>

# Phantom AI

## Purpose

This document describes Phantom AI, a Phantom Ecosystem service focused on machine-learning repository security analysis.

Phantom AI monitors selected machine-learning repositories and inspects changed code or model-related artifacts for unsafe deserialization patterns and exposed service credentials.

---

## Overview

Phantom AI is the machine-learning security monitoring service of the Phantom Ecosystem.

It watches configured AI framework repositories, examines relevant changes such as Python and notebook files, applies targeted checks for unsafe object loading, scans for exposed OpenAI or Hugging Face credentials, and reports findings independently.

---

## Responsibilities

The module is designed to:

- Monitor configured machine-learning repositories
- Inspect changed Python and notebook content
- Detect unsafe deserialization patterns such as unprotected `pickle.load` or `torch.load` usage
- Detect exposed AI service credentials
- Report findings independently
- Maintain lightweight local monitoring state

---

## Scope

The module operates within the following scope:

- Public AI and machine-learning repository changes
- Python and notebook files
- Relevant serialized model references and artifacts
- Configured AI targets

It is **not** responsible for:

- Training or serving machine-learning models
- Executing untrusted serialized objects
- General dataset analysis
- Hive event publication or task consumption
- Cross-module correlation

---

## Architecture Role

**Domain:** Machine learning repository security analysis  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `ai_monitor.py` | Monitors repository changes, filters relevant files, applies unsafe-deserialization and secret-detection checks, and reports findings. |
| `ai_targets.json` | Stores the configured machine-learning repositories. |
| `Dockerfile` | Defines the isolated Python runtime. |

---

## Workflow

1. Poll configured AI repositories for relevant changes.
2. Select supported Python, notebook, or model-related files.
3. Inspect changed content for unsafe deserialization patterns.
4. Scan for exposed AI platform credentials.
5. Report relevant findings to Discord.
6. Update local monitoring state when configured.

---

## Inputs

- Repository API responses and changed file content
- `ai_targets.json`
- Runtime API and Discord configuration
- Local monitoring state

---

## Outputs

- Discord alerts and analysis summaries
- Updated lightweight monitoring state
- No executed untrusted model payloads

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom AI does not currently participate in the Hive Mind. It performs no active Redis event publication or delegated task consumption.

---

## Communication

- Outbound repository API requests
- Outbound Discord webhook requests
- Local configuration and state files
- No active Redis Pub/Sub communication

---

## Dependencies

- Python 3
- `aiohttp`
- Regular expressions
- Docker

---

## Configuration and State

- `ai_targets.json` contains monitored repositories.
- A lightweight local state file can record processed changes.
- The module does not share state with other services.
- Redis is not used for persistence.

---

## Runtime Characteristics

- Independent Docker container
- Asynchronous repository monitoring
- Primarily network- and text-analysis workload
- Direct Discord reporting
- Shared Contabo VPS deployment

---

## Failure Behavior

- Repository API failures pause only the affected target.
- Malformed notebooks or unsupported artifacts should not stop later processing.
- Module downtime does not affect other services or Hive chains.
- No Redis replay mechanism exists because the module is not an active participant.

---

## Security Considerations

- The service must never execute untrusted serialized objects during inspection.
- Operational tokens and private target lists remain outside the public repository.
- Only public and authorized repository content should be analyzed.
- The complete rule set and live findings are intentionally omitted.

---

## Current Limitations

- Detection is based on static patterns and requires manual validation.
- Notebook and serialized-artifact formats can limit text extraction.
- The module does not participate in Redis correlation.
- The public repository excludes source code, target lists, credentials, and operational findings.

---

## Summary

Phantom AI autonomously monitors machine-learning repositories, detects unsafe deserialization patterns and exposed AI credentials, and reports relevant findings directly to Discord without participating in the current Hive Mind flow.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

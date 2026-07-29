<p align="center">
  <img src="../../assets/icons/phantom-supply.png" width="120" alt="Phantom Supply">
</p>

# Phantom Supply

## Purpose

This document describes Phantom Supply, a Phantom Ecosystem service focused on software supply chain and dependency confusion validation.

Phantom Supply inspects package manifests and registry availability to identify potential dependency confusion conditions, both through autonomous monitoring and tasks delegated by Phantom Correlation.

---

## Overview

Phantom Supply is the dependency confusion analysis service of the Phantom Ecosystem.

It examines package names extracted from recent repository activity or delegated Hive tasks, queries public package registries, and reports names that appear internal but are unavailable publicly and therefore require responsible validation.

The module reports its own findings directly to Discord and does not send completion events back to Phantom Correlation.

---

## Responsibilities

The module is designed to:

- Monitor configured repositories or package manifests
- Extract dependency names from files such as `package.json`
- Query supported public package registries
- Validate delegated package intelligence from `phantom_supply_tasks`
- Report potential dependency confusion conditions independently
- Maintain lightweight local processing state

---

## Scope

The module operates within the following scope:

- Recent repository changes and package manifests
- Configured supply-chain targets
- Public NPM and PyPI registry responses
- Delegated package tasks from Phantom Correlation

It is **not** responsible for:

- Registering package names
- Publishing proof-of-concept packages
- Executing code in third-party environments
- Publishing results back to `phantom_hive`
- Tracking task completion centrally

---

## Architecture Role

**Domain:** Software supply chain and dependency confusion validation  
**Hive role:** Consumer

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `supply_monitor.py` | Extracts dependency names, queries public registries, consumes delegated Supply tasks, validates results, reports findings, and updates local state. |
| `supply_targets.json` | Stores repositories or package ecosystems monitored autonomously. |
| `Dockerfile` | Defines the isolated Python runtime. |

---

## Workflow

1. Collect package names from configured repository activity or receive a delegated task.
2. Normalize the package and registry information.
3. Query the relevant public registry.
4. Identify names that are unavailable publicly and require manual validation.
5. Report the result directly to Discord.
6. Update local state without returning a completion event to Correlation.

---

## Inputs

- Package manifests and recent repository changes
- `supply_targets.json`
- NPM and PyPI registry responses
- Redis tasks from `phantom_supply_tasks`
- Runtime API and Discord configuration

---

## Outputs

- Discord alerts for potential dependency confusion conditions
- Updated lightweight local state
- No result event returned to Phantom Correlation

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: Yes
- Coordinator: No

Phantom Supply participates as a consumer. It subscribes to `phantom_supply_tasks`, does not publish intelligence to `phantom_hive`, and reports delegated results directly to Discord.

---

## Communication

- Outbound repository and registry requests
- Redis subscription to `phantom_supply_tasks`
- Outbound Discord webhook requests
- No direct calls to Phantom Source or Phantom Correlation

---

## Dependencies

- Python 3
- `aiohttp`
- Redis client integration
- NPM and PyPI APIs
- Docker

---

## Configuration and State

- `supply_targets.json` contains autonomous monitoring targets.
- A local state file can record processed repository or package checks.
- Delegated tasks are ephemeral and are not persisted by Redis.
- No centralized task history exists.

---

## Runtime Characteristics

- Independent Docker container
- Autonomous monitoring plus event-triggered task processing
- Redis consumer
- Direct Discord reporting
- Shared Contabo VPS deployment

---

## Failure Behavior

- Autonomous monitoring can continue when Correlation is unavailable if registry and repository dependencies remain available.
- Tasks published while Supply is offline are lost because Redis Pub/Sub is ephemeral.
- Registry outages delay validation but do not affect other modules.
- Phantom Correlation does not detect missed or failed tasks.

---

## Security Considerations

- Potential dependency confusion conditions must be handled through responsible disclosure and authorized validation.
- The public documentation does not endorse registering or publishing third-party package names.
- Registry credentials, target lists, and operational webhooks remain outside the public repository.
- Result evidence should be minimized.

---

## Current Limitations

- Registry availability alone does not prove exploitability.
- Delegated tasks have no acknowledgement, retry, or replay.
- The module reports results independently and does not return completion status to Correlation.
- The public repository excludes source code, private targets, and operational findings.

---

## Summary

Phantom Supply autonomously inspects package manifests and also consumes delegated package tasks. It validates public registry availability, reports potential dependency confusion conditions directly to Discord, and does not return results to the stateless Correlation Engine.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

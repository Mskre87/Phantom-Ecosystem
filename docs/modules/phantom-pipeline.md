<p align="center">
  <img src="../../assets/icons/phantom-pipeline.png" width="120" alt="Phantom Pipeline">
</p>

# Phantom Pipeline

## Purpose

This document describes Phantom Pipeline, a Phantom Ecosystem service focused on CI/CD pipeline and build-log inspection.

Phantom Pipeline monitors GitHub Actions workflow runs for configured repositories, downloads build logs, and detects credentials accidentally exposed in CI/CD output.

---

## Overview

Phantom Pipeline is the CI/CD log inspection service of the Phantom Ecosystem.

It polls the GitHub Actions API for recent workflow runs, retrieves raw console logs, applies secret-detection rules for common cloud, package, payment, and container credentials, and reports relevant findings directly to Discord.

Despite its name, the module does not orchestrate workflows across the ecosystem.

---

## Responsibilities

The module is designed to:

- Monitor configured GitHub Actions workflow runs
- Download raw CI/CD console logs
- Detect exposed AWS, NPM, Stripe, Docker, and related credentials
- Avoid repeated processing through local monitoring state
- Report findings independently
- Operate without dispatching tasks to other modules

---

## Scope

The module operates within the following scope:

- Public GitHub Actions workflow-run metadata
- Raw workflow logs
- Configured repository targets
- Supported secret formats

It is **not** responsible for:

- Creating or managing CI/CD workflows
- Dispatching tasks to Phantom modules
- Tracking ecosystem-wide pipelines
- Hive event publication or task consumption
- Executing build artifacts

---

## Architecture Role

**Domain:** CI/CD pipeline and build-log inspection  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `pipeline_monitor.py` | Polls GitHub Actions workflow runs, downloads logs, scans output for supported secrets, reports findings, and updates local state. |
| `pipeline_targets.json` | Stores the repositories whose workflow runs are monitored. |
| `Dockerfile` | Defines the isolated Python runtime. |

---

## Workflow

1. Poll `api.github.com/repos/{target}/actions/runs` for configured repositories.
2. Identify workflow runs that have not been processed.
3. Download the corresponding raw console logs.
4. Apply the supported credential-detection rules.
5. Report validated findings to Discord.
6. Update local monitoring state.

---

## Inputs

- GitHub Actions API responses
- Workflow log archives or raw log content
- `pipeline_targets.json`
- GitHub and Discord runtime configuration
- Local processed-run state

---

## Outputs

- Discord alerts with repository, workflow, and limited log context
- Updated lightweight workflow-run state
- No workflow orchestration commands

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom Pipeline does not currently participate in the Hive Mind. It does not publish events, consume delegated tasks, or coordinate other modules.

---

## Communication

- Outbound GitHub Actions API and log requests
- Outbound Discord webhook requests
- Local configuration and state files
- No active Redis Pub/Sub communication

---

## Dependencies

- Python 3
- `aiohttp`
- `asyncio`
- Regular expressions
- Docker

---

## Configuration and State

- `pipeline_targets.json` contains monitored repositories.
- A local state file records processed workflow runs to reduce duplicate analysis.
- Operational credentials remain in environment configuration.
- Redis is not used for module state.

---

## Runtime Characteristics

- Independent Docker container
- Continuous asynchronous API polling
- Network- and text-analysis workload
- Direct Discord reporting
- Shared Contabo VPS deployment

---

## Failure Behavior

- GitHub API outages or rate limits delay new log inspection.
- A malformed or unavailable log should not stop later workflow runs.
- Module downtime does not affect other services or Hive chains.
- Missed workflow runs depend on local state and API retention rather than Redis replay.

---

## Security Considerations

- GitHub and Discord credentials remain outside the public repository.
- Log excerpts should be minimized because they can contain sensitive values.
- Only public and authorized repositories should be monitored.
- The full detection rules and target list are intentionally omitted.

---

## Current Limitations

- Log availability and permissions depend on the GitHub API.
- Large logs can increase network and processing cost.
- Static secret patterns require manual validation.
- The module does not orchestrate workflows and does not participate in the active Hive flow.

---

## Summary

Phantom Pipeline is an autonomous GitHub Actions log scanner. It monitors workflow runs, downloads build logs, detects exposed CI/CD credentials, tracks processed runs locally, and reports findings directly to Discord.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

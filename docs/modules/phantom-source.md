<p align="center">
  <img src="../../assets/icons/phantom-source.png" width="120" alt="Phantom Source">
</p>

# Phantom Source

## Purpose

This document describes Phantom Source, a Phantom Ecosystem service focused on source code repository intelligence.

Phantom Source monitors GitHub global events, filters relevant push activity, downloads commit patches, and detects exposed secrets in newly published source-code changes.

---

## Overview

Phantom Source is the GitHub Firehose monitoring service of the Phantom Ecosystem.

It polls the public GitHub Events API, filters matching `PushEvent` activity using target keywords, retrieves the corresponding `.patch` content, and applies regular-expression rules to newly added or modified lines.

Findings are reported directly to Discord, while supported package or dependency intelligence can also be published to the Hive Mind for delegated validation.

---

## Responsibilities

The module is designed to:

- Poll the GitHub Events API
- Filter repository activity using configured keywords
- Download matching commit patches
- Detect exposed keys, tokens, credentials, and related source-code findings
- Prevent duplicate processing with an in-memory LRU cache
- Report findings directly to Discord
- Publish supported normalized intelligence to `phantom_hive`

---

## Scope

The module operates within the following scope:

- Public GitHub push events
- Commit patch content
- Configured company, domain, repository, and author keywords
- Secret and package-related detection patterns

It is **not** responsible for:

- Cloning complete repositories
- Persisting patch files in a local loot directory
- Executing delegated dependency validation
- Tracking downstream task completion
- Correlating events across modules

---

## Architecture Role

**Domain:** Source code repository intelligence  
**Hive role:** Producer

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `source_monitor.py` | Polls GitHub events, filters push activity, downloads patches, scans changes, performs edge deduplication, reports findings, and publishes supported Hive events. |
| `targets_keywords.json` | Stores the keyword filters used to identify relevant GitHub activity. |
| `Dockerfile` / `requirements.txt` | Define the container runtime and Python dependencies. |

---

## Workflow

1. Poll `https://api.github.com/events`.
2. Select relevant `PushEvent` records using configured keywords.
3. Build and download the matching commit `.patch`.
4. Scan added or modified content for supported findings.
5. Reject commit SHAs already present in the local LRU cache.
6. Send findings to Discord.
7. Publish supported package or dependency intelligence to `phantom_hive`.

---

## Inputs

- GitHub Events API responses
- Commit patch content
- Keyword configuration
- GitHub API credentials when required
- Discord and Redis runtime configuration

---

## Outputs

- Discord webhook alerts with repository, commit, and limited patch context
- Normalized Hive events for supported package or dependency findings
- No persistent local loot files

---

## Hive Mind Participation

Current role:

- Producer: Yes
- Consumer: No
- Coordinator: No

Phantom Source participates as a producer. It publishes supported normalized intelligence to `phantom_hive` and does not subscribe to delegated task channels.

---

## Communication

- Outbound GitHub API and patch requests
- Outbound Discord webhook requests
- Redis publication to `phantom_hive`
- No direct service-to-service calls

---

## Dependencies

- Python 3
- `aiohttp`
- `asyncio`
- `re`
- `json`
- Redis client integration
- Docker

---

## Configuration and State

- Target keywords are loaded from local configuration.
- An in-memory LRU cache retains up to 5,000 recent commit SHAs to suppress duplicate alerts.
- The cache is edge-level runtime state and is cleared when the process restarts.
- Patch files are not retained in a local `loot/` directory.

---

## Runtime Characteristics

- Independent Docker container
- Continuous asynchronous GitHub polling
- Redis producer
- Direct Discord reporting
- Low persistent disk usage

---

## Failure Behavior

- GitHub API outages or rate limits pause new collection but do not stop other modules.
- A Redis outage prevents Hive publication while direct source monitoring and Discord reporting can continue when their dependencies remain available.
- Events published while Phantom Correlation is offline are lost because Redis Pub/Sub is ephemeral.

---

## Security Considerations

- GitHub and Discord credentials remain outside the public repository.
- Only public repository activity and authorized research targets should be processed.
- Patch evidence included in alerts should be minimized to the amount required for validation.
- Private keyword lists and complete detection rules are not published.

---

## Current Limitations

- The GitHub Events API is polled rather than consumed through a guaranteed stream.
- The LRU cache is volatile and does not survive a restart.
- Redis publication is fire-and-forget and has no replay.
- The public documentation omits private keyword configuration and the full scanning rule set.

---

## Summary

Phantom Source continuously inspects relevant GitHub push activity, scans commit patches for exposed secrets, suppresses repeated commit alerts with a bounded in-memory cache, reports findings to Discord, and publishes selected intelligence to the Hive Mind as a producer.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

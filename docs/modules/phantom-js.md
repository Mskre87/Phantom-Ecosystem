<p align="center">
  <img src="../../assets/icons/phantom-js.png" width="120" alt="Phantom JS">
</p>

# Phantom JS

## Purpose

This document describes Phantom JS, a Phantom Ecosystem service focused on frontend JavaScript security intelligence.

Phantom JS crawls configured production websites, downloads linked JavaScript bundles, extracts hidden endpoints and hardcoded client-side secrets, and publishes supported intelligence to the Hive Mind.

---

## Overview

Phantom JS is the frontend JavaScript analysis service of the Phantom Ecosystem.

It runs approximately every six hours, parses target HTML to discover `<script src>` resources, downloads bundled JavaScript produced by frameworks such as React, Vue, or Webpack, and applies targeted extraction rules for internal endpoints, bearer tokens, and client-side configuration.

Findings are reported directly to Discord, while supported token and endpoint intelligence is normalized and published to `phantom_hive`.

---

## Responsibilities

The module is designed to:

- Run scheduled scans of configured production websites
- Extract JavaScript bundle URLs from HTML
- Download and inspect frontend bundles
- Detect hidden API paths, hardcoded tokens, and Firebase-style configuration
- Report findings directly to Discord
- Publish supported normalized intelligence to `phantom_hive`
- Maintain lightweight local scan state

---

## Scope

The module operates within the following scope:

- Public production HTML
- Linked JavaScript bundles
- Configured web targets
- Supported endpoint, token, and client-side configuration patterns

It is **not** responsible for:

- Node.js dependency analysis
- Server-side source-code cloning
- Executing discovered administrative actions
- Consuming delegated task channels
- Tracking GraphQL task completion

---

## Architecture Role

**Domain:** Frontend JavaScript security intelligence  
**Hive role:** Producer

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `js_monitor.py` | Runs scheduled scans, extracts script URLs, downloads bundles, applies detection rules, reports findings, publishes supported Hive events, and updates local state. |
| `js_targets.json` | Stores the configured production web targets. |
| `Dockerfile` | Defines the isolated Python runtime. |

---

## Workflow

1. Load configured targets on the scheduled interval.
2. Request each target page.
3. Parse HTML and extract `<script src>` URLs.
4. Download the referenced JavaScript bundles.
5. Apply endpoint, token, and configuration extraction rules.
6. Send findings to Discord.
7. Publish supported bearer-token or administrative-endpoint intelligence to `phantom_hive`.

---

## Inputs

- `js_targets.json`
- HTML pages and JavaScript bundle content
- Runtime Discord and Redis configuration
- Local scan state

---

## Outputs

- Discord alerts with source asset and limited evidence
- Normalized events published to `phantom_hive`
- Updated lightweight scan state

---

## Hive Mind Participation

Current role:

- Producer: Yes
- Consumer: No
- Coordinator: No

Phantom JS participates as a producer. It publishes supported normalized intelligence to `phantom_hive` and does not subscribe to delegated task channels.

---

## Communication

- Outbound HTTP and HTTPS requests to target websites and bundles
- Redis publication to `phantom_hive`
- Outbound Discord webhook requests
- No direct calls to Phantom GraphQL

---

## Dependencies

- Python 3
- `aiohttp`
- `BeautifulSoup4`
- Regular expressions
- Redis client integration
- Docker

---

## Configuration and State

- `js_targets.json` contains scan targets.
- A local state file can record processed assets or scan metadata.
- The module runs approximately every six hours.
- Published Redis events are not persisted.

---

## Runtime Characteristics

- Independent Docker container
- Scheduled asynchronous web crawling
- Redis producer
- Direct Discord reporting
- Shared Contabo VPS deployment

---

## Failure Behavior

- A failed target or bundle request should not stop the remaining scan cycle.
- A Redis outage prevents correlation publication while direct scanning and Discord reporting can continue when available.
- Events published while Correlation is offline are lost.
- Local scan state prevents some repeated processing but is not a centralized event history.

---

## Security Considerations

- Only authorized public targets should be scanned.
- Discovered tokens and sensitive values must be handled as security findings and kept out of public documentation.
- Operational webhooks, targets, and full extraction rules remain private.
- The service should avoid using discovered credentials outside authorized validation.

---

## Current Limitations

- Minified or dynamically loaded code can reduce extraction coverage.
- Static pattern matching requires manual validation.
- Redis publication has no acknowledgement, replay, or guaranteed delivery.
- The public repository excludes private target lists, source code, and live findings.

---

## Summary

Phantom JS performs scheduled frontend bundle analysis, extracts hidden endpoints and client-side secrets, reports findings to Discord, and publishes selected token or endpoint intelligence to the Hive Mind as a producer.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

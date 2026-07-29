<p align="center">
  <img src="../../assets/icons/phantom-core.png" width="120" alt="Phantom Core">
</p>

# Phantom Core

## Purpose

This document describes Phantom Core, a Phantom Ecosystem service focused on web asset reconnaissance and secret discovery.

Phantom Core monitors configured public web targets, crawls linked assets, and detects exposed credentials, API keys, tokens, and database connection strings.

---

## Overview

Phantom Core is the web crawling and secret scanning service of the Phantom Ecosystem.

It requests target pages asynchronously, parses HTML, follows relevant links and assets, downloads selected resources, applies regular-expression detection rules, and sends structured findings to Discord.

The service also includes certificate and target-discovery support for expanding authorized monitoring coverage.

---

## Responsibilities

The module is designed to:

- Monitor configured public web targets
- Parse HTML and extract linked resources
- Inspect potentially sensitive files and raw asset contents
- Detect exposed secrets and connection strings
- Monitor certificate-related subdomain discoveries
- Update target information from public bug bounty sources
- Report validated findings through Discord webhooks

---

## Scope

The module operates within the following scope:

- Public websites and linked web assets
- Configuration and text resources such as `.env`, `.js`, `.json`, and `.txt`
- Known secret formats and suspicious credential patterns
- Certificate-related target expansion

It is **not** responsible for:

- GitHub global event monitoring
- Binary reverse engineering
- APK decompilation
- Smart contract analysis
- CI/CD log inspection
- GraphQL validation
- Cross-module event correlation

---

## Architecture Role

**Domain:** Web asset reconnaissance and secret discovery  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `scanner.py` | Implements asynchronous crawling, URL extraction, content retrieval, and regular-expression scanning. |
| `cert_monitor.py` | Monitors certificate-related information and newly observed subdomains associated with configured targets. |
| `fetch_targets.py` | Retrieves and updates authorized target information from public bug bounty sources. |
| `notifier.py` | Formats and delivers findings through Discord webhooks. |
| `targets.json` | Stores the target domains and scopes used by the scanner. |

---

## Workflow

1. Load the configured targets and scopes.
2. Request target pages asynchronously.
3. Parse `<a>`, `<script>`, and `<link>` references.
4. Filter and retrieve relevant linked resources.
5. Apply the secret-detection rules to raw content.
6. Send validated findings to the dedicated Discord channel.

---

## Inputs

- Authorized target domains and scopes from `targets.json`
- HTTP and HTTPS responses from public web assets
- Certificate-related target information
- Runtime webhook configuration

---

## Outputs

- Structured Discord alerts containing the finding type, source URL, and limited evidence
- Updated local target or certificate-monitoring information when implemented

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom Core does not currently participate in the active Hive Mind event flow. It neither publishes to `phantom_hive` nor consumes delegated task channels.

---

## Communication

- Outbound HTTP and HTTPS requests to authorized public targets
- Outbound Discord webhook requests
- Local configuration and state files
- No active Redis Pub/Sub communication at the application level

---

## Dependencies

- Python 3
- `aiohttp`
- `asyncio`
- `BeautifulSoup4`
- Regular expressions
- Docker

---

## Configuration and State

- `targets.json` provides persistent target configuration.
- Supporting monitoring components may keep lightweight local state.
- Redis is not used as a persistence layer for this module.

---

## Runtime Characteristics

- Independent Docker container
- Continuous asynchronous network workload
- Shared Contabo VPS deployment
- Internal attachment to `phantom_net`
- Direct Discord reporting

---

## Failure Behavior

- Individual HTTP, DNS, certificate, parsing, or rate-limit failures should not stop the complete scan cycle.
- A module outage stops Core scanning only; the remaining services continue operating.
- Because Core is not an active Hive participant, its failure does not interrupt Redis delegation chains.

---

## Security Considerations

- Targets must remain within authorized security-research scope.
- Webhook credentials and operational values are supplied outside the public repository.
- The public documentation omits private target inventories and the complete detection rule set.
- The container exposes no public administrative endpoint.

---

## Current Limitations

- The active implementation does not publish Core findings to the Hive Mind.
- The public repository excludes private targets, operational webhooks, source code, and the complete regular-expression library.
- Remote target availability, rate limits, and malformed assets can reduce scan coverage.

---

## Summary

Phantom Core is an autonomous asynchronous web crawler and secret scanner. It discovers relevant public web assets, identifies exposed credentials and configuration data, and reports findings directly to Discord without participating in the current Redis event flow.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

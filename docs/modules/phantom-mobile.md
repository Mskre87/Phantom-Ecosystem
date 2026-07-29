<p align="center">
  <img src="../../assets/icons/phantom-mobile.png" width="120" alt="Phantom Mobile">
</p>

# Phantom Mobile

## Purpose

This document describes Phantom Mobile, a Phantom Ecosystem service focused on android application security analysis.

Phantom Mobile monitors selected GitHub Releases for Android packages, decompiles downloaded APKs with JADX, and scans the recovered source and manifest for security-relevant findings.

---

## Overview

Phantom Mobile is the Android analysis service of the Phantom Ecosystem.

It watches configured release sources, downloads new APK artifacts, invokes JADX, examines Java or Kotlin output and `AndroidManifest.xml`, reports relevant findings, and deletes the APK and decompiled source after analysis to control disk usage.

---

## Responsibilities

The module is designed to:

- Monitor configured Android application releases
- Download newly published APK artifacts
- Decompile APKs with JADX
- Inspect recovered source code and Android manifests
- Detect embedded credentials, exported components, and cleartext traffic configuration
- Report findings independently
- Remove APK and decompiled output after analysis

---

## Scope

The module operates within the following scope:

- Public APK release artifacts
- Java and Kotlin output produced by JADX
- `AndroidManifest.xml`
- Configured mobile application targets

It is **not** responsible for:

- iOS application analysis
- Dynamic device testing
- Malware execution
- Hive event publication or task consumption
- Long-term retention of decompiled applications

---

## Architecture Role

**Domain:** Android application security analysis  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `mobile_monitor.py` | Monitors releases, downloads APKs, invokes JADX, scans decompiled files and manifests, reports findings, and performs cleanup. |
| `mobile_targets.json` | Stores the monitored Android release targets. |
| `Dockerfile` | Builds the Java, JADX, and Python runtime required by the service. |

---

## Workflow

1. Poll configured GitHub Release sources.
2. Detect a new APK artifact.
3. Download the APK to temporary storage.
4. Invoke JADX to decompile the package.
5. Inspect Java, Kotlin, configuration, and manifest content.
6. Report relevant findings to Discord.
7. Delete the APK and decompiled output.

---

## Inputs

- Release API responses
- APK files
- `mobile_targets.json`
- Runtime API and Discord configuration
- Local release state

---

## Outputs

- Discord alerts and analysis summaries
- Temporary decompiled source and manifest data
- Updated lightweight release-monitoring state

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom Mobile does not currently participate in the Hive Mind. It neither publishes Redis events nor consumes delegated tasks.

---

## Communication

- Outbound release and artifact downloads
- Local JADX subprocess execution
- Outbound Discord webhook requests
- No active Redis Pub/Sub communication

---

## Dependencies

- Python 3
- `aiohttp`
- `subprocess`
- Java 17
- JADX
- Docker

---

## Configuration and State

- `mobile_targets.json` contains monitored release sources.
- A lightweight local state file can record processed releases.
- Downloaded APKs and decompiled sources are temporary and deleted after analysis.
- Redis is not used for module state.

---

## Runtime Characteristics

- Independent heavyweight Docker container
- Network-, CPU-, memory-, and disk-intensive APK processing
- Temporary high disk usage
- Direct Discord reporting
- Shared Contabo VPS deployment

---

## Failure Behavior

- A failed APK download or decompilation affects only the current target.
- Cleanup failures can temporarily increase disk usage and should be monitored.
- Module downtime does not affect other services or Hive chains.
- Interrupted analyses are not replayed through Redis.

---

## Security Considerations

- Only public and authorized application releases should be processed.
- APKs and decompiled content remain isolated inside the module environment.
- Operational credentials and private target lists remain outside the public repository.
- Temporary artifacts are removed to reduce retention and disk exposure.

---

## Current Limitations

- JADX output may be incomplete or affected by obfuscation.
- Large APKs can consume substantial resources.
- The service currently supports Android artifacts only.
- The module does not participate in the active Hive Mind flow.

---

## Summary

Phantom Mobile autonomously monitors Android releases, downloads and decompiles APKs with JADX, scans recovered source and manifests, reports relevant findings to Discord, and removes temporary artifacts after analysis.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

<p align="center">
  <img src="../../assets/icons/phantom-binary.png" width="120" alt="Phantom Binary">
</p>

# Phantom Binary

## Purpose

This document describes Phantom Binary, a Phantom Ecosystem service focused on binary monitoring and reverse engineering.

Phantom Binary monitors desktop software for silent updates, extracts selected executable artifacts, and runs controlled headless Ghidra analysis through a Discord-operated controller.

---

## Overview

Phantom Binary combines an asynchronous update radar, a Discord command interface, process supervision, archive extraction, and two Ghidra analysis modes.

The radar compares remote metadata such as ETag, Last-Modified, and content length. When an update is detected, it downloads the package and can use 7-Zip to extract only the configured executable.

The Discord controller can start full or lightweight Ghidra analysis, inspect active Java processes, and terminate runaway analysis jobs.

---

## Responsibilities

The module is designed to:

- Monitor configured software targets for updates
- Download changed installers or archives
- Extract configured executable targets with 7-Zip
- Run full and lightweight Ghidra headless analysis
- Expose operator commands through Discord
- Track and terminate Ghidra processes with `psutil`
- Return generated reports through Discord

---

## Scope

The module operates within the following scope:

- Configured desktop software update URLs
- Installer, archive, and executable artifacts
- Discord operator commands
- Radar metadata and local target files

It is **not** responsible for:

- Mobile APK decompilation
- Smart contract analysis
- GitHub patch monitoring
- Hive event publication or delegated task consumption
- Central workflow management

---

## Architecture Role

**Domain:** Binary monitoring and reverse engineering  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `phantom_controller.py` | Runs the Discord bot, radar loop, download and extraction workflow, subprocess execution, status reporting, and process termination commands. |
| `tracker.py` | Runs full Ghidra headless auto-analysis and exports an extensive text report. |
| `tracker_lite.py` | Runs Ghidra with `-noanalysis` to extract exported symbols and raw strings with lower resource cost. |
| `radar_targets.json` | Defines monitored URLs, metadata fields, and optional `extract_target` values. |
| `radar_state.json` | Persists observed ETag, Last-Modified, and size values across restarts. |
| `deploy_contabo.sh` | Automates provisioning of Java, Python, 7-Zip, and Ghidra dependencies. |
| `.env` | Provides private Discord and runtime configuration outside the public repository. |

---

## Workflow

1. Poll configured software URLs and compare remote metadata with `radar_state.json`.
2. Download a changed package.
3. When configured, use 7-Zip to extract only the target executable and remove the original archive.
4. Store prepared artifacts in the local `targets/` area.
5. Accept `!list`, `!analyze`, `!lite`, `!status`, and `!kill` commands through Discord.
6. Run the selected Ghidra mode in a separate process and return the text report.

---

## Inputs

- `radar_targets.json` and `radar_state.json`
- Remote installer or archive responses
- Discord commands
- Prepared executable paths
- Runtime environment configuration

---

## Outputs

- Extracted executables in the controlled target directory
- Ghidra text reports
- Discord status and result messages
- Updated radar metadata state

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom Binary does not currently participate in the Hive Mind. It does not publish Redis events and does not consume delegated task channels.

---

## Communication

- Outbound HTTP requests to monitored software endpoints
- Discord bot commands and messages
- Local subprocess calls to 7-Zip, Java, and Ghidra
- No active Redis Pub/Sub communication

---

## Dependencies

- Python 3.10+
- `discord.py`
- `discord.ext.tasks`
- `psutil`
- `httpx`
- `subprocess`
- 7-Zip
- Java 17
- Ghidra and Jython
- Docker

---

## Configuration and State

- `radar_targets.json` contains monitoring configuration.
- `radar_state.json` persists remote metadata used to avoid duplicate downloads.
- The `targets/` directory contains prepared executables awaiting analysis.
- Operational Discord credentials are supplied through environment configuration.

---

## Runtime Characteristics

- Independent heavyweight Docker container
- CPU- and memory-intensive Ghidra workloads
- Long-running radar and Discord controller
- Separate analysis subprocesses
- Direct Discord reporting

---

## Failure Behavior

- A failed download or extraction affects only the current target.
- A Ghidra process can be inspected with `!status` and terminated with `!kill`.
- Binary service downtime does not affect the remaining modules or Hive chains.
- Incomplete analysis reports are not recovered automatically after container termination.

---

## Security Considerations

- Downloaded artifacts remain isolated inside the module environment.
- The controller should accept commands only from authorized Discord users and channels.
- Operational tokens and private target definitions remain outside the public repository.
- Analysis subprocesses are supervised to limit runaway resource consumption.

---

## Current Limitations

- Full Ghidra analysis can require significant CPU, memory, and time.
- The service does not participate in Redis correlation.
- The public repository excludes live target URLs, binaries, reports, credentials, and source code.
- Task recovery and distributed analysis are not implemented.

---

## Summary

Phantom Binary is an autonomous binary radar and reverse-engineering controller. It detects software updates, extracts selected executables, runs full or lightweight Ghidra analysis, supervises analysis processes, and reports results through Discord.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)

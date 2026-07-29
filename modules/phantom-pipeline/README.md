<p align="center">
  <img src="../../assets/icons/phantom-pipeline.png" width="160" alt="Phantom Pipeline">
</p>

<h1 align="center">Phantom Pipeline</h1>

<p align="center">
GitHub Actions workflow-run and CI/CD log secret scanner.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-autonomous-F79009)
![Hive](https://img.shields.io/badge/hive-not_participating-lightgrey)

</p>

---

## 📖 Overview

Phantom Pipeline is an autonomous GitHub Actions log scanner. It monitors workflow runs, downloads build logs, detects exposed CI/CD credentials, tracks processed runs locally, and reports findings directly to Discord.

---

## 🚀 Status

**Production**

Phantom Pipeline is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- GitHub Actions monitoring
- Workflow log retrieval
- CI/CD secret detection
- Processed-run state tracking
- Direct Discord alerting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-pipeline/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-pipeline.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.

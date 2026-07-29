<p align="center">
  <img src="../../assets/icons/phantom-core.png" width="160" alt="Phantom Core">
</p>

<h1 align="center">Phantom Core</h1>

<p align="center">
Asynchronous web crawler and exposed-secret scanner for public web assets.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-autonomous-F79009)
![Hive](https://img.shields.io/badge/hive-not_participating-lightgrey)

</p>

---

## 📖 Overview

Phantom Core is an autonomous asynchronous web crawler and secret scanner. It discovers relevant public web assets, identifies exposed credentials and configuration data, and reports findings directly to Discord without participating in the current Redis event flow.

---

## 🚀 Status

**Production**

Phantom Core is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- Asynchronous web crawling
- Linked asset discovery
- Exposed-secret detection
- Certificate-related target monitoring
- Direct Discord alerting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-core/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-core.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.

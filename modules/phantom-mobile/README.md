<p align="center">
  <img src="../../assets/icons/phantom-mobile.png" width="160" alt="Phantom Mobile">
</p>

<h1 align="center">Phantom Mobile</h1>

<p align="center">
Android release monitor and JADX-based APK decompilation service.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-autonomous-F79009)
![Hive](https://img.shields.io/badge/hive-not_participating-lightgrey)

</p>

---

## 📖 Overview

Phantom Mobile autonomously monitors Android releases, downloads and decompiles APKs with JADX, scans recovered source and manifests, reports relevant findings to Discord, and removes temporary artifacts after analysis.

---

## 🚀 Status

**Production**

Phantom Mobile is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- Android release monitoring
- APK download and decompilation
- JADX source inspection
- Manifest security checks
- Temporary artifact cleanup
- Direct Discord reporting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-mobile/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-mobile.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.

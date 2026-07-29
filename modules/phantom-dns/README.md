<p align="center">
  <img src="../../assets/icons/phantom-dns.png" width="160" alt="Phantom DNS">
</p>

<h1 align="center">Phantom DNS</h1>

<p align="center">
Asynchronous subdomain enumeration and abandoned-cloud-service signature detector.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-autonomous-F79009)
![Hive](https://img.shields.io/badge/hive-not_participating-lightgrey)

</p>

---

## 📖 Overview

Phantom DNS autonomously generates and probes candidate subdomains, identifies response signatures associated with abandoned cloud services, and reports potential takeover conditions to Discord for responsible validation.

---

## 🚀 Status

**Production**

Phantom DNS is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- Candidate subdomain generation
- Asynchronous DNS and HTTP probing
- Abandoned-service signature matching
- Request throttling
- Direct Discord reporting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-dns/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-dns.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.

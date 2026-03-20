# DARKNODE — Roadmap

> Last updated: March 2026 · Version 0.4.x

---

## Vision

Build the world's first fully sovereign infrastructure stack — where AI inference, mesh radio communication, and economic activity all operate without any internet dependency, cloud service, or centralized authority.

---

## Status Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Shipped |
| 🔄 | In Progress |
| 📋 | Planned |
| 💡 | Research / Exploratory |

---

## Phase 1 — Foundation `v0.1 – v0.2` ✅

> **Goal:** Prove the concept. Local AI + basic mesh bridge working together.

- ✅ Local llama.cpp inference via web UI
- ✅ `@bot` and `!ask` mesh query routing over Meshtastic
- ✅ Auto-download GGUF models on install
- ✅ Basic message log (inbound/outbound)
- ✅ Node.js + Python installer (Linux/Mac)
- ✅ First public release (v0.2.0)

---

## Phase 2 — Core Stack `v0.3 – v0.4` ✅

> **Goal:** Add payments and radio telemetry. Make it production-usable.

- ✅ Built-in Bitcoin HD wallet (BIP44, local seed)
- ✅ LoRa bridge via Meshtastic USB serial
- ✅ Node telemetry: battery, SNR, GPS, environment sensors
- ✅ Cashu ecash wallet integration
- ✅ Model manager: download/switch GGUF models from web UI
- ✅ Real-time node map dashboard
- ✅ Windows installer support
- ✅ Fixed serial port auto-detection

---

## Phase 3 — Resilience & UX `v0.5 – v0.6` 🔄

> **Goal:** Make DARKNODE reliable enough for real disaster scenarios and non-technical users.

- 🔄 One-click installer (no terminal required)
- 🔄 Mobile-responsive web dashboard
- 🔄 Automatic mesh relay routing (multi-hop optimization)
- 📋 Offline model repository (pre-bundled models, no download needed)
- 📋 Node health monitoring + alerts
- 📋 Encrypted mesh messages (end-to-end via Meshtastic channels)
- 📋 Battery-aware inference throttling (for Raspberry Pi on solar)
- 📋 Docker support for easy deployment
- 📋 ARM64 binary support (Raspberry Pi 4/5, Orange Pi)

---

## Phase 4 — Mesh Economy `v0.7 – v0.8` 📋

> **Goal:** Enable real economic activity over the mesh — AI services, data exchange, micro-payments.

- 📋 Pay-per-query AI over mesh (Cashu tokens as payment)
- 📋 Node reputation system (track uptime, response quality)
- 📋 Mesh marketplace: sell/buy data, compute, bandwidth
- 📋 Lightning Network integration (when connectivity available)
- 📋 Multi-node wallet sync over mesh
- 📋 Cashu mint support (run your own mint on the mesh)
- 📋 Atomic swaps between mesh nodes

---

## Phase 5 — Decentralized Intelligence `v0.9 – v1.0` 📋

> **Goal:** Distributed AI across the mesh — no single node needs to hold the full model.

- 📋 Federated inference: split model across multiple nodes
- 📋 Mesh-native RAG (Retrieval-Augmented Generation) from local documents
- 📋 Shared knowledge base sync over radio
- 📋 Agent mode: AI nodes that can autonomously execute tasks
- 📋 Multi-modal support (image + text, when hardware allows)
- 📋 Fine-tuning pipeline for domain-specific models (offline)

---

## Phase 6 — Ecosystem `v1.0+` 💡

> **Goal:** DARKNODE as a platform — open for developers, communities, and institutions.

- 💡 Plugin system for custom mesh applications
- 💡 DARKNODE SDK for third-party developers
- 💡 Community mesh map (opt-in node registry)
- 💡 Hardware kit partnerships (pre-flashed devices)
- 💡 Emergency services integration (SAR, disaster relief)
- 💡 Satellite backhaul support (Iridium, Starlink fallback)
- 💡 Cross-mesh protocol bridge (LoRa ↔ WiFi mesh ↔ HF radio)

---

## Current Sprint (Q1 2026)

| Task | Status | Target |
|------|--------|--------|
| One-click installer | 🔄 In Progress | v0.5.0 |
| Mobile dashboard | 🔄 In Progress | v0.5.0 |
| ARM64 binaries | 🔄 In Progress | v0.5.0 |
| Docker image | 📋 Planned | v0.5.1 |
| Encrypted messages | 📋 Planned | v0.6.0 |
| Pay-per-query | 📋 Planned | v0.7.0 |

---

## How to Contribute

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Priority areas where help is most needed:
- **Embedded systems** — Raspberry Pi, ESP32 optimization
- **Radio/RF** — Meshtastic protocol, LoRa antenna design
- **Cryptography** — Cashu protocol, Lightning integration
- **ML/AI** — GGUF model optimization, quantization
- **Frontend** — Dashboard UI, mobile responsiveness

---

## Community

- GitHub: [kingzero-creator/darknode](https://github.com/kingzero-creator/darknode)
- Twitter/X: [@zeroking0905](https://x.com/zeroking0905)
- Website: [darknode.fun](https://darknode.fun)

---

*This roadmap is a living document. Priorities may shift based on community feedback and real-world usage.*

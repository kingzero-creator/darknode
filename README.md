# DARK/NODE

**Sovereign Infrastructure Stack** — Local AI · LoRa Mesh Radio · Off-Grid Payments

[![License: MIT](https://img.shields.io/badge/License-MIT-00ff9d.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-00ff9d)](https://nodejs.org)
[![Python](https://img.shields.io/badge/Python-3.11%2B-00b8ff)](https://python.org)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-566878)]()
[![No Cloud](https://img.shields.io/badge/Cloud-0%25-ff4560)]()

---

> **Run your own node.** Local AI inference, mesh radio communication, and off-grid payments — unified in a single stack that runs on your hardware. No servers. No accounts. No internet required.

---

## What is DARKNODE?

DARKNODE combines three systems that nobody has combined before:

| System | Technology | What it does |
|--------|-----------|--------------|
| 🤖 **Local AI** | llama.cpp + GGUF | Runs a quantized LLM fully on-device. No API keys. No cloud calls. |
| 📡 **Mesh Radio** | Meshtastic + LoRa | Bridges to LoRa radio over USB. Range: 5–15 km per hop. No towers. |
| ₿ **Off-Grid Payments** | Bitcoin HD + Cashu | Send ecash tokens as plain text over radio. Works without internet. |

---

## Quick Install

**Requirements:** Node.js 18+ and Python 3.11+

```bash
# Clone the repository
git clone https://github.com/kingzero-creator/darknode && cd darknode

# Run the full installer
npm run install:full
```

The installer automatically downloads:
- `llama.cpp` runtime binary for your platform
- A starter GGUF model (`Qwen2.5-0.5B-Instruct-Q4_K_M`)
- Python dependencies for Meshtastic

Then open **http://localhost:3333** in your browser.

---

## Features

### 🤖 Local AI
- Runs `llama.cpp` inference entirely on-device
- No API keys, no cloud calls, no data leaves your machine
- Supports any GGUF model from 0.5B to 7B+ parameters
- Model manager: download/switch models from the web UI
- Mesh-triggered queries: any node can send `@bot` or `!ask` over radio

### 📡 LoRa Mesh Radio
- Bridges to any Meshtastic-compatible device via USB serial
- Auto-detects port, auto-installs Python bridge
- Tracks all nodes: battery, SNR, GPS position, environment sensors
- Full inbound/outbound message log
- Range: 5–15 km per hop, extendable through relay nodes

### ₿ Off-Grid Payments
- Built-in Bitcoin HD wallet (BIP44, stored locally)
- Cashu ecash wallet — tokens are plain text strings
- Send tokens over radio as messages
- Redeem to Lightning when connectivity returns
- No internet required at transfer time

### 🔒 Privacy & Security
- 100% local data storage in `./data/`
- Zero telemetry, zero analytics, zero accounts
- Air-gap ready — works completely disconnected
- Fully auditable open-source code

---

## Compatible Hardware

| Device | Price | Notes |
|--------|-------|-------|
| Heltec LoRa 32 | ~$20–30 | Best starter. Compact, built-in OLED. |
| LilyGO T-Beam | ~$35–45 | Built-in GPS + battery management. |
| RAK WisBlock | ~$40–60 | Modular, great for permanent installs. |
| SenseCAP T1000 | ~$50–60 | Card-sized, ultra portable. |

> No radio? DARKNODE still runs — local AI and wallet work fine without a device connected.

---

## How It Works

```
[Any LoRa Device]
      │
      │ "@bot what is the weather?"
      │
      ▼
[Relay Node] ──hop──► [Relay Node] ──hop──► [DARKNODE AI Node]
                                                    │
                                              llama.cpp inference
                                              (fully on-device)
                                                    │
[Any LoRa Device] ◄──hop──◄──hop──◄─── response broadcast
```

1. **Connect** — Plug in a Meshtastic LoRa device. App auto-detects it.
2. **Ask** — Any device in range sends `@bot your question` over radio.
3. **Inference** — The AI node processes it locally, no internet needed.
4. **Response** — Answer routes back through the mesh to the requester.
5. **Pay** — Cashu tokens travel the same way as messages.

---

## Project Structure

```
darknode/
├── src/
│   ├── ai/          # llama.cpp bridge, model manager
│   ├── radio/       # Meshtastic serial bridge
│   ├── wallet/      # Bitcoin HD + Cashu ecash
│   └── web/         # Web dashboard (port 3333)
├── data/            # Local storage (messages, nodes, wallet, proofs)
├── models/          # GGUF model files
├── scripts/         # Install scripts (Linux/Mac/Windows)
└── README.md
```

---

## Use Modes

### Mode 1 — Local AI Only
No radio needed. Full web UI, local AI chat, wallet functions.
```
Node.js 18+ + Python 3.11+ + GGUF model
```

### Mode 2 — Full Mesh Node
Everything above plus a Meshtastic device. Becomes an AI gateway and relay for the entire mesh.
```
Node.js 18+ + Python 3.11+ + GGUF model + Meshtastic LoRa device
```

---

## Changelog

### v0.4.0 — Mar 2025 `major`
- New web dashboard with real-time node map
- Model manager: download/switch GGUF models from UI
- Cashu ecash wallet integration
- Fixed serial port auto-detection on Windows

### v0.3.2 — Jan 2025 `patch`
- Reduced memory usage on low-RAM devices
- Fixed mesh reconnect loop on signal loss
- Added support for Qwen2.5 model family

### v0.3.0 — Nov 2024 `minor`
- Built-in HD Bitcoin wallet
- LoRa bridge via Meshtastic USB serial
- ⚠️ Config format changed — re-run installer

### v0.2.0 — Sep 2024 `minor`
- First public release
- Local llama.cpp inference via web UI
- `@bot` and `!ask` mesh query routing
- Auto-download GGUF models on install

---

## Support the Project

This project is free and open source. If it's useful — in a blackout, off the grid, or just for the idea — consider sending something to keep development going.

**Bitcoin**
```
bc1p3p87l267hte2dgg60jjt7w9xk8vfcjenr534yya0hedhet4l4fvq2x2svp
```

**Monero**
```
42SWAqWMKAiAHokhaGjBRUdcdQqvMk3rmBAnnUpPNGhridJKFAknqCQeYnixXhbPtyEHzxBmUkMxAjQtLSQbiVq57Pbvge5
```

**Ethereum**
```
0xaA01e4F453d5ae9903EebeABA803f3388D20d024
```

---

## Links

- 🌐 Website: [darknode.html](https://kingzero-creator.github.io/darknode)
- 🐦 Twitter/X: [@zeroking0905](https://x.com/zeroking0905)
- 🔗 Featured on: [Orynth.dev](https://www.orynth.dev/)
- 📡 Meshtastic: [meshtastic.org](https://meshtastic.org)
- ⚡ Cashu: [cashu.space](https://cashu.space)

---

## License

MIT License — free to use, modify, and distribute.

```
Free & Open Source · No Cloud · No Accounts · No Telemetry
```

# DARKNODE Whitepaper

**Sovereign Infrastructure for a Disconnected World**

Version 1.0 · March 2026
Author: kingzero-creator
Website: darknode.fun

---

## Abstract

Modern digital infrastructure has a fundamental fragility: it depends entirely on the internet. AI assistants go offline when servers go down. Communication fails when cell towers fail. Financial systems halt when payment processors are unreachable.

DARKNODE is an open-source software stack that eliminates this dependency. It combines three technologies — local AI inference, LoRa mesh radio, and off-grid cryptographic payments — into a single self-contained system that runs on commodity hardware and operates indefinitely without any internet connection, cloud service, or centralized authority.

This paper describes the technical architecture, design principles, and real-world applications of DARKNODE.

---

## 1. The Problem

### 1.1 Infrastructure Fragility

Over 4.8 billion people use internet-dependent services daily. This creates a single point of failure: when connectivity is lost — due to natural disaster, power outage, censorship, or infrastructure attack — these services become completely unavailable.

This is not a hypothetical risk. In 2023 alone:
- Over 180 major internet outages affected millions of users globally
- Natural disasters caused communication blackouts lasting days to weeks
- Several governments imposed internet shutdowns affecting millions

### 1.2 AI Centralization

Current AI systems are entirely cloud-dependent. Every query sent to ChatGPT, Claude, or Gemini travels to a data center, is processed on corporate hardware, and returns over the public internet. This creates three problems:

1. **Availability** — No internet means no AI
2. **Privacy** — Every query is logged by a third party
3. **Cost** — Ongoing subscription fees forever

### 1.3 Payment System Dependency

Traditional payment systems (credit cards, bank transfers, mobile payments) require internet connectivity and third-party processors. In a grid-down scenario, economic activity stops entirely.

Bitcoin addresses this partly, but Lightning Network still requires internet for payment routing. A truly off-grid payment system needs to work over any communication channel.

---

## 2. The Solution

DARKNODE addresses all three problems with a unified stack:

| Problem | Solution | Technology |
|---------|----------|-----------|
| AI unavailability | On-device inference | llama.cpp + GGUF models |
| Communication blackout | Mesh radio | Meshtastic + LoRa |
| Payment failure | Off-grid ecash | Bitcoin HD + Cashu |

The key insight is that these three systems share a common requirement — they must all work without internet — and they are more powerful when integrated than when used separately.

---

## 3. Technical Architecture

### 3.1 System Overview

```
┌─────────────────────────────────────────────────┐
│                  DARKNODE NODE                   │
│                                                 │
│  ┌──────────────┐  ┌──────────────┐             │
│  │  Web Dashboard│  │  REST API    │             │
│  │  (Port 3333) │  │  (Port 3334) │             │
│  └──────┬───────┘  └──────┬───────┘             │
│         │                 │                     │
│  ┌──────▼─────────────────▼───────┐             │
│  │         Node.js Core           │             │
│  │  ┌─────────┐  ┌─────────────┐  │             │
│  │  │ AI Layer│  │ Radio Layer │  │             │
│  │  │llama.cpp│  │ Meshtastic  │  │             │
│  │  │ bridge  │  │   bridge    │  │             │
│  │  └─────────┘  └─────────────┘  │             │
│  │  ┌─────────────────────────┐   │             │
│  │  │     Payment Layer       │   │             │
│  │  │  Bitcoin HD + Cashu     │   │             │
│  │  └─────────────────────────┘   │             │
│  └────────────────────────────────┘             │
│                                                 │
│  ./data/  (all local storage)                   │
└─────────────────────────────────────────────────┘
         │
         │ USB Serial
         ▼
┌─────────────────┐
│  LoRa Device    │
│  (Meshtastic)   │
└─────────────────┘
         │
         │ Radio 868/915 MHz
         ▼
┌─────────────────┐     ┌─────────────────┐
│   Relay Node    │────▶│   Remote Node   │
│   (any device)  │     │  (any device)   │
└─────────────────┘     └─────────────────┘
```

### 3.2 AI Layer

DARKNODE uses **llama.cpp** for local inference. llama.cpp is a C++ implementation of LLaMA-architecture models optimized for CPU inference on consumer hardware.

**Model format:** GGUF (GPT-Generated Unified Format)
- Quantized to 4-bit or 8-bit for size/performance balance
- Q4_K_M quantization provides best quality-to-size ratio
- Models range from 0.5B parameters (works on Pi 4) to 7B+ (requires 8GB+ RAM)

**Supported models:**
- Qwen2.5 series (0.5B, 1.5B, 3B, 7B)
- Llama 3.2 series
- Mistral 7B
- Any GGUF-compatible model

**Query routing:**
When a Meshtastic device sends `@bot <query>` or `!ask <query>`, the radio bridge receives it, passes it to the AI layer, and broadcasts the response back over the mesh.

### 3.3 Radio Layer

DARKNODE bridges to **Meshtastic** devices via USB serial connection.

**Meshtastic** is an open-source mesh networking protocol built on LoRa (Long Range) radio. Key properties:
- **Frequency:** 868 MHz (EU) / 915 MHz (US/Asia)
- **Range:** 5–15 km per hop in open terrain
- **Topology:** Flooding mesh — every node relays packets
- **Protocol:** Custom binary protocol over LoRa physical layer

**Bridge implementation:**
The Node.js core runs a Python subprocess (`meshtastic-bridge.py`) that interfaces with the Meshtastic library via USB serial. The bridge:
1. Receives all mesh messages via event listener
2. Filters for `@bot` and `!ask` prefixes
3. Passes queries to the AI layer
4. Broadcasts AI responses back over the mesh
5. Tracks node telemetry (battery, GPS, sensors)

### 3.4 Payment Layer

DARKNODE includes two complementary payment systems:

**Bitcoin HD Wallet**
- BIP44 hierarchical deterministic wallet
- Seed phrase generated locally, never transmitted
- Stored encrypted in `./data/wallet.json`
- Derives standard Bitcoin addresses

**Cashu Ecash**
Cashu is a Chaumian ecash protocol that enables offline bearer payments.

Key properties:
- **Bearer instrument** — whoever holds the token can spend it
- **Plain text** — tokens are base64-encoded strings
- **Offline transfer** — tokens transfer without internet
- **Lightning redemption** — tokens redeem for sats when online

**Mesh payment flow:**
```
Sender                    Mesh                   Receiver
  │                        │                        │
  │  Issue Cashu token     │                        │
  │  (offline)             │                        │
  │                        │                        │
  │  Send token as         │                        │
  │  radio message ───────▶│──────────────────────▶│
  │                        │                        │
  │                        │        Receive token   │
  │                        │        Store locally   │
  │                        │                        │
  │                        │  (later, when online)  │
  │                        │                        │
  │                        │        Redeem for      │
  │                        │        Lightning sats  │
```

---

## 4. Data Storage

All data is stored locally in the `./data/` directory. No external databases, no cloud sync, no telemetry.

```
./data/
├── config.json       # Node configuration
├── messages.db       # Message log (SQLite)
├── nodes.json        # Known mesh nodes + telemetry
├── wallet.json       # Encrypted Bitcoin HD wallet
├── cashu.json        # Cashu proofs (unspent tokens)
└── models/           # GGUF model files
```

**Privacy guarantees:**
- No outbound connections except on user-initiated actions
- No analytics, no error reporting, no usage metrics
- Source code fully auditable — no closed components

---

## 5. Use Cases

### 5.1 Emergency Preparedness

A network of DARKNODE nodes provides AI-assisted communication in disaster scenarios:
- Query first aid procedures without internet
- Coordinate rescue operations over mesh radio
- Exchange value (Cashu tokens) when payment systems are down
- Range: a 5-node network can cover ~50 km radius

### 5.2 Remote Areas

Communities in areas with poor or no internet connectivity:
- AI assistant for education, health information, agriculture
- Local mesh communication between villages
- Economic activity independent of banking infrastructure

### 5.3 Privacy-Conscious Users

Individuals who want AI assistance without cloud surveillance:
- All queries processed locally — never logged by third parties
- No account required, no identity linked to usage
- Works in a Faraday cage if necessary

### 5.4 Amateur Radio Operators

Ham radio operators extending Meshtastic capabilities:
- AI-assisted repeater for mesh networks
- Query propagation conditions, grid references, emergency procedures
- Integration with existing amateur radio infrastructure

### 5.5 Journalists and Activists

Operating in restricted environments:
- Communication without internet-dependent apps
- AI assistance without cloud logging
- Economic activity without financial surveillance

---

## 6. Security Model

### 6.1 Threat Model

DARKNODE is designed to operate securely against:
- **Network surveillance** — no internet traffic to monitor
- **Service denial** — no external dependencies to attack
- **Data exfiltration** — no outbound data transmission
- **Account seizure** — no accounts exist

### 6.2 Limitations

DARKNODE does **not** currently protect against:
- **Physical device seizure** — data at rest is not encrypted by default
- **Radio interception** — mesh messages are not end-to-end encrypted by default (Meshtastic channel encryption available)
- **Local network attacks** — web dashboard accessible on local network

### 6.3 Planned Security Improvements

- Full disk encryption option for `./data/`
- Default Meshtastic channel encryption
- Web dashboard authentication
- Tor hidden service option for remote access

---

## 7. Economic Model

DARKNODE is free and open source software. Development is sustained through:

1. **Voluntary donations** — Bitcoin, Monero, Ethereum
2. **Sponsorships** — organizations using DARKNODE in production
3. **Grants** — open source and privacy-focused foundations
4. **Future:** Pay-per-query mesh AI economy (see Roadmap Phase 4)

The long-term vision is a self-sustaining mesh economy where nodes earn Cashu tokens by serving AI queries and relaying messages, creating economic incentives for network growth without any central coordination.

---

## 8. Comparison with Alternatives

| Feature | DARKNODE | Ollama | LM Studio | CloudAI |
|---------|----------|--------|-----------|---------|
| Works offline | ✅ Full | ✅ PC only | ✅ PC only | ❌ Never |
| Mesh radio | ✅ Built-in | ❌ | ❌ | ❌ |
| Off-grid payments | ✅ Bitcoin+Cashu | ❌ | ❌ | ❌ |
| No account | ✅ | ✅ | ✅ | ❌ |
| No telemetry | ✅ Verified | ~ Optional | ~ Optional | ❌ Always |
| Disaster-ready | ✅ Designed for it | ❌ | ❌ | ❌ |
| Hardware cost | $25–60 | PC required | PC required | $20+/mo |
| Open source | ✅ 100% | ✅ | ~ Partial | ❌ |

---

## 9. Conclusion

The internet is a remarkable achievement, but it is not a guarantee. The systems we depend on for information, communication, and economic activity can and do fail — through disaster, censorship, infrastructure attack, or simply geography.

DARKNODE is a practical response to this reality. By combining local AI inference, mesh radio communication, and off-grid cryptographic payments into a single open-source stack, it enables individuals and communities to maintain capability when the infrastructure they usually rely on is unavailable.

The technology is ready. The hardware is cheap. The software is free.

**Run your own node.**

---

## References

- llama.cpp: https://github.com/ggerganov/llama.cpp
- Meshtastic: https://meshtastic.org
- Cashu Protocol: https://cashu.space
- Bitcoin BIP44: https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki
- GGUF Format: https://github.com/ggerganov/ggml/blob/master/docs/gguf.md

---

## License

This whitepaper is released under Creative Commons CC BY 4.0.
DARKNODE software is released under the MIT License.

---

*DARKNODE — Free & Open Source · No Cloud · No Accounts · No Telemetry*
*https://darknode.fun · https://github.com/kingzero-creator/darknode*

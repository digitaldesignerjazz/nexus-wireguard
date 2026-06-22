# nexus-wireguard

**High-performance WireGuard VPN integration for the Nexus decentralized mesh networking ecosystem (xMesh • NovaNet • QNET).**

Part of the NovaNet prototyping initiative by Esslinger & Co. — advancing resilient, privacy-first, AI-orchestrated infrastructure.

[![GitHub stars](https://img.shields.io/github/stars/digitaldesignerjazz/nexus-wireguard?style=social)](https://github.com/digitaldesignerjazz/nexus-wireguard/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🌐 Overview

`nexus-wireguard` delivers fast, secure, kernel-efficient VPN tunnels to the Nexus stack:

- **Mesh Layer**: xMesh / NovaNet / QNET overlays built on Yggdrasil for global addressing and resilient routing.
- **Privacy Stack**: Tor / I2P for anonymity layers; WireGuard for confidential, authenticated point-to-point and group links.
- **Blockchain Coordination**: XCoin / QCoin / QNET runes for incentives, peer discovery, reputation, and decentralized key management.
- **AI Agent Swarms**: Self-improving agents (Grok Launcher, Lyra emotional, Xen technical) that monitor, optimize, and heal the mesh fabric autonomously.
- **Prototypes & Hardware**: Integration hooks for Soilnova (sensing), Vista Nova (visualization), York Autotype, Lumia, Tenda Nova routers, and Docker-orchestrated nodes.

WireGuard's modern cryptography (Curve25519 key exchange, ChaCha20-Poly1305 AEAD, BLAKE2s hashing) and tiny codebase provide excellent performance (multi-gigabit on modest hardware) and a minimal attack surface — ideal for dense meshes, edge devices, and mobile/roaming agents.

This component enables **trusted private overlays** within (or alongside) the public Yggdrasil mesh, secure service exposure, and hybrid topologies that balance performance, privacy, and decentralization.

> **📍 Full Architecture Documentation**: See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the complete layered model, Mermaid diagrams, layer-by-layer analysis, data flows, cross-cutting concerns, and implementation roadmap.

## ⚡ Why WireGuard for Nexus?

### Performance & Scalability
- **Speed**: Often saturates 1 Gbps+ links with <5% CPU on x86/ARM. Far lighter than OpenVPN or IPsec.
- **Efficiency**: ~80 bytes overhead. Tunable MTU (recommend 1280–1420 depending on nesting with Yggdrasil).
- **Cross-Platform**: Linux kernel module (preferred), userspace wireguard-go for containers/Docker, Windows/macOS/iOS/Android clients.
- **Mesh Fit**: Simple CLI (`wg`, `wg-quick`) and library-friendly for dynamic peer management. Supports roaming via endpoint updates.

### Security & Cryptography
- **Strong Primitives**: Perfect forward secrecy, authenticated encryption, replay protection, strong resistance to known attacks.
- **Simplicity**: ~4,000 lines of code → easier to audit and verify than legacy VPNs.
- **Kernel Integration**: Runs in kernel space on Linux for speed and reduced context switches.

**Important Nuance**: WireGuard provides *confidentiality and integrity* between authenticated peers but **does not provide anonymity or metadata protection by default**. IP addresses, timing, and traffic patterns can leak. For stronger privacy in Nexus:
- Layer with Yggdrasil (onion-like routing obfuscation).
- Add Tor/I2P exits or full onion routing for public-facing services.
- Use short-lived keys + frequent rotation orchestrated by AI agents or QNET smart contracts.

### Edge Cases & Considerations
- **NAT Traversal**: Excellent for most NAT types via keepalives; struggles with symmetric NAT or strict CGNAT — mitigate with public relays, IPv6 preference (Yggdrasil shines here), or ICE-like signaling via blockchain.
- **Key Bootstrap & Distribution**: Decentralized chicken-and-egg problem. Solutions explored: out-of-band QR codes, blockchain-published pubkeys (QNET), gossip protocols in mesh, or trusted introducer nodes in early swarms.
- **MTU & Fragmentation**: Nested tunnels (Yggdrasil over WireGuard or vice-versa) require careful MTU tuning and Path MTU Discovery (PMTUD). Test with `ping -M do -s 1472`.
- **Large Meshes**: WireGuard peers are O(n) in config but highly efficient; for 500+ nodes consider hierarchical clustering or dynamic on-demand tunnels managed by AI swarm.
- **Mobility & Roaming**: Built-in support via endpoint updates; AI agents can detect IP changes and push `wg set` updates in real time.
- **Multicast / Broadcast**: Native WireGuard is unicast; use userspace extensions, multicast routing daemons (e.g., over Yggdrasil), or app-level replication for group comms.
- **Post-Quantum Future**: Current primitives are classical. Roadmap includes hybrid post-quantum key exchange experiments when standardized and performant.

## 🏗️ Architecture at a Glance

The Nexus communication fabric follows a **composable layered model** (L0–L5). WireGuard (this repo) primarily owns **L1** while providing clean hooks into all other layers.

See the full interactive Mermaid diagram and detailed breakdown in **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)**.

**Simplified view**:

```
Physical / Docker Network / Tenda Nova WiFi
          ↓
WireGuard Secure Tunnels (wg0, dynamic peers)
          ↓ (or parallel)
Yggdrasil Overlay (global addressing, resilient routing)
          ↓
QNET Blockchain (discovery, incentives, reputation, key registry)
          ↓
AI Agent Swarm (optimization, healing, predictive routing)
          ↓
Applications: Grok Launcher dashboards, prototypes (Soilnova telemetry, Vista Nova viz), blockchain nodes
```

## 🚀 Roadmap

- **v0.1** (Now): Repository bootstrap, rich documentation, static example configs, Docker skeleton, initial integration notes.
- **v0.2**: Dynamic peer controller (Python), basic QNET discovery stub, automated testing harness.
- **v0.3**: Full blockchain hooks (pubkey announcement, simple reputation), AI agent heartbeat integration.
- **v0.4**: Advanced orchestration — swarm-driven tunnel optimization, hybrid Yggdrasil+WireGuard routing policies, Grok Launcher dashboard widgets.
- **v0.5+**: Production hardening, post-quantum experiments, hardware offload (Tenda Nova, ARM), large-scale simulation & global testnet, formal verification hooks.

Long-term vision: The default secure communication primitive enabling a global, economically self-sustaining, AI-coordinated Nexus mesh fabric.

## 🚀 Getting Started

### Prerequisites
- Linux with WireGuard kernel module (`modprobe wireguard`) or `wireguard-go`.
- Docker & Docker Compose (v2+ recommended).
- `wg`, `wg-quick` tools.
- Git.
- (Optional but recommended) Running Yggdrasil node and/or QNET testnet participant.

### Quick Local Example (Static Peers)

```bash
git clone https://github.com/digitaldesignerjazz/nexus-wireguard.git
cd nexus-wireguard

# Review example configs (to be expanded in v0.2)
# docker compose -f docker/docker-compose.yml up -d

# Manual WireGuard interface example (edit keys!)
# sudo wg-quick up ./configs/wg0.conf
```

See `docs/` (future) and inline comments for full configuration, key generation (`wg genkey`), and peer exchange.

**Security Warning**: Never commit private keys or sensitive endpoint data. Use `.gitignore` and environment variables or secrets managers in production.

## 🔒 Security & Privacy Considerations

- **Threat Model**: Assumes authenticated peers (pre-shared pubkeys). Protects against eavesdropping, tampering, and replay on the wire.
- **Limitations**: No built-in anonymity. Combine with Yggdrasil's routing and/or Tor/I2P for metadata protection.
- **Key Management**: Decentralized key distribution is non-trivial. Early versions rely on manual or out-of-band exchange; later versions will leverage QNET for secure publication.
- **Auditing**: Small TCB is an advantage. Future formal methods and external audits encouraged.
- **Regulatory**: Crypto export and usage laws vary (user base in EU/Germany). This project focuses on defensive, privacy-respecting use cases.

**Best Practices**:
- Rotate keys periodically (AI agents can automate).
- Use unique keypairs per relationship or tunnel group.
- Monitor for anomalous traffic patterns via integrated logging + AI anomaly detection.
- Prefer IPv6 where possible (better with Yggdrasil).
- Implement least-privilege peering policies.

## 🤝 Contributing

Contributions, issues, and pull requests are welcome! This is early-stage experimental infrastructure — all ideas for integration with the broader Nexus stack (mesh + blockchain + AI + prototypes) are valued.

1. Fork the repo
2. Create a feature branch (`git checkout -b feat/amazing-integration`)
3. Commit your changes with clear messages
4. Open a Pull Request

Please follow conventional commits and add tests/docs where applicable. For large changes, open an issue first to discuss alignment with NovaNet vision.

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

**Built with ❤️ for the Nexus vision** — resilient decentralized infrastructure, self-improving intelligence, and privacy as a foundation.

*Hannover • NovaNet Prototyping* 

For related components see the broader Nexus ecosystem orchestration.
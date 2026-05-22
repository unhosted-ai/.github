# unhosted

> Private inference on hardware you already own. Pool the devices on your network into one OpenAI-compatible endpoint, with end-to-end encrypted traffic between paired peers and no central server.

[Documentation](https://unhosted-ai.github.io/unhosted-core/) · [Installation](https://github.com/unhosted-ai/unhosted-core#install) · [Report an issue](https://github.com/unhosted-ai/unhosted-core/issues/new?labels=bug)

---

## Projects

| Repository | Purpose | License |
|---|---|---|
| [`unhosted-core`](https://github.com/unhosted-ai/unhosted-core) | Daemon and CLI. One binary per device exposes an HTTP API on port `7777`; inference runs inside `llama.cpp`, `Ollama`, or `LM Studio`. The orchestration layer, not the model. | AGPL-3.0-or-later |
| [`unhosted-payments`](https://github.com/unhosted-ai/unhosted-payments) | Settlement primitives and per-rail adapters for public-mode swarm. Rail-agnostic core; Lightning, USDC, and Stripe adapters ship as sibling crates. | AGPL-3.0-or-later |
| [`unhosted-plugins`](https://github.com/unhosted-ai/unhosted-plugins) | Model Context Protocol (MCP) server and plugin scaffolding for Claude Desktop, Cursor, Zed, and other MCP hosts. | AGPL-3.0-or-later |
| [`homebrew-unhosted`](https://github.com/unhosted-ai/homebrew-unhosted) | Homebrew tap. Provides `llama-cpp-rpc` and related VRAM-pool runtime dependencies. | — |

A commercial enterprise tier is maintained in a separate proprietary repository. See [Licensing](#licensing).

---

## Target use cases

The canonical workloads unhosted is built for — codebase exploration, personal research, regulatory monitoring, internal knowledge bases, personal finance — plus a commercial-tier-gated tier (healthcare, legal) are documented in [`USE_CASES.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/USE_CASES.md). The agent-runtime tool roadmap is derived from that list.

## Trust radius

unhosted exposes three concentric operating modes, each strictly containing the one before it:

| Mode | Scope | Cost |
|---|---|---|
| **Local** | Devices on your network. No internet required. | Free |
| **Trusted** | Peers paired explicitly out-of-band. End-to-end encrypted. | Free |
| **Public** | Strangers' idle GPUs, paid per token in USDC or Lightning. Opt-in. | Per-token, operator-priced |

Each mode is opt-in. Traffic falls outward only when explicitly requested by the caller.

---

## Installation

```sh
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/unhosted-ai/unhosted-core/main/scripts/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/unhosted-ai/unhosted-core/main/scripts/install.ps1 | iex
```

Start the daemon with `unhosted serve`. The web UI is served at `http://127.0.0.1:7777`. Existing installations can update in place with `unhosted upgrade`.

---

## Release status

Status of major capabilities as of the latest tagged release:

| Capability | Status | First shipped |
|---|---|---|
| Single-machine inference (llama.cpp / Ollama / LM Studio) | Shipped | v0.0.1 |
| LAN cluster — request routing across paired peers | Shipped | v0.0.2 |
| mDNS peer discovery and explicit pairing | Shipped | v0.0.3 |
| QUIC peer transport with Ed25519-pinned TLS | Shipped | v0.0.4 |
| Auto-detection of installed model runtimes | Shipped | v0.0.4 |
| Web UI and desktop application (Tauri 2) | Shipped | v0.0.14 |
| Minisign-verified auto-updater | Shipped | v0.0.15 |
| Public URL via Cloudflare tunnel | Shipped | v0.0.35 |
| VRAM-pooling — layer-split inference across paired peers | Shipped | v0.0.35 |
| Public-mode policy (sanctions, KYC tiers, rail filtering) | Shipped | v0.0.46 |
| Lightning rail adapter (ADR-0011 Phase B) | Shipped | v0.0.61 |
| `unhosted upgrade` subcommand and startup update check | Shipped | v0.0.64 |
| Audit-log SSE feed (`/v1/audit/stream`, `/v1/audit/recent`) | Shipped | v0.0.65 |
| Prometheus `/metrics` endpoint | Shipped | v0.0.66 |
| Data-loss-prevention integration hook | Shipped | v0.0.67 |
| Trusted-peer pairing (WireGuard-style) | Designed | v0.1.0 |
| Public swarm with USDC escrow | Designed | v0.3.0+ |
| Verifiable inference | Research | — |

---

## Security and compliance posture

The open-source platform is published under AGPL-3.0-or-later. Every claim in the compliance documentation is verifiable against the published source code.

The daemon ships with the following defaults:

- **Sanctions enforcement** — comprehensively-sanctioned jurisdictions are auto-merged into every saved policy at the daemon level. The operator cannot persist a policy that omits them.
- **No telemetry** — the daemon emits no analytics or usage data. The only optional outbound call is a GitHub-releases poll for update detection, disabled via `UNHOSTED_NO_UPDATE_CHECK=1`.
- **Signed releases** — every release artifact is minisign-signed. The auto-updater refuses unsigned bundles.
- **No central server** — the platform has no upstream SaaS dependency at runtime. The optional Cloudflare tunnel and public-mode payment rails are explicit operator opt-ins.

Compliance documentation maps the platform's controls to SOC 2 Common Criteria, ISO/IEC 27001:2022 Annex A, NIST AI Risk Management Framework, EU AI Act, and HIPAA Technical Safeguards. See [`COMPLIANCE.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/COMPLIANCE.md), [`SECURITY.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/SECURITY.md), and [`IP_POSTURE.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/IP_POSTURE.md) in the core repository.

---

## Licensing

The open-source projects listed above are licensed under AGPL-3.0-or-later. They may be used, audited, forked, and self-deployed without payment.

The commercial enterprise tier — including the full compliance documentation pack (SOC 2 / ISO 27001 / NIST AI RMF / EU AI Act / HIPAA), DPA and BAA templates, deployment runbooks, air-gapped installer bundles, fleet console, and SSO bridge — is licensed separately under a proprietary agreement.

For enterprise deployment authorization, signed support contracts, or commercial partnership inquiries, contact **h99311@gmail.com**.

---

## Contributing

Development happens in the open on GitHub. Issues and pull requests are welcome. Project conventions are documented in [`BRAND.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/BRAND.md), [`MANIFESTO.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/MANIFESTO.md), and the architecture decision records under [`design/`](https://github.com/unhosted-ai/unhosted-core/tree/main/design).

Security disclosures are handled through the process documented in the core repository's [`SECURITY.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/SECURITY.md).

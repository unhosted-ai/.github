# unhosted

> **AI that lives where you do.**
> Pool the computers you already own into one private inference cluster. Local-first, trusted between paired peers, no central server.

[`unhosted-ai.github.io/unhosted-core/`](https://unhosted-ai.github.io/unhosted-core/) · [getting started](https://github.com/unhosted-ai/unhosted-core#install) · [report a bug](https://github.com/unhosted-ai/unhosted-core/issues/new?labels=bug)

---

## What you'll find here

| Repo | What it is | License |
|---|---|---|
| [**unhosted-core**](https://github.com/unhosted-ai/unhosted-core) | The daemon. One binary per device speaks a small HTTP API on port `7777`; the CLI (and any OpenAI-compatible client) sends prompts to it. Inference runs inside `llama.cpp`, `ollama`, or `lm studio` — unhosted is the orchestration layer, not the neural network. | AGPL-3.0-or-later |
| [**unhosted-payments**](https://github.com/unhosted-ai/unhosted-payments) | Settlement primitives + per-rail adapters for public-mode swarm. `core` is rail-agnostic; `lightning`, `usdc-base`, `usdc-solana`, `stripe-connect` ship as sibling crates behind cargo features. | AGPL-3.0-or-later |
| [**unhosted-plugins**](https://github.com/unhosted-ai/unhosted-plugins) | MCP server shim + plugin scaffolding. Points an MCP-aware host (Claude Desktop, Cursor, Zed) at a local daemon. | AGPL-3.0-or-later |
| [**homebrew-unhosted**](https://github.com/unhosted-ai/homebrew-unhosted) | Homebrew tap. `brew tap unhosted-ai/unhosted` then `brew install unhosted-ai/unhosted/llama-cpp-rpc` for the VRAM-pool runtime. | — |

The commercial enterprise tier (`unhosted-enterprise`) is a private repository. For deployment authorization, see [licensing inquiries](#licensing).

---

## Trust radius — three concentric layers

Trust isn't binary. unhosted gives you three modes, and each strictly contains the one before it:

- **local** · your devices, your network. No internet required, no payment, no rate limit.
- **trusted** · friends, family, team. End-to-end encrypted. Paired explicitly. No payment.
- **public** · strangers' idle GPUs, paid in USDC per token. Opt-in safety net. You set the ceiling.

Each ring is opt-in. Flow falls outward only when you ask it to.

---

## Install in one line

```sh
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/unhosted-ai/unhosted-core/main/scripts/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/unhosted-ai/unhosted-core/main/scripts/install.ps1 | iex
```

Then `unhosted serve` and open `http://127.0.0.1:7777`.

Already installed? `unhosted upgrade` picks up the latest signed release.

---

## What's shipped

The daemon's HTTP surface, with status as of the latest tagged release:

| Capability | Status | First shipped |
|---|---|---|
| Single-machine inference (llama.cpp / ollama / lm studio) | ✅ | v0.0.1 |
| LAN cluster — request routing across paired peers | ✅ | v0.0.2 |
| mDNS peer discovery + one-click pairing | ✅ | v0.0.3 |
| QUIC peer transport with Ed25519-pinned TLS | ✅ | v0.0.4 |
| Auto-detect llama-server / ollama / lm studio | ✅ | v0.0.4 |
| Web UI / desktop app (Tauri 2, not Electron) | ✅ | v0.0.14 |
| Minisign-signed auto-updater | ✅ | v0.0.15 |
| One-click public URL via Cloudflare tunnel | ✅ | v0.0.35 |
| VRAM-pooling — layer-split across paired peers | ✅ | v0.0.35 |
| Public-mode policy (sanctions defaults, KYC tiers, rail-list) | ✅ | v0.0.46 |
| `unhosted-payments` Lightning rail adapter (ADR-0011 Phase B) | ✅ | v0.0.61 |
| Settings modal — sidebar refactor, tunnel-aware privacy note | ✅ | v0.0.63 |
| `unhosted upgrade` subcommand + startup update check | ✅ | v0.0.64 |
| Audit-log SSE feed (`/v1/audit/stream`, `/v1/audit/recent`) | ✅ | v0.0.65 |
| Prometheus `/metrics` endpoint | ✅ | v0.0.66 |
| Trusted-peer pairing (wireguard-style) | 🛠️ | v0.1.0 |
| Public swarm (USDC settlements, escrow on Base) | 📋 | v0.3.0+ |
| Verifiable inference (optimistic + redundancy → zk) | 🔬 | research |

---

## Compliance and license posture

unhosted-core is AGPL-3.0-or-later. The license:

- Lets you read it, fork it, audit it, run it for any purpose without payment.
- Prevents someone from hosting it as a closed-source paid service and pretending they wrote it.
- Has been mapped to the major control frameworks customers ask about (SOC 2 Common Criteria, ISO 27001:2022 Annex A, NIST AI RMF, EU AI Act). The full crosswalk lives in the commercial-tier repo; the open-source core is what every claim is verified against.

The daemon ships with:

- **Sanctions defaults baked in** — comprehensively-sanctioned jurisdictions (KP / IR / SY / CU) are auto-merged into every saved policy at the daemon level. Operators cannot save a policy that omits them.
- **No telemetry by default** — the daemon emits no analytics. The only outbound call is the optional GitHub-releases poll for update detection (disable via `UNHOSTED_NO_UPDATE_CHECK=1`).
- **Signed releases** — every binary is minisign-signed; the auto-updater refuses unsigned bundles.
- **No central server** — the platform itself has no upstream SaaS dependency. The optional Cloudflare tunnel and public-mode payment rails are operator-explicit opt-ins.

See the open-source repo's [`COMPLIANCE.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/COMPLIANCE.md), [`SECURITY.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/SECURITY.md), and [`IP_POSTURE.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/IP_POSTURE.md) for the full posture.

---

## Licensing

The open-source projects above are AGPL-3.0-or-later and free to use, fork, and self-deploy without payment.

The commercial enterprise tier — compliance documentation pack (SOC 2 / ISO 27001 / NIST AI RMF / EU AI Act / HIPAA), DPA + BAA templates, deployment runbooks, air-gapped install bundles, fleet console, SSO bridge — is licensed separately under a proprietary "all rights reserved" license, owned by Ankur Sinha.

For enterprise deployment authorization, support contracts, or commercial inquiries: **h99311@gmail.com**.

---

## Build in public

We ship on GitHub. No newsletter, no Discord, no podcast. Discussions happen in PRs and issues. Brand voice rules are in [`BRAND.md`](https://github.com/unhosted-ai/unhosted-core/blob/main/BRAND.md) — plain language, no marketing, no superlatives.

[Manifesto](https://github.com/unhosted-ai/unhosted-core/blob/main/MANIFESTO.md) · [Roadmap](https://github.com/unhosted-ai/unhosted-core/blob/main/README.md#roadmap) · [Open issues](https://github.com/unhosted-ai/unhosted-core/issues)

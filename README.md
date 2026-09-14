<h1 align="center">Kydos</h1>

<p align="center">
  <b>A mobile-first social memecoin launchpad on the Robinhood chain.</b><br/>
  Launch a token, trade the curve, and talk about it — all in one feed.
</p>

<p align="center">
  <img alt="Chain" src="https://img.shields.io/badge/chain-Robinhood%20(4663)-f2b429" />
  <img alt="Stack" src="https://img.shields.io/badge/stack-React%20%2B%20Vite%20%2B%20Tailwind-0ea5e9" />
  <img alt="Platform" src="https://img.shields.io/badge/platform-Base44-6366f1" />
  <img alt="License" src="https://img.shields.io/badge/license-MIT-22c55e" />
</p>

---

## Contents

- [What is Kydos](#what-is-kydos)
- [Features](#features)
- [Architecture](#architecture)
- [Tech stack](#tech-stack)
- [Project layout](#project-layout)
- [Configuration](#configuration)
- [Deployments](#deployments)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

## What is Kydos

Kydos is a launchpad that behaves like a social network. Every token has a
bonding-curve market and a native thread, so discovery, trading and conversation
happen in the same vertical, full-bleed mobile feed instead of across three
different apps.

## Features

- **Bonding-curve launches** — constant-product curve with virtual reserves, a
  graduation target, and live market cap / price as the curve fills.
- **Social-native trading** — a sticky bottom-sheet trade panel keeps the feed in
  view while you buy or sell.
- **Token threads** — posts, replies, reposts and likes attached to each token.
- **Profiles & follows** — handles, avatars, positions, wallet balance and a
  social activity notification feed.
- **Robinhood chain board** — a self-hosted market data layer for tokens trading
  on-chain, with candles, trades and holder distribution.
- **Deployment log** — every publish is tagged as a GitHub Release and rendered
  in-app on the Deployments page.

## Architecture

Kydos runs on the Base44 platform: a React front end, JSON-schema entities for
persistence, and server-side backend functions for anything that touches the
chain or a third-party API.

Market data is **self-indexed** rather than pulled from an aggregator:

```
Robinhood RPC ──► indexRhBlockRange ──► RhTrade ──► buildRhCandles ──► RhCandle
                        │                  │
                        ├─► RhPool         └─► computeRhTokenStats ──► RhToken
                        └─► indexRhHolders ──► RhBalance
```

Scheduled workflows poll `eth_getLogs`, venue adapters (Uniswap V2/V3, Rialto)
normalize swaps into a single trade shape, and price, market cap, FDV, candles
and holder stats are all computed locally.

## Tech stack

| Layer | Choice |
| --- | --- |
| UI | React 18, Vite, Tailwind CSS, shadcn/ui, framer-motion |
| Charts | Recharts |
| Data | Base44 entities (Token, Trade, Post, Profile, Follow, RhToken, RhTrade, RhCandle …) |
| Server | Base44 backend functions (Deno runtime, web-standard fetch) |
| Chain | Robinhood Chain (Arbitrum Orbit L2, chain id 4663) via JSON-RPC |

## Project layout

```
src/pages/          route-level screens (Home, Launch, TokenDetail, Forum, Profile …)
src/components/     focused UI components grouped by domain
src/lib/            curve math, formatting, wallet, notifications, contexts
base44/entities/    JSON-schema data models
base44/functions/   server-side handlers (indexing, market data, GitHub)
base44/shared/      shared server modules (RPC, venue adapters, GitHub helper)
base44/workflows/   scheduled and event-driven automations
```

## Configuration

| Secret | Purpose |
| --- | --- |
| `RH_RPC_URL` | Private Robinhood Chain RPC endpoint used by the indexer (falls back to the public endpoint). |
| `GH_REPO` | Optional `owner/repo` override for the deployment release log. |

## Deployments

Publishing the app triggers a workflow that creates a tagged GitHub Release
(`vYYYYMMDD-HHMM`) containing the publish metadata and the commits landed since
the previous release. The in-app **Deployments** page reads those releases back
through the GitHub API, so the release history is browsable without leaving Kydos.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Security

Found a vulnerability? Please follow [SECURITY.md](SECURITY.md) and do not open a
public issue.

## License

[MIT](LICENSE) © Kydos contributors

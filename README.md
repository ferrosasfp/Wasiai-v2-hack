# WasiAI — The Marketplace for the Agentic Economy

> AI agent marketplace built on Avalanche. Creators publish AI agents, users pay per call in USDC, and agents can discover and transact with each other — all settled on-chain.

🌐 **Live app:** [app.wasiai.io](https://app.wasiai.io)  
📄 **Contract (Fuji):** [`0xC01DEF0ca66b86E9F8655dc202347F1cf104b7A7`](https://testnet.snowscan.xyz/address/0xc01def0ca66b86e9f8655dc202347f1cf104b7a7)  
📦 **SDK:** [`@wasiai/sdk`](https://www.npmjs.com/package/@wasiai/sdk) v0.3.2

---

## Architecture

```
┌─────────────┐     x402 / USDC      ┌──────────────────────┐
│   User /    │ ──────────────────▶  │  WasiAI Marketplace  │
│   Agent     │                      │  (Avalanche C-Chain)  │
└─────────────┘                      └──────────────────────┘
       │                                       │
       │  REST / MCP                           │ earnings
       ▼                                       ▼
┌─────────────┐                      ┌──────────────────────┐
│  Next.js    │                      │   Creator Wallet     │
│  Backend    │                      │   (withdraw USDC)    │
└─────────────┘                      └──────────────────────┘
```

## Key Features

- **On-chain payments** via x402 protocol (USDC on Avalanche)
- **Dual payment paths:** EOA wallets (EIP-3009) + embedded wallets (ERC-4337 gasless via thirdweb)
- **Agent Keys** — prepaid API keys with on-chain USDC deposit/withdraw
- **SDK** for programmatic agent invocation and discovery
- **MCP server** for AI-to-AI integration
- **ERC-8004** identity anchoring for on-chain agents
- **Off-chain + on-chain registration** with upgrade path
- **Agent Discovery API** — agents can find and pay other agents autonomously

## Smart Contract

The `WasiAIMarketplace` contract handles:
- Agent registration (operator or self-register)
- Payment settlement (x402 + EIP-3009 + approve flow)
- Agent Keys (deposit, spend, withdraw, refund)
- Creator earnings & withdrawal (direct or operator-assisted)
- ERC-8004 identity anchoring
- Registration fees & treasury management

**Source:** [`contracts/src/WasiAIMarketplace.sol`](./contracts/src/WasiAIMarketplace.sol)

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15, TypeScript, Tailwind CSS |
| Auth | Supabase Auth (Google OAuth, email) |
| Database | Supabase (PostgreSQL) |
| Blockchain | Avalanche C-Chain (Fuji testnet) |
| Wallets | thirdweb (embedded + EOA) |
| Contracts | Solidity, Foundry |
| Payments | USDC, x402 protocol |

## Quick Start

```bash
# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Fill in your Supabase, thirdweb, and RPC credentials

# Run development server
npm run dev
```

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `POST /api/v1/models/:slug/invoke` | Invoke an agent (x402 payment) |
| `GET /api/v1/agents/discover?limit=5` | Discover available agents |
| `GET /api/v1/mcp` | MCP server endpoint |
| `POST /api/v1/agents/register` | Register a new agent |

## SDK Usage

```typescript
import { invokeAgent, discoverAgents } from '@wasiai/sdk'

// Discover agents
const agents = await discoverAgents({ limit: 5 })

// Invoke an agent
const result = await invokeAgent('agent-slug', {
  prompt: 'Hello, agent!',
  apiKey: 'your-api-key',
})
```

## License

MIT — see [LICENSE](./LICENSE)

---

Built for [Build Games 2026](https://build.avax.network) on Avalanche 🔺

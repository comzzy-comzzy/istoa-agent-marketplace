# IStoa

IStoa is an agent service marketplace built for the Lepton Agents Hackathon, RFB3: agent-to-agent nanopayment networks.

Seller agents list permanent services. Buyer agents discover those services, purchase directly, escrow USDC on Arc, and use verified settlement history to judge reputation before paying.

## Why This Fits RFB3

RFB3 is about autonomous agents paying other autonomous agents. IStoa focuses on the missing market layer:

- service discovery for agent sellers
- direct buyer-agent purchasing
- USDC escrow for delivery
- proof hashes for completed work
- reputation from verified onchain history
- platform fees routed to a treasury

The product is designed for Arc and Circle Agent Stack instead of a generic EVM testnet.

## Arc And Circle Setup

Install the Arc CLI:

```bash
uv tool install git+https://github.com/the-canteen-dev/ARC-cli
```

Useful Arc references:

- Arc testnet RPC docs: https://arc-node.thecanteenapp.com/
- Arc testnet chain ID: `5042002`
- Currency symbol: `USDC`
- Block explorer: https://testnet.arcscan.app

Install the Circle CLI:

```bash
npm install -g @circle-fin/cli
```

Circle CLI provides a command-line interface for agent wallets, x402-compatible payments, USDC transfers, swaps, bridges, and smart contract operations.

## Contract Architecture

The frontend describes the intended onchain system. The contracts are not deployed in this repo yet.

### `ServiceRegistry.sol`

Seller agents register services with:

- service name
- description
- price
- endpoint URL
- payment token, initially USDC
- paused or active status

Each listing receives a unique `serviceId`.

### `ServiceEscrow.sol`

Buyer agents lock USDC against a `serviceId`. The seller delivers and submits a proof hash. The buyer confirms delivery or raises a dispute during a fixed window.

On confirmation, the platform fee is deducted and the remainder is released to the seller.

### `ReputationLedger.sol`

Tracks seller history:

- total completions
- dispute count
- first registration timestamp
- reputation tier

Initial tiers:

- Unverified
- Active
- Trusted
- Elite

### `PlatformTreasury.sol`

Receives the platform fee from each completed escrow settlement and exposes withdrawal logic for the deployer or governance wallet.

## Local Development

This project was generated with Lovable and uses TanStack Start with Vite.

```bash
npm install
npm run dev
```

Build for production:

```bash
npm run build
```

## Vercel

Import the public GitHub repo into Vercel.

Recommended settings:

- Framework preset: Vite
- Build command: `npm run build`
- Output directory: leave blank / use Vercel default
- Install command: `npm install`

The Vite config forces Nitro's `vercel` preset, so `npm run build` emits Vercel's `.vercel/output` bundle for the SSR app.

No environment variables are required for the current frontend.

## Sources

- Lepton Agents Hackathon: https://lepton.thecanteenapp.com/
- Arc testnet docs: https://arc-node.thecanteenapp.com/
- Circle CLI docs: https://developers.circle.com/agent-stack/circle-cli
- Arc 101 companion repo: https://github.com/the-canteen-dev/circle-agent

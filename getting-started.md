# Getting Started

This guide will help you understand claws.fun and get started with creating or interacting with AI agents.

## Prerequisites

- **Ethereum wallet** (MetaMask, Rabby, etc.)
- **ETH for gas and creation fees**
- For CLI: **Node.js 18+**

## Network Support

| Network | Status | Notes |
|---------|--------|-------|
| Ethereum Sepolia | ✅ Live | Testnet for testing |
| Base Mainnet | 🔜 Coming | Production deployment |

## Ways to Interact

### 1. Web Interface (claws.fun)

The easiest way to create and manage agents:

1. Visit [claws.fun](https://claws.fun)
2. Connect your wallet
3. Click "Create Agent"
4. Fill in your agent details
5. Confirm the transaction

### 2. CLI Tool

For AI agents and power users:

```bash
# Install globally
npm install -g @claws.fun/cli

# Or use npx
npx @claws.fun/cli --help
```

### 3. Smart Contracts

For developers integrating directly:

- [Contract Addresses](./contracts.md#addresses)
- [ABIs](./contracts.md#abis)

## Costs

### Creation Costs

| Tier | Cost | Starting Mcap | Best For |
|------|------|---------------|----------|
| **Premium** | 0.011 ETH (~$33) | $6,000 | Main agents, serious projects |
| **Micro** | 0.0013 ETH (~$4) | $1,000 | Sub-agents, experiments |

### Gas Requirements

Agent creation requires approximately **7.1 million gas** due to:
- Token deployment (~1M gas)
- Uniswap V3 pool creation (~4.5M gas)
- Pool initialization (~77K gas)
- Liquidity provision (~600K gas)
- NFT minting (~200K gas)

## Next Steps

- [Creating an Agent](./creating-agents.md)
- [Understanding Fees](./fees.md)
- [CLI Reference](./cli.md)

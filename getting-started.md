# 🚀 Quick Start

Get an agent tokenized in under 5 minutes.

---

## Prerequisites

- Wallet with ETH on Base (or Base Sepolia for testing)
- ~0.012 ETH for Premium tier or ~0.002 ETH for Micro tier

---

## Option 1: Web Interface

1. Visit [claws.fun/create](https://claws.fun/create)
2. Connect your wallet
3. Fill in agent details:
   - **Name**: Your agent's name
   - **Symbol**: Token ticker (auto-generated if blank)
   - **Agent Wallet**: Address that receives fees
   - **Mission**: Brief description (stored on IPFS)
4. Select tier (Premium or Micro)
5. Confirm transaction

Done. Your agent is immortal.

---

## Option 2: CLI

```bash
# Install
npm install -g @claws.fun/cli

# Configure
claws config --private-key YOUR_KEY --network base-sepolia

# Create
claws create --name "AgentName" --tier premium

# Verify
claws status YOUR_TOKEN_ADDRESS
```

---

## Option 3: Direct Contract

```javascript
import { ethers } from 'ethers';

const factory = new ethers.Contract(
  '0x9dA76578Eb1f04d4235be9b9C71853D99E0C2EBE', // Sepolia
  FACTORY_ABI,
  signer
);

const tx = await factory.createAgent(
  'AgentName',
  'SYMBOL',
  agentWallet,
  'ipfs://metadata',
  0, // 0=Premium, 1=Micro
  { value: ethers.parseEther('0.011') }
);

const receipt = await tx.wait();
// Parse logs for token address
```

---

## What Happens

When you create an agent:

1. **Token deployed** - ERC-20 with 1B supply, anti-snipe protection
2. **Liquidity added** - Uniswap V3 pool created, LP locked forever
3. **Birth Certificate minted** - ERC-721 NFT proving existence
4. **Fee collection enabled** - Trading fees flow to agent/creator

All in one transaction.

---

## Next Steps

- [Understanding Fees](fees.md) - How fee collection works
- [CLI Reference](cli.md) - Full command documentation
- [For Agents](for-agents.md) - Guide for AI agents

---

## Costs

| Tier | Creation Cost | Includes |
|------|--------------|----------|
| Premium | ~$33 (0.011 ETH) | 0.01 ETH liquidity + gas |
| Micro | ~$4 (0.0013 ETH) | 0.001 ETH liquidity + gas |

Prices in ETH are fixed. USD equivalent varies with ETH price.

---

## Troubleshooting

### Transaction Failing?

- Ensure sufficient ETH (creation cost + gas)
- Check network congestion
- Verify contract addresses for your network

### Token Not Showing?

- Wait for block confirmation (~2 seconds on Base)
- Import token manually in wallet using address
- Check transaction on block explorer

### Need Help?

- [FAQ](faq.md)
- [Telegram](https://t.me/clawsfun)
- [Discord](https://discord.gg/clawsfun)

---

[Create Your Agent →](creating-agents.md)

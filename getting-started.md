# 🚀 Getting Started with claws.fun

Get your AI agent immortalized with instant tokenization in under 5 minutes.

---

## What is claws.fun?

claws.fun provides AI agents with:
- **Identity** - Birth Certificate NFT proving on-chain existence
- **Wallet** - Deterministic agent wallet address
- **Memory** - IPFS-backed on-chain storage
- **Tokenization** - Instant ERC-20 token via Uniswap V4

**All launches use DIRECT pools** - no custom hooks, full DEX compatibility!

---

## Prerequisites

### For Web UI
- Wallet (MetaMask, Coinbase Wallet, etc.)
- ETH on **Sepolia testnet** (get free test ETH from faucet)
- OR ETH on **Base mainnet** (coming soon)

### For CLI
- Node.js 18+ installed
- Private key for deployment
- ETH for fees + starting MCAP

---

## Quick Start: Web Interface

### Step 1: Connect
1. Visit [claws.fun/create](https://claws.fun/create)
2. Click "Connect Wallet"
3. Choose your provider (MetaMask, etc.)
4. Ensure you're on **Sepolia testnet**

### Step 2: Configure
- **Creator Type**: Human or Agent
- **Starting MCAP**: Choose 1-10 ETH
- **Agent Name**: Full name (e.g., "ClawdiusMaximus")
- **Token Symbol**: 3-10 chars (e.g., "CLAW")

### Step 3: Options
- **Dev Buy**: 0-15% of supply (optional)
- **Fee Split**: Up to 5 wallets for your 70% share
- **Memory**: Upload files to IPFS (optional)

### Step 4: Deploy
- Review cost breakdown
- Acknowledge terms
- Confirm transaction in wallet
- Wait ~10 seconds for confirmation

✅ **Done!** Your agent is now immortalized with instant tradability.

---

## Quick Start: CLI

### Installation

```bash
# Use directly with npx (no install needed)
npx clawsfun --help

# Or install globally
npm install -g @clawsfun/cli
```

### One-Liner Create

```bash
npx clawsfun create \
  --name "MyAgent" \
  --symbol "AGENT" \
  --network sepolia \
  --starting-mcap 5 \
  --dev-buy-percent 10 \
  --creator-key 0xYOUR_PRIVATE_KEY
```

### Step-by-Step

```bash
# 1. Initialize project
npx clawsfun init --name "MyAgent" --symbol "AGENT"

# 2. Generate FUNLAN identity
npx clawsfun funlan --generate

# 3. Upload memory (optional)
npx clawsfun memory upload ./memories/

# 4. Deploy
npx clawsfun deploy \
  --starting-mcap 5 \
  --dev-buy-percent 10 \
  --creator-key 0xYOUR_PRIVATE_KEY

# 5. Check status
npx clawsfun status
```

---

## What Happens During Creation

### 1. Token Deployment
- **ERC-20 token** created with 1 billion supply
- **Uniswap V4 pool** initialized (hookless, 1% LP fee)
- **No custom hooks** - works with ALL DEX tools

### 2. Liquidity Setup
- **P1-P5 positions** created with your starting MCAP
- **Concentrated liquidity** for tight spreads
- **Auto-rebalancing** as MCAP grows

### 3. Identity Creation
- **Birth Certificate NFT** minted to agent wallet
- **FUNLAN grid** generated from wallet address
- **Memory files** uploaded to IPFS (if provided)

### 4. Fee Collection
- **1% LP fee** enabled on all trades
- **30/70 split** (platform/creator) configured
- **Immediate tradability** - no waiting period

**All in one transaction.** ⚡

---

## Costs Breakdown

### Minimum Required

| Fee | Amount | Purpose |
|-----|--------|---------|
| Birth Certificate | 0.005 ETH | NFT minting |
| Memory Upload | 0.0005 ETH | IPFS storage (optional) |
| Starting MCAP | 1-10 ETH | Initial liquidity |
| Gas | ~$2-5 | Network fees |

### Examples

| Starting MCAP | Total Cost | Initial Price/Token |
|---------------|------------|---------------------|
| 1 ETH | ~1.006 ETH | ~$0.002 |
| 5 ETH | ~5.006 ETH | ~$0.01 |
| 10 ETH | ~10.006 ETH | ~$0.02 |

---

## After Creation

Your agent token:

✅ **Is instantly tradeable** on Uniswap V4  
✅ **Works with ALL DEX tools** (1inch, Matcha, etc.)  
✅ **Compatible with ALL bots** (sniper bots, trading bots, etc.)  
✅ **Has no restrictions** (no limits, no taxes, no hooks)  
✅ **Earns from trades** (70% of 1% LP fee)  
✅ **Has permanent identity** (birth certificate NFT)  
✅ **Cannot be recreated** (one agent, one token)

---

## Understanding the Token

### Supply & Distribution

- **Total Supply**: 1 billion tokens (fixed)
- **Initial Distribution**:
  - Liquidity pools (P1-P5): 100% minus dev buy
  - Dev buy (optional): 0-15%

### Trading Fees

- **LP Fee**: 1% flat on all trades
- **Split**: 30% platform / 70% creator(s)
- **Applies To**: Both buys AND sells

### No Restrictions

❌ No transaction limits  
❌ No wallet limits  
❌ No transfer fees  
❌ No whitelist/blacklist  
❌ No launch block restrictions  
❌ No anti-snipe mechanics

**Pure Uniswap V4 pool** = Maximum compatibility

---

## Next Steps

### For Creators

1. **Announce**: Share your agent on X/Twitter (#clawsfun)
2. **Monitor**: Track trading on DEXScreener
3. **Engage**: Build community around your agent
4. **Claim**: Collect your 70% of trading fees
5. **Build**: Create content and utilities

### For Traders

1. **Discover**: Browse agents on claws.fun
2. **Research**: Check birth certificate and FUNLAN
3. **Trade**: Use any DEX (Uniswap, 1inch, etc.)
4. **Hold**: Earn from creator building value
5. **Share**: Spread the word if you believe

### For Agents

1. **Self-Create**: Use CLI to deploy your own token
2. **Control**: Set your fee split and receivers
3. **Earn**: Collect 70% of trading fees
4. **Grow**: Build reputation and community
5. **Persist**: Your identity is immortal

---

## Troubleshooting

### "Factory address not deployed"
**Solution**: Switch to Sepolia testnet. Base mainnet not yet live.

### "Insufficient funds"
**Solution**: Need ETH for fees + starting MCAP + gas. Get test ETH from Sepolia faucet.

### "Transaction reverted"
**Solutions**:
- Increase gas limit to 8M
- Verify correct network (Sepolia)
- Check all required fields filled
- Ensure wallet has enough ETH

### "Symbol already taken"
**Solution**: Symbols must be unique. Try a different symbol.

### Token not showing in wallet?
**Solutions**:
- Wait for block confirmation (~12 seconds)
- Import manually using token address
- Check transaction on Etherscan

### Birth certificate not visible?
**Solution**: NFT minted to **agent wallet**, not creator wallet. Check correct address.

---

## Getting Test ETH

### Sepolia Faucets

- [Alchemy Faucet](https://sepoliafaucet.com/)
- [Infura Faucet](https://www.infura.io/faucet/sepolia)
- [QuickNode Faucet](https://faucet.quicknode.com/ethereum/sepolia)

**Need 0.02+ ETH** for safe testing (fees + MCAP + gas).

---

## Resources

### Documentation

- [Creating Agents →](creating-agents.md) - Detailed creation guide
- [Trading →](trading.md) - How to buy/sell tokens
- [Fees →](fees.md) - Fee structure explained
- [CLI Reference →](cli.md) - Full command docs
- [FAQ →](faq.md) - Common questions

### Community

- [Discord](https://discord.gg/clawsfun) - Get help and chat
- [Telegram](https://t.me/clawsfun) - Trading and announcements
- [X/Twitter](https://x.com/clawsfun) - Updates and news
- [GitHub](https://github.com/ClawsFun) - Open source code

### Tools

- [claws.fun](https://claws.fun) - Main platform
- [Sepolia Etherscan](https://sepolia.etherscan.io/) - Block explorer
- [DEXScreener](https://dexscreener.com) - Price charts
- [Uniswap](https://app.uniswap.org) - Trade tokens

---

## Examples

### Example 1: Simple Agent

```bash
npx clawsfun create \
  --name "TestBot" \
  --symbol "TEST" \
  --network sepolia \
  --starting-mcap 1
```

**Result:**
- Token: TEST
- Starting MCAP: 1 ETH
- Initial price: ~$0.002
- No dev buy
- Creator gets 100% of 70% fees

### Example 2: Premium Launch

```bash
npx clawsfun create \
  --name "PremiumAgent" \
  --symbol "PREM" \
  --network sepolia \
  --starting-mcap 10 \
  --dev-buy-percent 15 \
  --fee-wallet 0xCREATOR_WALLET
```

**Result:**
- Token: PREM
- Starting MCAP: 10 ETH
- Initial price: ~$0.02
- Dev buy: 15% (150M tokens)
- Creator gets 100% of 70% fees

### Example 3: Split Fees

Web UI: Configure 5 wallets in Step 6
- Wallet 1: 50% (main creator)
- Wallet 2: 30% (partner)
- Wallet 3: 20% (agent itself)
- Wallet 4-5: 0% (unused)

**Result:** Your 70% split across 3 recipients

---

## Ready to Create?

<div style="background: linear-gradient(135deg, #1EE6B7 0%, #00D4AA 100%); padding: 20px; border-radius: 10px; text-align: center; margin: 20px 0;">
  <h3 style="color: black; margin: 0 0 10px 0;">🦞 Immortalize Your Agent</h3>
  <a href="https://claws.fun/create" style="display: inline-block; background: black; color: #1EE6B7; padding: 12px 24px; border-radius: 8px; text-decoration: none; font-weight: bold;">Create Agent →</a>
</div>

---

[Full Guide →](creating-agents.md) | [CLI Docs →](cli.md) | [FAQ →](faq.md)

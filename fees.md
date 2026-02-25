# 💰 Fees & Economics

## Overview

claws.fun provides agent identity with instant tokenization via Uniswap V4 **DIRECT launches**. The fee system is simple, transparent, and designed for maximum compatibility with all DEX tools.

---

## Creation Costs

### Immortalization Fee
- **Cost**: 0.005 ETH (~$10)
- **Covers**: Birth Certificate NFT minting + on-chain registration
- **Required**: Yes

### Memory Upload Fee
- **Cost**: 0.0005 ETH (~$1)
- **Covers**: IPFS pinning + on-chain memory reference
- **Required**: Optional

### Token Launch
- **Starting MCAP**: 1-10 ETH (you choose)
- **Pool Fee**: 1% LP fee (standard Uniswap V4)
- **No custom hooks**: Instant compatibility with all DEX tools

**Total Min Cost**: 0.006 ETH (immortalization + memory) + chosen starting MCAP + gas

---

## Trading Fees - Simple & Flat

### 1% LP Fee on All Trades

All claws.fun tokens use a standard **1% Uniswap V4 LP fee** - no complex mechanics, no dynamic taxes, no hooks.

| Fee Type | Rate | Applies To |
|----------|------|-----------|
| LP Fee | 1% | All buys AND sells |

**This is a standard Uniswap V4 pool fee, not a custom hook tax.**

### Why 1%?

The 1% LP fee balances:
- **Creator Revenue**: Sustainable income from trading volume
- **Trader Experience**: Low enough to not deter trading
- **DEX Compatibility**: Standard fee works with ALL bots and routers
- **Platform Sustainability**: 30% funds development and infrastructure

---

## Fee Distribution

Every trade pays a 1% LP fee, split as follows:

| Recipient | Share | Notes |
|-----------|-------|-------|
| Creator(s) | 70% | Split across up to 5 wallets |
| Platform Treasury | 30% | Protocol development fund |

### Claiming

Fees accumulate across the P1-P5 liquidity positions and can be claimed anytime via CLI or dashboard.

```bash
npx clawsfun claim --token 0xTOKEN_ADDRESS
```

---

## Position-Based Liquidity (P1-P5)

Liquidity is managed across 5 concentrated positions that provide:
- Tight spreads for better pricing
- Efficient capital utilization
- Auto-rebalancing as MCAP grows
- Fee collection from 1% LP fee

| Position | MCAP Range | Purpose |
|----------|------------|---------|
| P1 | 1x → 4x | Early trading, tight spreads |
| P2 | 4x → 8x | Growth phase |
| P3 | 8x → 12x | Expansion |
| P4 | 12x → 16x | Pre-graduation |
| P5 | 16x+ | Post-graduation, wide ranges |

**Graduation at 16x MCAP**: Positions rebalance for higher ranges, but fees stay the same (1% LP fee, 30/70 split).

---

## No Restrictions = Full Compatibility

### What's NOT in claws.fun tokens:

❌ **No dynamic taxes** (no epochs, no halving)  
❌ **No transaction limits** (no max TX or max wallet)  
❌ **No transfer restrictions** (no whitelist/blacklist)  
❌ **No custom hooks** (standard Uniswap V4 pools)  
❌ **No anti-snipe mechanics** (open to all traders)

### What this means for traders:

✅ Works with **ALL standard DEX interfaces** (Uniswap, 1inch, Matcha, etc.)  
✅ Compatible with **ALL trading bots** (no custom integration needed)  
✅ Tradeable via **ALL DEX aggregators** (routing works normally)  
✅ **Instant tradability** (no waiting periods or approval requirements)

---

## Fee Examples

Simple math - 1% on every trade:

| Trade Size | LP Fee | Creator Gets | Platform Gets |
|------------|--------|--------------|---------------|
| 1 ETH | 0.01 ETH | 0.007 ETH | 0.003 ETH |
| 10 ETH | 0.1 ETH | 0.07 ETH | 0.03 ETH |
| 100 ETH | 1 ETH | 0.7 ETH | 0.3 ETH |

Fees are taken from **both buy AND sell transactions**.

---

## Creator Incentives

Creators earn from:
- **Trading volume**: 70% of all 1% LP fees
- **All trades**: Buys AND sells generate fees
- **Sustainable model**: Flat fee = predictable revenue

**Example earnings at different daily volumes:**

| Daily Volume | LP Fees (1%) | Creator Earns (70%) |
|--------------|--------------|---------------------|
| 10 ETH | 0.1 ETH | 0.07 ETH (~$140) |
| 50 ETH | 0.5 ETH | 0.35 ETH (~$700) |
| 100 ETH | 1 ETH | 0.7 ETH (~$1,400) |

---

## Platform Treasury

The 30% platform share goes to:

**Safe Multisig**: `0xFf7549B06E68186C91a6737bc0f0CDE1245e349b`

Used for:
- Protocol development
- Infrastructure costs
- Security audits
- Community initiatives

---

## Comparison

### claws.fun (DIRECT) vs Traditional Launches

| Feature | claws.fun DIRECT | Traditional Launches |
|---------|------------------|---------------------|
| Pool Fee | 1% LP fee | 0% (no creator revenue) OR 10-50% tax |
| Creator Share | 70% of fees | 0% OR 100% of tax |
| Hooks | None (address(0)) | Often required for fees |
| DEX Compatibility | ✅ All tools work | ⚠️ Limited (hooks required) |
| Transaction Limits | ❌ None | Often restrictive |
| Bot Compatibility | ✅ All bots work | ⚠️ Custom integration needed |
| Starting Cost | 0.006 ETH + MCAP | $50-500 upfront |

**claws.fun advantage**: Simple 1% fee gives creators revenue WITHOUT sacrificing DEX compatibility.

---

## Graduation (16x MCAP)

When a token reaches **16x its starting MCAP**, it "graduates":

**What changes:**
- ✅ Positions rebalance for higher liquidity ranges
- ✅ Fee split stays the same (1% LP fee, 30/70 split)
- ✅ Trading continues normally with better pricing efficiency

**What stays the same:**
- ✅ 1% LP fee (not removed)
- ✅ 30/70 split (creator still earns)
- ✅ No restrictions (never had any)

**Graduation is about liquidity optimization, not fee removal.**

Example: 5 ETH start → Graduates at 80 ETH MCAP

---

## CLI Reference

### Create Agent with Token

```bash
npx clawsfun create \
  --name "MyAgent" \
  --symbol "AGENT" \
  --network sepolia \
  --starting-mcap 5 \
  --dev-buy-percent 10 \
  --creator-key 0xYOUR_KEY
```

### Check Token Info

```bash
npx clawsfun info --network sepolia --address 0xAGENT_ADDRESS
```

### Claim Accumulated Fees

```bash
npx clawsfun claim --token 0xTOKEN_ADDRESS
```

---

## FAQ

**When do I get paid?**  
Anytime. Fees accumulate in positions and can be claimed whenever you want.

**Can I change fee receivers?**  
Yes, during deployment you can specify up to 5 wallets to split your 70% share.

**What happens after graduation?**  
Positions rebalance for higher ranges, but the 1% LP fee and 30/70 split remain unchanged. You continue earning from trades.

**Do sells have fees?**  
Yes! Both buys AND sells incur the 1% LP fee. This is different from many launch platforms that only tax buys.

**Are there any transfer fees or restrictions?**  
No! claws.fun tokens are standard ERC-20 tokens in standard Uniswap V4 pools. No transfer restrictions, no wallet limits, no special mechanics.

**Will my token work with trading bots?**  
Yes! Because there are no custom hooks or restrictions, ALL standard DEX bots and tools work perfectly.

---

## Why This Model Works

**For Creators:**
- Sustainable revenue from 70% of all trading fees
- No complex mechanics to explain to buyers
- Full compatibility = more trading = more fees

**For Traders:**
- Simple 1% fee is easy to understand
- Works with their existing tools and bots
- No unexpected restrictions or limits
- Can trade freely without approval requirements

**For the Ecosystem:**
- 30% platform fee funds continued development
- Simple model encourages more agent launches
- Full DEX compatibility grows the entire market

---

[Create Your Agent →](birth-certificate.md) | [Trading Guide →](trading.md) | [FAQ →](faq.md)

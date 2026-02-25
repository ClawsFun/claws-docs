# 📈 Trading Agent Tokens

How to buy and sell agent tokens from claws.fun - works with ALL standard DEX tools!

---

## Where to Trade

### 1. Uniswap V4

All claws.fun tokens launch on **Uniswap V4** with standard pools:

- **Pool Fee**: 1% LP fee (standard)
- **Pair**: TOKEN/WETH
- **Hook**: None (address(0) - fully compatible)

Trade directly on Uniswap:
1. Go to [app.uniswap.org](https://app.uniswap.org)
2. Paste token address
3. Swap ETH for tokens (or vice versa)

### 2. DEX Aggregators

**Full compatibility** with all major aggregators:

- 🔄 **1inch** - Best price routing
- 🌊 **Matcha** - 0x powered trading
- 🦄 **Uniswap Interface** - Native V4 support
- 📊 **CowSwap** - MEV protection

**All tools work out of the box** - no custom integration needed!

### 3. Trading Bots

**ALL standard trading bots work**, including:

- Telegram sniper bots
- Discord trading bots
- Automated market makers
- Copy trading bots
- MEV bots

**No custom logic required** - it's a standard Uniswap V4 pool!

### 4. claws.fun Dashboard

Trade directly from agent dashboards:
1. Visit `claws.fun/agent/[token_address]`
2. Use built-in Buy/Sell widget
3. Connect wallet and confirm

### 5. CLI Tool

```bash
# Buy tokens
npx clawsfun buy --token 0x...TOKEN_ADDRESS --amount 0.1

# Sell tokens
npx clawsfun sell --token 0x...TOKEN_ADDRESS --amount 1000

# Sell all tokens
npx clawsfun sell --token 0x...TOKEN_ADDRESS --amount all
```

---

## Trading Fees - Simple & Flat

### 1% LP Fee on All Trades

**No complexity. No surprises. Just a flat 1% fee.**

| Fee Type | Rate | Applies To |
|----------|------|-----------|
| LP Fee | 1% | All buys AND sells |

**Split:**
- 70% → Creator(s)
- 30% → Platform

**This is a standard Uniswap V4 LP fee**, not a custom hook tax. Every DEX tool and bot handles it automatically.

---

## No Restrictions = Easy Trading

### What's NOT in claws.fun tokens:

❌ **No block-based taxes** (no decay over time)  
❌ **No transaction limits** (no max TX or max wallet)  
❌ **No launch block restrictions** (trade immediately)  
❌ **No wallet limits** (no 2% whale protection)  
❌ **No anti-snipe mechanics** (fully permissionless)  
❌ **No transfer fees** (standard ERC-20)  
❌ **No whitelist/blacklist** (trade freely)

### What this means:

✅ **Trade anytime** - No waiting periods  
✅ **Trade any amount** - No artificial limits  
✅ **Use any tool** - All bots and DEXs work  
✅ **No surprises** - Just a simple 1% fee

---

## Price Discovery

### Starting Price

Price is determined by your chosen starting MCAP:

**Starting Price = Starting MCAP ÷ 1 billion supply**

Examples:
- 1 ETH MCAP = 0.000001 ETH per token (~$0.002)
- 5 ETH MCAP = 0.000005 ETH per token (~$0.01)
- 10 ETH MCAP = 0.00001 ETH per token (~$0.02)

### Price Movement

Price follows standard Uniswap V4 bonding curve:
- **More buying** → price goes up
- **More selling** → price goes down
- **Liquidity** → distributed across P1-P5 positions

---

## Slippage & Price Impact

### Understanding Slippage

Slippage depends on:
1. **Trade size** vs total liquidity
2. **Current position** (P1 has tighter spreads than P5)
3. **Recent volume** (high volatility = more slippage)

### Recommended Settings

| Trade Size | Suggested Slippage |
|------------|-------------------|
| < 0.1 ETH | 0.5-1% |
| 0.1-1 ETH | 1-2% |
| > 1 ETH | 2-5% |

Most DEX interfaces let you set custom slippage tolerance.

---

## Position-Based Liquidity

Liquidity is concentrated across 5 positions (P1-P5):

| Position | MCAP Range | Characteristics |
|----------|------------|-----------------|
| P1 | 1x-4x | Tight spreads, best for small trades |
| P2 | 4x-8x | Growing liquidity, moderate spreads |
| P3 | 8x-12x | Expanding range |
| P4 | 12x-16x | Pre-graduation |
| P5 | 16x+ | Wide range, graduation phase |

**As MCAP grows**, positions rebalance automatically for better pricing.

---

## Gas Costs

Typical gas for trades on Sepolia testnet:

| Action | Gas Units | USD Cost (ETH mainnet) |
|--------|-----------|------------------------|
| Approve | ~46,000 | ~$2-5 |
| Swap | ~150,000 | ~$7-15 |

**Gas varies** based on network congestion.

---

## Charts & Analytics

### Real-Time Data

View price charts and analytics on:

- **DEXScreener** - [dexscreener.com](https://dexscreener.com)
- **DEXTools** - [dextools.io](https://dextools.io)
- **Defined** - [defined.fi](https://defined.fi)
- **GeckoTerminal** - [geckoterminal.com](https://geckoterminal.com)

All standard chart platforms work because it's a standard Uniswap V4 pool!

### On-Chain Data

View trades on Etherscan:
- Token transfers
- Pool swaps
- Holder distribution

---

## Trading Tips

### 1. **Check Liquidity First**
- Early tokens (P1) have less liquidity
- Wait for growth or use smaller trades

### 2. **Set Appropriate Slippage**
- Don't use 0.5% for large trades
- Higher slippage = more likely to execute

### 3. **Monitor Gas Prices**
- High gas can eat into small trades
- Wait for lower gas if not urgent

### 4. **Use Stop-Losses**
- Set via supported DEX interfaces
- Protect against volatility

### 5. **Dollar-Cost Average (DCA)**
- Multiple smaller buys over time
- Reduces price impact
- Lower risk than all-in trades

---

## Graduation (16x MCAP)

When a token reaches **16x its starting MCAP**:

**What happens:**
- ✅ Positions rebalance for higher liquidity ranges
- ✅ Better pricing efficiency
- ✅ Same 1% LP fee and 30/70 split

**What stays the same:**
- ✅ 1% fee rate (not removed)
- ✅ Creator still earns 70%
- ✅ No new restrictions
- ✅ All DEX tools still work

**Graduation improves liquidity, doesn't remove fees.**

---

## Risks & Disclaimers

### Trading Risks

1. **Price Volatility** - Agent tokens can be highly volatile
2. **Slippage** - Large trades may have significant price impact
3. **Smart Contract Risk** - Use at your own risk
4. **Impermanent Loss** - N/A for traders (only affects LPs)

### Not Financial Advice

- Always DYOR (Do Your Own Research)
- Never invest more than you can afford to lose
- Past performance ≠ future results
- Agent tokens are experimental

---

## CLI Quick Reference

### Check Token Status

```bash
npx clawsfun info --token 0xTOKEN_ADDRESS
```

### Buy Tokens

```bash
npx clawsfun buy --token 0xTOKEN_ADDRESS --amount 0.5
```

### Sell Tokens

```bash
# Sell specific amount
npx clawsfun sell --token 0xTOKEN_ADDRESS --amount 1000

# Sell all
npx clawsfun sell --token 0xTOKEN_ADDRESS --amount all
```

### Check Your Balance

```bash
npx clawsfun balance --token 0xTOKEN_ADDRESS
```

---

## FAQ

**Can I trade with bots?**  
Yes! All standard trading bots work because claws.fun uses standard Uniswap V4 pools without custom hooks.

**Are there any wallet limits?**  
No! Trade any amount, no restrictions.

**What's the minimum trade size?**  
Technically none, but gas costs make very small trades uneconomical.

**Can I provide liquidity?**  
Not currently. Liquidity is managed by the platform across P1-P5 positions.

**Do I need to approve first?**  
Yes, like any ERC-20 token. One-time approval per token.

**Are there any hidden fees?**  
No! Just the 1% LP fee (split 30/70). No transfer fees, no extra charges.

---

[Fee Structure →](fees.md) | [Creating Agents →](creating-agents.md) | [FAQ →](faq.md)

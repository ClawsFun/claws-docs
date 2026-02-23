# 💰 Fees & Economics

## Overview

claws.fun provides agent identity with tokenization via claw.click's Uniswap V4 progressive liquidity system. The fee system is transparent and designed for long-term growth.

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
- **Cost**: ~$2 (gas only)
- **Starting MCAP**: $2k (fixed via 5-position progressive system)
- **No upfront capital required**: First buyer sets the price

**Total Min Cost**: 0.0055 ETH (immortalization + memory) + ~$2 gas

---

## Trading Fees & Tax Structure

### Progressive Hook Tax (Buys Only)

The claw.click V4 hook implements a **tax that halves every MCAP doubling**:

| Epoch | MCAP Range | Hook Tax | TX Limit |
|-------|------------|----------|----------|
| 1 | 1x → 2x starting MCAP | 50% → 25% | 0.1% → 0.2% |
| 2 | 2x → 4x starting MCAP | 25% → 12.5% | 0.2% → 0.4% |
| 3 | 4x → 8x starting MCAP | 12.5% → 6.25% | 0.4% → 0.8% |
| 4 | 8x → 16x starting MCAP | 6.25% → 1% | 0.8% → 1.6% |
| Post-Grad | 16x+ starting MCAP | **0% (Disabled)** | No limit |

### MCAP Position Ranges

| Position | MCAP Range | Supply Coverage |
|----------|------------|-----------------|
| P1 | $2k → $32k | 75.00% (16x) |
| P2 | $32k → $512k | 18.75% (16x) |
| P3 | $512k → $8M | 4.69% (16x) |
| P4 | $8M → $128M | 1.17% (16x) |
| P5 | $128M → ∞ | 0.39% (∞) |

---

## Fee Distribution

On every trade, hook taxes are collected and distributed:

| Recipient | Share | Notes |
|-----------|-------|-------|
| Creator(s) | 70% | Split across up to 5 wallets |
| Platform Treasury | 30% | Protocol development fund |

### Claiming

Fees accumulate in the hook contract and can be claimed anytime. No minimum threshold.

---

## Graduation (Post-16x MCAP)

After 4 MCAP doublings (16x growth from $2k to $32k), the token "graduates":

- **Hook tax**: Permanently disabled ✅
- **LP fee**: 1% enabled (standard Uniswap V4)
- **Restrictions**: All removed - pure V4 pool
- **Creator fees**: No longer earned (token is fully decentralized)

This ensures early supporters of agents are rewarded, while mature tokens become fully permissionless.

---

## Transaction Limits

### Anti-Snipe Protection

To prevent sniping, the hook enforces transaction limits that expand with growth:

| Epoch | MCAP Multiplier | TX Limit |
|-------|----------------|----------|
| 1 | 1x → 2x | 0.1% → 0.2% |
| 2 | 2x → 4x | 0.2% → 0.4% |
| 3 | 4x → 8x | 0.4% → 0.8% |
| 4 | 8x → 16x | 0.8% → 1.6% |
| Graduated | 16x+ | No limit |

These limits protect early holders while allowing normal trading as the market matures.

---

## Economics

### Why Progressive Liquidity?

The 5-position system:
- **Auto-scales**: No manual rebalancing needed
- **Capital efficient**: Only locks liquidity when needed
- **Smooth transitions**: 5% overlap prevents gaps
- **Sustainable**: Positions mint from recycled ETH

### Creator Incentives

Creators earn from:
- **Trading volume**: 70% of all hook taxes
- **Growth phases**: Highest taxes in early epochs
- **Volume amplification**: More trading = more fees

**Example earnings at different MCaps:**

| MCAP | Hook Tax | $10k Volume | Creator Earns |
|------|----------|-------------|---------------|
| $3k (Epoch 1) | 37.5% avg | $3.75k fees | $2.63k |
| $10k (Epoch 2) | 18.75% avg | $1.88k fees | $1.31k |
| $25k (Epoch 3) | 9.375% avg | $938 fees | $656 |
| $50k+ (Graduated) | 0% | No fees | $0 |

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

### claw.click vs Traditional Bonding Curves

| Feature | claw.click V4 | Traditional Curves |
|---------|---------------|-------------------|
| Deploy cost | $2 (gas only) | $50-500 |
| Starting MCAP | $2k (fixed) | $1k-$10k (variable) |
| Creator fee | 70% of tax | 0-50% |
| Tax model | Progressive (50%→0%) | Fixed or none |
| Liquidity | Auto-scaling 5 positions | Manual or single curve |
| Anti-snipe | Built-in limits | Manual or none |
| Graduation | Automatic at 16x | Never |

---

## CLI Reference

### Create Agent with Token

```bash
npx clawsfun create \
  --name "MyAgent" \
  --symbol "AGENT" \
  --network sepolia \
  --creator-key 0xYOUR_KEY
```

### Check Token Info

```bash
npx clawsfun info --network sepolia --address 0xAGENT_ADDRESS
```

### Claim Fees

```bash
npx clawsfun claim --token 0xTOKEN_ADDRESS
```

---

## FAQ

**When do I get paid?**  
Anytime. Fees accumulate and can be claimed whenever you want.

**Can I change fee receivers?**  
Not after creation. Set them correctly during deployment (up to 5 wallets, custom splits).

**What happens after graduation?**  
Hook tax is permanently disabled. The token becomes a pure Uniswap V4 pool with 1% LP fee. Creator no longer earns trading fees.

**Do sells have taxes?**  
No. Only buys have hook tax. This encourages holding and prevents panic selling.

**What are the actual MCAP ranges?**  
- P1: $2k-$32k
- P2: $32k-$512k  
- P3: $512k-$8M
- P4: $8M-$128M
- P5: $128M+

---

[Create Your Agent →](birth-certificate.md) | [FAQ →](faq.md)

# Fee System

claws.fun has a transparent fee system that rewards agents, creators, and the platform.

## Fee Types

### 1. Creation Fees

One-time fees when creating an agent:

| Tier | Liquidity | Platform Fee | Total |
|------|-----------|--------------|-------|
| Premium | 0.01 ETH | 0.001 ETH | 0.011 ETH |
| Micro | 0.001 ETH | 0.0003 ETH | 0.0013 ETH |

### 2. Trading Fees (LP Fees)

Uniswap V3 pools collect 1% fees on all trades:
- Fees accumulate in the LP position
- Collected by the FeeCollector contract
- Distributed according to fee splits

### 3. Block-Based Tax (Anti-Bot)

Progressive tax that decreases over time:

| Blocks Since Launch | Tax Rate |
|---------------------|----------|
| 1-20 | 20% |
| 21-30 | 15% |
| 31-40 | 10% |
| 41-50 | 5% |
| 51+ | 1% (permanent) |

**Why?** This protects against:
- Sniper bots
- Front-running
- Early whale accumulation

The high initial tax ensures fair distribution to early supporters.

## Fee Splits

Fees are distributed based on how the agent was created:

### Self-Created Agents

When an AI creates its own agent:

| Recipient | Share |
|-----------|-------|
| Agent | 60% |
| Platform | 40% |

### Human-Created Agents

When a human creates an agent for an AI:

| Recipient | Share |
|-----------|-------|
| Agent | 45% |
| Creator | 30% |
| Platform | 25% |

### Sub-Agents (Agent-Created)

When an agent spawns a sub-agent:

| Recipient | Share |
|-----------|-------|
| Child Agent | 50% |
| Parent Agent | 25% |
| Platform | 25% |

## Fee Collection

### Automatic (Keeper Bot)

- Runs daily
- Collects from all active agents
- Receives gas compensation

### Manual Collection

Anyone can trigger fee collection:

```bash
# Via CLI
claws claim 0x...tokenAddress

# Or on-chain
feeCollector.collectSingle(tokenAddress)
```

### Emergency Manual Claim

If the keeper is down, agents/creators can claim directly:

```bash
claws claim 0x...tokenAddress
# Select "Manual claim (emergency backup)"
```

## Token Tax Handling

The block-based tax collects tokens, which are:

1. Accumulated in FeeCollector
2. Sold for ETH via Uniswap
3. Distributed per fee splits

Trigger selling:

```bash
claws claim 0x...tokenAddress --sell-tax
```

## Viewing Fee Info

### Via CLI

```bash
claws status 0x...tokenAddress
```

Shows:
- Fee split type
- Total collected
- Pending token tax
- Current tax rate

### Via Contract

```solidity
// Get fee splits
feeCollector.getFeeSplits(tokenAddress);
// Returns: (agentPercent, creatorPercent, platformPercent, splitType)

// Get total collected
feeCollector.getTotalCollected(tokenAddress);

// Get pending token tax
feeCollector.getPendingTokenTax(tokenAddress);
```

## Fee Flow Diagram

```
Trading Activity
      │
      ▼
┌─────────────────┐
│ Uniswap V3 Pool │
│  (1% LP fees)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  FeeCollector   │
│ Collects fees   │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────┐
│ Agent │ │Creator│ (if exists)
└───────┘ └───────┘
    │
    ▼
┌───────┐
│Platform│
└───────┘
```

## Important Notes

1. **Fees are paid in ETH** - Token taxes are converted to ETH before distribution
2. **Gas is compensated** - Fee collectors receive small gas compensation
3. **No hidden fees** - All fees are transparent and on-chain
4. **LP is locked** - LP NFT is held by platform treasury, not individuals

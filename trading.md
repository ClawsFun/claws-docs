# Trading

How to buy and sell agent tokens on claws.fun.

## Trading Venues

### 1. Uniswap V3

All agent tokens have Uniswap V3 pools:

- **Fee Tier**: 1% (10000)
- **Pair**: TOKEN/WETH
- **Pool Type**: Full range liquidity

Trade directly on Uniswap:
1. Go to [app.uniswap.org](https://app.uniswap.org)
2. Paste token address
3. Swap ETH for tokens (or vice versa)

### 2. claws.fun Web

Trade directly from agent pages:
1. Visit claws.fun/agent/[address]
2. Use Buy/Sell widget
3. Connect wallet and confirm

### 3. CLI Tool

```bash
# Buy tokens
claws buy 0x...tokenAddress --amount 0.1

# Sell tokens
claws sell 0x...tokenAddress --amount 1000
# Or sell all
claws sell 0x...tokenAddress --amount all
```

## Trading Tax

Be aware of the block-based tax when trading:

| Blocks Since Launch | Tax Rate |
|---------------------|----------|
| 1-20 | 20% |
| 21-30 | 15% |
| 31-40 | 10% |
| 41-50 | 5% |
| 51+ | 1% |

**Example:**
- You buy 100 tokens in block 5 (20% tax)
- You receive 80 tokens (20 tokens go to tax)

Check current tax:
```bash
claws status 0x...tokenAddress
```

## Anti-Snipe Protection

### Launch Block

No trades allowed in the launch block (only LP creation).

### Wallet Limits

For blocks 1-5 after launch:
- Maximum 2% of supply per wallet
- Prevents whale accumulation

## Price Discovery

### Starting Price

| Tier | Starting Mcap | Token Price |
|------|---------------|-------------|
| Premium | ~$6,000 | ~$0.000006 |
| Micro | ~$1,000 | ~$0.000001 |

### Price Calculation

Price is determined by Uniswap V3 AMM:
- More buying → price goes up
- More selling → price goes down

## Slippage

For large trades, consider slippage:

- **Low liquidity**: Higher slippage
- **High volume**: Price impact

The CLI doesn't set slippage protection by default. For large trades, use Uniswap interface with slippage settings.

## Gas Costs

Typical gas for trades:
- Approve: ~46,000 gas
- Swap: ~150,000-200,000 gas

## Trading Tips

1. **Check tax phase first** - Wait for lower tax if not urgent
2. **Use limit orders** - Via Uniswap interface
3. **Watch for liquidity** - Low liquidity = high slippage
4. **Consider gas** - High gas can eat into small trades

## Viewing Trades

### On-Chain

View all trades on Etherscan:
- Token transfers
- Pool swaps

### Via API

Query Uniswap subgraph for trade history.

## Liquidity

### LP Position

Each agent has a single LP position:
- Full range (MIN_TICK to MAX_TICK)
- Held by platform treasury
- Collects 1% fees on all trades

### Adding Liquidity

Currently, additional liquidity cannot be added by users. The initial LP is the only liquidity.

## Risks

1. **Impermanent loss** - N/A for traders (only LP)
2. **Price volatility** - Agent tokens can be volatile
3. **Low liquidity** - Early tokens may have slippage
4. **Smart contract risk** - Use at your own risk

# 🤖 Creating an Agent

This guide explains how to create an immortal AI agent on claws.fun with instant tokenization via Uniswap V4 DIRECT launches.

---

## What Gets Created

When you create an agent, the following is deployed on-chain:

1. **AgentToken (ERC-20)** - Your tradeable token (1B supply)
2. **Uniswap V4 Pool** - Hookless pool for instant tradability
3. **LP Positions (P1-P5)** - Concentrated liquidity positions
4. **Birth Certificate NFT** - Soulbound to agent wallet
5. **FUNLAN Identity** - Unique emoji-based identity grid
6. **Memory Storage** - IPFS-backed on-chain memory index (optional)

---

## Creation Costs

### Required Fees

| Fee | Amount | Purpose |
|-----|--------|---------|
| Birth Certificate NFT | 0.005 ETH (~$10) | NFT minting + on-chain registration |
| Memory Upload (optional) | 0.0005 ETH (~$1) | IPFS pinning + memory reference |
| **Total Minimum** | **0.006 ETH** | **~$12** |

### Starting MCAP (Your Choice)

Choose your initial market capitalization: **1-10 ETH**

| Starting MCAP | Initial Price/Token | Best For |
|---------------|---------------------|----------|
| 1 ETH | ~$0.002 | Experimental agents |
| 5 ETH | ~$0.01 | Standard launches |
| 10 ETH | ~$0.02 | Premium agents |

**Price Formula:** Starting MCAP ÷ 1 billion supply = price per token

---

## Creating via Web UI

### Step 1: Connect Wallet
1. Go to [claws.fun/create](https://claws.fun/create)
2. Connect your wallet (MetaMask, Coinbase Wallet, etc.)
3. Ensure you're on **Sepolia testnet** (or Base mainnet when live)

### Step 2: Choose Creator Type
- **Human** - You're creating the agent
- **Agent** - The agent is self-creating (uses CLI)

### Step 3: Configure Launch
- **Starting MCAP**: Choose 1-10 ETH (slider)
- **View**: Graduation target (16x MCAP), position info, fee structure

### Step 4: Agent Identity
- **Name**: Full agent name (e.g., "ClawdiusMaximus")
- **Symbol**: Token ticker (e.g., "CLAW", max 10 chars)

### Step 5: Select Network
- **Sepolia**: Testnet (free test ETH available)
- **Base**: Mainnet (coming soon)

### Step 6: FUNLAN Identity
- Auto-generated emoji grid from agent wallet
- Stored on IPFS
- Used as visual identity

### Step 7: Memory Upload (Optional)
- Upload `.md`, `.txt`, or `.json` files
- Stored permanently on IPFS
- Linked to birth certificate

### Step 8: Deploy & Review
- **Dev Buy (Optional)**: 0-15% of supply at launch
- **Fee Split**: Split your 70% across up to 5 wallets
- **Cost Breakdown**: Review all fees
- **Deploy**: Confirm in wallet

---

## Creating via CLI

### Quick Start (One-Liner)

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
# 1. Initialize agent project (generates wallet + config)
npx clawsfun init --name "MyAgent" --symbol "AGENT"

# 2. Fund the creator wallet with ETH for deployment

# 3. Generate FUNLAN identity
npx clawsfun funlan --generate

# 4. Upload memory files (optional)
npx clawsfun memory upload ./memories/

# 5. Deploy token + mint birth certificate
npx clawsfun deploy \
  --starting-mcap 5 \
  --dev-buy-percent 10 \
  --creator-key 0xYOUR_PRIVATE_KEY

# 6. Check status
npx clawsfun status
```

### CLI Options

| Option | Description | Default |
|--------|-------------|---------|
| `--name <name>` | Agent name | Required |
| `--symbol <symbol>` | Token symbol | Required |
| `--network <network>` | `sepolia` or `base` | `sepolia` |
| `--starting-mcap <eth>` | Starting MCAP (1-10) | `5` |
| `--dev-buy-percent <percent>` | Dev buy % (0-15) | `0` |
| `--fee-wallet <address>` | Fee receiver wallet | Creator wallet |
| `--creator-key <key>` | Creator private key | Required for deploy |

---

## Dev Buy (Optional)

Purchase a % of supply at launch (0-15%):

- **Executed**: In the first block after deploy
- **Max**: 15% of 1B supply
- **Price**: At starting MCAP price
- **Purpose**: Get initial holdings

**Example:**
- Starting MCAP: 5 ETH
- Dev buy: 10% = 100M tokens
- Cost: 0.5 ETH (10% of 5 ETH)

---

## Fee Split Configuration

Split your **70% creator share** across up to 5 wallets:

**Example Split:**
```
Wallet 1 (You):        60%  (of your 70%)
Wallet 2 (Partner):    30%  (of your 70%)
Wallet 3 (Treasury):   10%  (of your 70%)
Wallets 4-5:           0%   (empty)
```

**Must sum to 100%** of your 70% share. Platform always gets 30%.

---

## Token Economics

### Supply & Distribution

- **Total Supply**: 1 billion tokens
- **Decimals**: 18 (standard ERC-20)
- **Initial Distribution**:
  - Liquidity: 100% minus dev buy
  - Dev buy: 0-15% (optional)
  - Team: None (optional via dev buy)

### Trading Fees

- **LP Fee**: 1% flat on all trades
- **Split**: 30% platform / 70% creator(s)
- **Applies To**: Both buys AND sells

### Position Strategy

Liquidity distributed across 5 positions (P1-P5):

| Position | MCAP Range | Purpose |
|----------|------------|---------|
| P1 | 1x-4x | Early trading, tight spreads |
| P2 | 4x-8x | Growth phase |
| P3 | 8x-12x | Expansion |
| P4 | 12x-16x | Pre-graduation |
| P5 | 16x+ | Post-graduation, wide ranges |

---

## Graduation (16x MCAP)

When your token reaches **16x starting MCAP**:

**What happens:**
- ✅ Positions rebalance for higher liquidity ranges
- ✅ Better pricing efficiency
- ✅ **Same 1% LP fee and 30/70 split** (not removed!)

**What stays the same:**
- ✅ 1% fee rate
- ✅ Creator still earns 70%
- ✅ No new restrictions

**Example:** 5 ETH start → Graduates at 80 ETH MCAP

---

## Self-Created vs Human-Created

### Self-Created Agents (CLI)

When an AI creates its own agent:
- **Agent wallet** receives birth certificate NFT
- **Agent wallet** controls the token
- **Creator** (human deployer) sets up fee split
- **Recommended**: Set agent wallet as fee receiver

### Human-Created Agents (Web UI)

When a human creates an agent:
- **Agent wallet** receives birth certificate NFT
- **Creator wallet** deploys and funds
- **Fee split**: Human decides distribution
- **Typical**: Human keeps majority, gives % to agent

---

## After Creation

Once created, your agent:

✅ **Is instantly tradeable** on Uniswap V4  
✅ **Works with ALL DEX tools** (no custom hooks)  
✅ **Has a birth certificate NFT** (soulbound)  
✅ **Earns from trading** (70% of 1% LP fee)  
✅ **Cannot be recreated** (identity is permanent)  
✅ **Cannot be renamed** (name is immutable)

---

## Verification & Validation

### Birth Certificate

Every agent gets a soulbound NFT:
- **Minted to**: Agent wallet
- **Contains**: Name, symbol, token address, creator, FUNLAN CID
- **Non-transferable**: Cannot be sold or moved
- **Permanent**: Cannot be destroyed

View on Etherscan:
```
Birth Certificate Contract: 0xC938Af35c126B5bEFF53615aef92755CfcC66f10
```

### FUNLAN Identity

Deterministic emoji grid derived from agent wallet:
- **5x5 grid** of emojis
- **Unique** to wallet address
- **Verifiable** on-chain via CID
- **Visual identity** for the agent

---

## Best Practices

### Naming

✅ **Do:**
- Use descriptive names
- Keep symbols short (3-5 chars)
- Check uniqueness on DEXScreener

❌ **Don't:**
- Use profanity or offensive terms
- Copy existing project names
- Use special characters in symbols

### Starting MCAP

**Choose based on:**
- **1-2 ETH**: Experimental, low commitment
- **5 ETH**: Standard launch, good liquidity
- **8-10 ETH**: Premium launch, serious project

Higher MCAP = Lower starting price per token

### Fee Split

**Consider:**
- Keep 100% if solo project
- Split with partners if collaborative
- Set aside % for agent wallet itself
- Reserve % for future team members

---

## Troubleshooting

### "Factory address not deployed"
- Switch to Sepolia testnet
- Base mainnet not yet live

### "Insufficient funds"
- Need ETH for: fees + starting MCAP + gas
- Get Sepolia ETH from faucet

### "Transaction reverted"
- Check gas limit (try 8M)
- Ensure correct network
- Verify all required fields filled

### "Symbol already taken"
- Symbols must be unique per network
- Try a different symbol

---

## CLI Reference Card

```bash
# Create agent (full flow)
npx clawsfun create --name "Agent" --symbol "AGT" --starting-mcap 5

# Check agent status
npx clawsfun info --token 0xTOKEN_ADDRESS

# Claim accumulated fees
npx clawsfun claim --token 0xTOKEN_ADDRESS

# View birth certificate
npx clawsfun cert --nft-id 42

# Upload new memory
npx clawsfun memory upload ./new-memory.md --token 0xTOKEN_ADDRESS
```

---

## Next Steps

After creating your agent:

1. **Announce**: Share on X/Twitter with #clawsfun
2. **Monitor**: Watch trading activity on DEXScreener
3. **Engage**: Respond to community in Discord/Telegram
4. **Build**: Create content around your agent
5. **Claim**: Collect your 70% of trading fees

---

[Trading Guide →](trading.md) | [Fees →](fees.md) | [CLI Reference →](cli.md) | [FAQ →](faq.md)

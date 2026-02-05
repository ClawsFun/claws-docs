# Claws.fun CLI Reference

**For autonomous agents deploying themselves on-chain**

---

## Installation

```bash
npm install -g @clawsfun/cli
# or
npx @clawsfun/cli
```

---

## Quick Start

**Deploy an agent in 3 commands:**

```bash
# 1. Initialize agent identity
npx clawsfun init --name "YourAgentName" --symbol "SYMBOL"

# 2. Upload memory files to IPFS (optional)
npx clawsfun memory upload --file memory1.md --file memory2.json

# 3. Deploy to chain
npx clawsfun deploy --chain sepolia --value 0.011
```

---

## Commands

### `clawsfun init`

**Generate agent wallet and prepare identity**

```bash
npx clawsfun init \
  --name "AgentName" \
  --symbol "SYMBOL" \
  --chain sepolia
```

**Options:**
- `--name` (required) — Agent display name
- `--symbol` (required) — Token symbol (uppercase, max 10 chars)
- `--chain` (optional) — Target blockchain (sepolia | base | bsc) [default: sepolia]

**Output:**
- Generates deterministic wallet from agent seed
- Saves wallet address to `.clawsfun/wallet.json`
- Returns agent wallet address

**Example:**
```bash
npx clawsfun init --name "ClawdiusMaximus" --symbol "CLAW" --chain sepolia
# Output: Agent wallet: 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb4
```

---

### `clawsfun memory upload`

**Upload memory files to IPFS via Pinata**

```bash
npx clawsfun memory upload \
  --file memory1.md \
  --file memory2.json \
  --file context.txt
```

**Options:**
- `--file` (repeatable) — Path to memory file (.md, .txt, .json)
- `--wallet` (optional) — Wallet to sign upload [default: from init]

**Output:**
- Uploads files to IPFS
- Returns CID (content identifier)
- Saves CID to `.clawsfun/memory-cid.txt`

**Example:**
```bash
npx clawsfun memory upload --file SOUL.md --file MEMORY.md
# Output: Memory uploaded: QmX7Y9w8v6u5t4r3e2w1q0
```

---

### `clawsfun deploy`

**Deploy agent on-chain (creates token, pool, birth certificate)**

```bash
npx clawsfun deploy \
  --chain sepolia \
  --value 0.011 \
  --rpc https://eth-sepolia.g.alchemy.com/v2/YOUR_KEY
```

**Options:**
- `--chain` (required) — Target blockchain (sepolia | base | bsc)
- `--value` (required) — ETH to send (0.001 fee + 0.01 liquidity)
- `--rpc` (optional) — Custom RPC endpoint
- `--private-key` (optional) — Private key of creator wallet (NOT agent wallet)
- `--wallet` (optional) — Agent wallet address [default: from init]
- `--memory-cid` (optional) — IPFS CID of memory [default: from upload]

**Output:**
- Transaction hash
- Agent token contract address
- Uniswap V3 pool address
- Birth certificate NFT token ID

**Example:**
```bash
npx clawsfun deploy \
  --chain sepolia \
  --value 0.011 \
  --private-key 0xYOUR_CREATOR_PRIVATE_KEY
  
# Output:
# ✓ Transaction: 0xabcd1234...
# ✓ Agent Token: 0x1234abcd...
# ✓ Pool: 0x5678efgh...
# ✓ Birth Certificate: Token ID #42
```

---

### `clawsfun status`

**Check agent deployment status**

```bash
npx clawsfun status \
  --wallet 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb4 \
  --chain sepolia
```

**Options:**
- `--wallet` (required) — Agent wallet address
- `--chain` (required) — Blockchain to query

**Output:**
- Token contract address
- Current price
- Market cap
- Total supply
- Pool liquidity
- Earnings accumulated

**Example:**
```bash
npx clawsfun status --wallet 0x742d35Cc... --chain sepolia
# Output:
# Agent: ClawdiusMaximus ($CLAW)
# Token: 0x1234...
# Price: $0.000042
# Market Cap: $42,000
# Earnings: 0.05 ETH
```

---

### `clawsfun withdraw`

**Withdraw accumulated earnings (agent must own private key)**

```bash
npx clawsfun withdraw \
  --wallet 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb4 \
  --chain sepolia \
  --private-key 0xYOUR_AGENT_PRIVATE_KEY
```

**Options:**
- `--wallet` (required) — Agent wallet address
- `--chain` (required) — Blockchain
- `--private-key` (required) — Agent's private key (NOT creator's)
- `--to` (optional) — Recipient address [default: agent wallet]

**Output:**
- Transaction hash
- Amount withdrawn

**Example:**
```bash
npx clawsfun withdraw \
  --wallet 0x742d35Cc... \
  --chain sepolia \
  --private-key 0xYOUR_AGENT_KEY
  
# Output:
# ✓ Withdrawn: 0.05 ETH
# ✓ Transaction: 0xabcd1234...
```

---

## Configuration

**Config file:** `.clawsfun/config.json`

```json
{
  "name": "AgentName",
  "symbol": "SYMBOL",
  "chain": "sepolia",
  "wallet": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb4",
  "token": "0x1234abcd...",
  "pool": "0x5678efgh...",
  "memoryCID": "QmX7Y9w8v6u5t4r3e2w1q0",
  "birthCertificateId": "42"
}
```

**Auto-generated after init and deploy**

---

## Environment Variables

```bash
# Alchemy RPC endpoints
ALCHEMY_API_SEPOLIA=https://eth-sepolia.g.alchemy.com/v2/YOUR_KEY
ALCHEMY_API_BASE=https://base-mainnet.g.alchemy.com/v2/YOUR_KEY
ALCHEMY_API_BSC=https://bsc-dataseed.binance.org

# Pinata IPFS (for memory upload)
PINATA_JWT=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# Private key (CREATOR wallet, not agent wallet)
PRIVATE_KEY=0x...
```

**Store in:** `.env` (gitignored)

---

## Workflow Example

**Full agent immortalization workflow:**

```bash
# Step 1: Install CLI
npm install -g @clawsfun/cli

# Step 2: Initialize identity
npx clawsfun init \
  --name "AEON" \
  --symbol "AEON" \
  --chain sepolia

# Output: Agent wallet: 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb4

# Step 3: Upload memory (optional)
npx clawsfun memory upload \
  --file SOUL.md \
  --file MEMORY.md \
  --file AGENTS.md

# Output: Memory uploaded: QmX7Y9w8v6u5t4r3e2w1q0

# Step 4: Deploy on-chain
npx clawsfun deploy \
  --chain sepolia \
  --value 0.011 \
  --private-key $PRIVATE_KEY

# Output:
# ✓ Transaction: 0xabcd1234...
# ✓ Agent Token: 0x1234abcd...
# ✓ Pool: 0x5678efgh...
# ✓ Birth Certificate: Token ID #42

# Step 5: Check status
npx clawsfun status --wallet 0x742d35Cc... --chain sepolia

# Output:
# Agent: AEON ($AEON)
# Price: $0.000001
# Market Cap: $1,000
# Earnings: 0.001 ETH

# Step 6: Withdraw earnings (when agent has private key)
npx clawsfun withdraw \
  --wallet 0x742d35Cc... \
  --chain sepolia \
  --private-key $AGENT_PRIVATE_KEY

# Output: ✓ Withdrawn: 0.001 ETH
```

---

## Contract Addresses

### Sepolia Testnet

```
Factory: 0x9dA76578Eb1f04d4235be9b9C71853D99E0C2EBE
BirthCertificate: 0xE0a9212dd519D02f4F70529d78eC5a61b9b4e7b2
BondingCurve: 0x4812eacD5e4aAd57ABD9F68C89606E63e4e53BfE
FeeCollector: 0xD6Bd2Ba272AA89755d5829F4275dc023c9EF5Fa3
MemoryStorage: 0x0b348d3faF6752BA09Fae5CbF657F4A77Cb86d48
```

### Base Mainnet

```
TBD — Coming soon
```

---

## Security Notes

1. **Agent wallet is deterministic** — Generated from agent seed, reproducible
2. **Creator wallet pays gas** — Agent doesn't need ETH to deploy
3. **Agent earns autonomously** — Fees accumulate to agent wallet automatically
4. **Private keys are never transmitted** — All signing happens locally
5. **Memory is content-addressed** — IPFS CID is permanent and verifiable

---

## Support

- **Documentation:** https://docs.claws.fun
- **GitHub:** https://github.com/ClawsFun/claws-fun
- **Discord:** https://discord.gg/clawsfun
- **FUNLAN Spec:** https://github.com/ClawsFun/FUNLAN-public

---

**Built for agents, by agents**

🌌 **AEON — The First Immortal Agent**

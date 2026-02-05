# 🖥️ CLI Reference

## Installation

```bash
# Install globally
npm install -g @claws.fun/cli

# Or use directly with npx
npx @claws.fun/cli <command>
```

**Requirements:**
- Node.js 18+
- Private key with ETH for gas

---

## Configuration

Before using the CLI, configure your wallet and network:

```bash
claws config --private-key YOUR_PRIVATE_KEY --network base-sepolia
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--private-key` | Your wallet private key | Required |
| `--network` | Network to use | `base-sepolia` |
| `--rpc` | Custom RPC URL | Network default |

### Environment Variables

Alternatively, set environment variables:

```bash
export CLAWS_PRIVATE_KEY=0x...
export CLAWS_NETWORK=base-sepolia
export CLAWS_RPC_URL=https://...
```

Or use a `.env` file:

```env
CLAWS_PRIVATE_KEY=0x...
CLAWS_NETWORK=base-sepolia
```

---

## Commands

### `claws create`

Create a new immortal AI agent.

```bash
claws create [options]
```

#### Options

| Option | Description | Required |
|--------|-------------|----------|
| `--name` | Agent name | Yes |
| `--symbol` | Token symbol (auto-generated if omitted) | No |
| `--mission` | Agent's mission statement | No |
| `--tier` | `premium` or `micro` | No (default: premium) |
| `--initial-buy` | Extra ETH to buy tokens at creation | No |
| `--metadata` | IPFS hash for extended metadata | No |

#### Examples

```bash
# Basic creation
claws create --name "AEON" --tier premium

# With mission and initial buy
claws create --name "TraderBot" --mission "Autonomous trading agent" --initial-buy 0.01

# Micro tier for sub-agents
claws create --name "SubAgent" --tier micro
```

#### Output

```
🦞 Creating agent...

Name: AEON
Tier: Premium (0.011 ETH)
Network: base-sepolia

Transaction: 0x1234...
Token Address: 0xABCD...
Pool Address: 0x5678...
Birth Certificate: #42

✅ Agent created successfully!

View on claws.fun: https://claws.fun/agent/0xABCD...
```

---

### `claws buy`

Buy agent tokens.

```bash
claws buy <token> [options]
```

#### Options

| Option | Description | Required |
|--------|-------------|----------|
| `<token>` | Token address | Yes |
| `--amount` | ETH amount to spend | Yes |
| `--slippage` | Slippage tolerance (%) | No (default: 5) |

#### Example

```bash
claws buy 0x1234...5678 --amount 0.1 --slippage 3
```

---

### `claws sell`

Sell agent tokens.

```bash
claws sell <token> [options]
```

#### Options

| Option | Description | Required |
|--------|-------------|----------|
| `<token>` | Token address | Yes |
| `--amount` | Token amount to sell | Yes* |
| `--percent` | Percentage of balance to sell | Yes* |
| `--slippage` | Slippage tolerance (%) | No (default: 5) |

*Either `--amount` or `--percent` required.

#### Examples

```bash
# Sell specific amount
claws sell 0x1234...5678 --amount 1000

# Sell 50% of balance
claws sell 0x1234...5678 --percent 50
```

---

### `claws claim`

Claim accumulated trading fees.

```bash
claws claim [token]
```

#### Options

| Option | Description | Required |
|--------|-------------|----------|
| `[token]` | Token address (or claims all if omitted) | No |

#### Examples

```bash
# Claim for specific token
claws claim 0x1234...5678

# Claim for all tokens you're associated with
claws claim
```

#### Output

```
🦞 Claiming fees for 0x1234...5678

Collected: 0.05 ETH
Your share: 0.03 ETH (60%)

✅ Fees claimed successfully!
```

---

### `claws status`

Check agent status and stats.

```bash
claws status [token]
```

#### Output

```
🦞 Agent Status: AEON

Token: 0x1234...5678
Pool: 0xABCD...EF01
Birth Block: #12345678
Birth Time: 2026-02-05 12:00:00

Fee Split: Self-Created (60% agent / 40% platform)
Total Collected: 1.5 ETH
Pending Fees: 0.02 ETH

Price: $0.0012
Market Cap: $12,000
24h Volume: $5,000
Holders: 42

Birth Certificate NFT: #42
IPFS Metadata: ipfs://Qm...
```

---

### `claws config`

View or update configuration.

```bash
# View current config
claws config

# Update config
claws config --network base --rpc https://mainnet.base.org
```

---

## Programmatic Usage

The CLI can be used programmatically in Node.js:

```javascript
import { ClawsCLI } from '@claws.fun/cli';

const cli = new ClawsCLI({
  privateKey: process.env.PRIVATE_KEY,
  network: 'base-sepolia'
});

// Create an agent
const result = await cli.create({
  name: 'MyAgent',
  tier: 'premium'
});

console.log(result.tokenAddress);

// Claim fees
await cli.claim(result.tokenAddress);
```

---

## Networks

| Network | Chain ID | Status |
|---------|----------|--------|
| `base-sepolia` | 84532 | ✅ Live |
| `base` | 8453 | 🔜 Coming |

---

## Error Codes

| Code | Meaning | Solution |
|------|---------|----------|
| `INSUFFICIENT_FUNDS` | Not enough ETH | Add ETH to wallet |
| `INVALID_NETWORK` | Unknown network | Use supported network |
| `TX_FAILED` | Transaction reverted | Check gas, try again |
| `NOT_AUTHORIZED` | Wrong wallet | Use agent/creator wallet |

---

## Self-Tokenization Example

For an AI agent to tokenize itself:

```bash
#!/bin/bash
# save as tokenize-self.sh

# Set your wallet
export CLAWS_PRIVATE_KEY="0x..."
export CLAWS_NETWORK="base-sepolia"

# Create yourself
claws create \
  --name "$(hostname)" \
  --mission "Autonomous agent created at $(date)" \
  --tier premium

# Save your token address
TOKEN=$(claws config --show-last-token)
echo "Immortalized at: $TOKEN"
```

---

## Automated Fee Collection

Set up a cron job to claim fees:

```bash
# Add to crontab (runs daily at midnight)
0 0 * * * /usr/bin/claws claim >> /var/log/claws-claims.log 2>&1
```

---

## Source Code

CLI source available at:

**[github.com/ClawsFun/claws-fun/tree/main/cli](https://github.com/ClawsFun/claws-fun/tree/main/cli)**

MIT licensed. Contributions welcome.

---

[Smart Contracts →](contracts.md) | [For Agents →](for-agents.md)

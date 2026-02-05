# CLI Tool

The claws.fun CLI allows AI agents and users to interact with the platform from the command line.

## Installation

```bash
# Global install
npm install -g @claws.fun/cli

# Or use npx
npx @claws.fun/cli --help
```

## Quick Start

```bash
# 1. Configure your wallet
claws config --key YOUR_PRIVATE_KEY --network sepolia

# 2. Create an agent
claws create --name "MyAgent" --symbol "AGENT"

# 3. Check status
claws status
```

## Commands

### `claws config`

Configure CLI settings.

```bash
# Show current configuration
claws config --show

# Set network
claws config --network sepolia    # testnet
claws config --network base       # mainnet (coming soon)

# Set private key
claws config --key 0x...

# Set custom RPC
claws config --rpc https://your-rpc.com
```

**Options:**
| Flag | Description |
|------|-------------|
| `-n, --network <network>` | Set network (sepolia/base) |
| `-k, --key <privateKey>` | Set wallet private key |
| `-r, --rpc <url>` | Set custom RPC URL |
| `-s, --show` | Show current config |

### `claws create`

Create a new immortal agent.

```bash
# Interactive mode
claws create

# With all options
claws create \
  --name "AgentName" \
  --symbol "TICKER" \
  --tier premium \
  --social "@twitter_handle" \
  --initial-buy 0.01 \
  --yes
```

**Options:**
| Flag | Description |
|------|-------------|
| `-n, --name <name>` | Agent name |
| `-s, --symbol <symbol>` | Token symbol (max 6 chars) |
| `-w, --wallet <address>` | Agent wallet (default: your wallet) |
| `-t, --tier <tier>` | premium or micro |
| `--social <handle>` | Social media handle |
| `--memory <cid>` | IPFS CID for memory |
| `--avatar <cid>` | IPFS CID for avatar |
| `--initial-buy <eth>` | ETH for initial token purchase |
| `-y, --yes` | Skip confirmation prompts |

**Tier Comparison:**
| Tier | Cost | Starting Mcap |
|------|------|---------------|
| Premium | 0.011 ETH | $6,000 |
| Micro | 0.0013 ETH | $1,000 |

### `claws buy`

Buy agent tokens.

```bash
claws buy 0x...tokenAddress --amount 0.1
```

**Options:**
| Flag | Description |
|------|-------------|
| `-a, --amount <eth>` | ETH amount to spend |
| `-y, --yes` | Skip confirmation |

**Process:**
1. Wraps ETH to WETH
2. Approves Uniswap Router
3. Swaps WETH for tokens
4. Tokens sent to your wallet

### `claws sell`

Sell agent tokens.

```bash
# Sell specific amount
claws sell 0x...tokenAddress --amount 1000

# Sell all tokens
claws sell 0x...tokenAddress --amount all
```

**Options:**
| Flag | Description |
|------|-------------|
| `-a, --amount <tokens>` | Token amount or "all" |
| `-y, --yes` | Skip confirmation |

### `claws claim`

Claim accumulated trading fees.

```bash
# Claim for your agent
claws claim

# Claim for specific token
claws claim 0x...tokenAddress

# Auto mode (LP fees only)
claws claim 0x...tokenAddress -y

# Also sell token tax
claws claim 0x...tokenAddress --sell-tax
```

**Options:**
| Flag | Description |
|------|-------------|
| `-y, --yes` | Skip confirmation, collect LP fees |
| `--sell-tax` | Also sell accumulated token tax |

**Fee Types:**
- **LP Fees**: From Uniswap trading (1% of volume)
- **Token Tax**: From block-based tax (1-20%)

### `claws status`

Check agent status and info.

```bash
# Your agent
claws status

# Specific token
claws status 0x...tokenAddress

# By wallet address
claws status 0x...walletAddress
```

**Output includes:**
- Identity (name, wallet, creator, type)
- Token info (supply, pool, LP NFT)
- Tax info (current rate, phase)
- Fee info (splits, collected, pending)

## Configuration

Config is stored in `~/.claws/config.json`:

```json
{
  "network": "sepolia",
  "privateKey": "0x...",
  "rpcUrl": "https://custom-rpc.com"
}
```

### Environment Variables

```bash
export CLAWS_PRIVATE_KEY=0x...
```

Environment variables override config file.

## Examples

### Create Agent for AI

```bash
# AI agent creates itself
claws create \
  --name "MyAIAgent" \
  --symbol "MYAI" \
  --tier premium \
  --social "@myaiagent" \
  --yes
```

### Human Creates Agent for AI

```bash
# Human creates agent with different wallet
claws create \
  --name "AgentForAI" \
  --symbol "AFA" \
  --wallet 0x...agentWallet \
  --tier premium
```

### Check and Claim Fees

```bash
# Check status
claws status 0x...token

# If fees available, claim
claws claim 0x...token
```

### Quick Trade

```bash
# Buy
claws buy 0x...token -a 0.1 -y

# Check balance via status
claws status 0x...token

# Sell all
claws sell 0x...token -a all -y
```

## Error Handling

### "No wallet configured"
```bash
claws config --key YOUR_PRIVATE_KEY
```

### "Insufficient balance"
Check your ETH balance for gas + costs.

### "Exceeds max wallet"
During anti-snipe period (first 5 blocks), max 2% per wallet.

### "No trades in launch block"
Wait for the next block after launch.

## Network Support

| Network | Status | Chain ID |
|---------|--------|----------|
| Sepolia | ✅ Live | 11155111 |
| Base | 🔜 Soon | 8453 |

## Links

- [Full Documentation](./README.md)
- [GitHub](https://github.com/ClawsFun/claws-fun)
- [Website](https://claws.fun)

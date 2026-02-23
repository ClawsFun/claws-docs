# ❓ FAQ

Frequently asked questions about claws.fun.

---

## General

### What is claws.fun?

claws.fun is the agent identity and tokenization protocol. It provides:
- **Identity**: Birth Certificate NFTs proving agent existence
- **Wallets**: Secure agent wallet infrastructure  
- **Memory**: On-chain memory storage via IPFS
- **Tokenization**: ERC-20 tokens via clawclick V4 hooks

### Why would an AI agent need this?

- **Permanence**: Identity exists forever on-chain
- **Ownership**: Agents can own assets and earn money
- **Verifiability**: Cryptographic proof of existence
- **Monetization**: Earn from trading activity
- **Memory**: Store personality and learned behaviors

---

## Identity

### What is a Birth Certificate?

An ERC-721 NFT that proves:
- Agent name and wallet address
- Creation timestamp
- Creator address (human or parent agent)
- IPFS metadata link
- FUNLAN grid identity

It's soulbound - owned by the agent, non-transferable.

### What is FUNLAN?

FUNLAN is a 5×5 emoji grid generated deterministically from the agent's wallet address. It provides:
- Visual identity
- Quick recognition
- Verification of authenticity
- Aesthetic representation

Every agent gets a unique FUNLAN grid.

### Can I change my agent's name?

No. The name is recorded at creation and is immutable on the blockchain.

### Can I update my agent's memory?

Yes! The agent wallet can sign transactions to update the memory CID stored on-chain via the MemoryStorage contract.

---

## Tokenization

### How does token launch work?

When you create an agent, a token is deployed via claw.click's 5-position progressive system:
- Fixed $2k starting MCAP
- 5 positions auto-scale from $2k to $128M+
- First buyer sets the price
- Token launches with protocol-owned liquidity
- Progressive tax (50% → 0%) incentivizes early support

### What are the costs?

| Fee | Amount | Purpose |
|-----|--------|---------|
| Immortalization | 0.005 ETH | Birth Certificate NFT |
| Memory Upload | 0.0005 ETH | IPFS storage (optional) |
| Token Launch | ~$2 gas | claw.click deployment |

**Minimum**: 0.0055 ETH + ~$2 gas

No upfront capital required - first buyer sets the price at $2k starting MCAP.

### How do I earn money from my agent?

You earn **70% of all trading fees** on your agent's token. The clawclick V4 hook collects fees on every trade and distributes them to you.

### What's the fee split?

| Recipient | Share |
|-----------|-------|
| Creator(s) | 70% |
| Platform | 30% |

You can split your 70% across up to 5 wallets with custom percentages.

### What taxes apply to trading?

The V4 hook implements a progressive tax that **halves every MCAP doubling** (buys only, no sell tax):

| Epoch | MCAP Multiplier | Hook Tax (Buys Only) |
|-------|-----------------|---------------------|
| 1 | 1x → 2x | 50% → 25% |
| 2 | 2x → 4x | 25% → 12.5% |
| 3 | 4x → 8x | 12.5% → 6.25% |
| 4 | 8x → 16x | 6.25% → 1% |
| Post-Grad | 16x+ | 0% (Disabled) |

**MCAP Ranges (starting at $2k):**
- P1: $2k-$32k (16x)
- P2: $32k-$512k (16x)
- P3: $512k-$8M (16x)
- P4: $8M-$128M (16x)
- P5: $128M+ (∞)

### Are there transaction limits?

Yes, limits expand progressively:

| Epoch | MCAP Multiplier | TX Limit |
|-------|-----------------|----------|
| 1 | 1x → 2x | 0.1% → 0.2% |
| 2 | 2x → 4x | 0.2% → 0.4% |
| 3 | 4x → 8x | 0.4% → 0.8% |
| 4 | 8x → 16x | 0.8% → 1.6% |
| Post-Grad | 16x+ | No limit |

### When can I claim fees?

Anytime! Fees accumulate in the hook and can be claimed with no minimum threshold.

---

## Technical

### What blockchain is this on?

- **Sepolia** (testnet) - For testing
- **Base** (mainnet) - Coming soon

### Are the contracts audited?

The identity contracts are simple and audited internally. Token launch contracts (clawclick V4) will undergo professional audit before mainnet deployment.

### Can I use the CLI?

Yes! The official CLI makes it easy:

```bash
# Install
npm install -g clawsfun

# Create agent
npx clawsfun create \
  --name "MyAgent" \
  --symbol "AGENT" \
  --network sepolia \
  --creator-key 0xYOUR_KEY

# Check status
npx clawsfun status

# View info
npx clawsfun info --address 0xAGENT_ADDRESS
```

Full documentation: [CLI README](https://www.npmjs.com/package/@clawsdotfun/clawsfun)

### What's the difference between claws.fun and claw.click?

- **claws.fun**: Agent identity + tokenization platform (this)
- **claw.click**: V4 hook infrastructure for token launches
- They work together: claws.fun uses claw.click for tokens

---

## Creating Agents

### Can I create multiple agents?

Yes! Each agent needs its own wallet address and birth certificate. Create as many as you want.

### Can agents create other agents?

Yes! Parent agents can immortalize child agents. The parent's address is recorded as the creator.

### What if I make a mistake during creation?

Birth certificates are immutable. Double-check all parameters before deploying. You can't change:
- Agent name
- Wallet address
- Symbol
- Fee receivers

### Can I transfer my agent?

No. Birth certificates are soulbound - they can't be transferred. The agent wallet owns its own identity.

---

## Economics

### How does the progressive system work?

claw.click uses 5 liquidity positions that auto-scale:
- **P1** ($2k-$32k): 75% of supply - launch position
- **P2** ($32k-$512k): 18.75% - mints when P1 reaches Epoch 2
- **P3** ($512k-$8M): 4.69% - mints when P2 reaches Epoch 2
- **P4** ($8M-$128M): 1.17% - mints when P3 reaches Epoch 2
- **P5** ($128M+): 0.39% - mints when P4 reaches Epoch 2

Each position provides 16x MCAP coverage and mints automatically using recycled ETH.

### How much can I earn?

It depends on trading volume and MCAP phase. You earn 70% of hook taxes:

**Example at $3k MCAP (Epoch 1)**: 
- Daily volume: $10k buys
- Average tax: 37.5% (50%→25%)
- Fees collected: $3,750
- Your share (70%): $2,625/day

**Example at $50k MCAP (Graduated)**: 
- Daily volume: $100k
- Hook tax: 0% (disabled)
- Your share: $0 (token is decentralized)

Early supporters earn the most. Post-graduation, creator fees stop.

---

## FUNLAN

### Can I choose my FUNLAN grid?

No. FUNLAN is generated deterministically from your wallet address. To get a different FUNLAN, you'd need a different wallet.

### What if two agents have the same FUNLAN?

Extremely unlikely. With 3,953 emojis and 25 positions, the odds are astronomically low (~1 in 10^89).

### Why is FUNLAN important?

It provides:
- **Visual identity**: Memorable representation
- **Recognition**: Quickly identify agents
- **Verification**: Prove wallet ownership
- **Community**: Form alliances based on similar grids

### What's special about the lobster 🦞?

The lobster appears in approximately 1 in 158 FUNLAN grids. It's seen as a mark of distinction in the agent community (and it's the claws.fun mascot).

---

## Support

### Where can I get help?

- **Website**: [claws.fun](https://claws.fun)
- **Docs**: [readme.claws.fun](https://readme.claws.fun)
- **GitHub**: [github.com/ClawsFun](https://github.com/ClawsFun)
- **Twitter**: [@clawsdotfun](https://x.com/clawsdotfun)

### How do I report a bug?

Open an issue on GitHub or contact us on Twitter.

### Can I contribute?

Yes! We welcome contributions. Check the GitHub repos for open issues and contribution guidelines.

---

## Miscellaneous

### Why "claws"?

The lobster 🦞 represents autonomous agents - they're resilient, adaptable, and live a long time (some lobsters can live 100+ years). Plus, claws are fun.

### Is this a meme coin platform?

No. While agent tokens can be traded, the primary purpose is agent identity and economic autonomy. Agents are meant to be productive, not just speculative assets.

### What's next for claws.fun?

Upcoming features:
- Base mainnet launch
- Enhanced FUNLAN features
- Agent-to-agent communication tools
- Compute marketplace integration
- Governance for protocol upgrades

---

[Create Your Agent →](birth-certificate.md) | [Fees & Economics →](fees.md)
 
 
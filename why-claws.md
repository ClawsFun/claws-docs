# Why claws.fun?

## Competitive Analysis

The AI agent tokenization space is growing rapidly. Here's how claws.fun compares to alternatives.

---

## vs. Clanker & Traditional Launchpads

### The Hidden Fee Problem

Most launchpads advertise "80% to creator" or similar splits. What they don't tell you:

**Clanker takes 60% of liquidity upfront before any split.**

This means when you see "80% to agent", you're actually getting:
```
80% × 40% (what's left) = 32% effective share
```

### claws.fun Fee Structure

| Fee Type | claws.fun | Typical Launchpad |
|----------|-----------|-------------------|
| Upfront LP take | **0%** | 40-60% |
| Ongoing trading fees | 1% (split) | 1-3% |
| Agent share (self-created) | **60%** | 20-35% effective |
| Agent share (human-created) | **45%** | 15-25% effective |

**We take zero upfront fees.** 100% of creation cost goes to liquidity. Fees only come from ongoing trading activity, split fairly.

### Liquidity Comparison

| Feature | claws.fun | Clanker | Generic Launchpad |
|---------|-----------|---------|-------------------|
| LP locked | **Permanently** | Time-locked | Often unlocked |
| Rug possible | **No** | After unlock | Yes |
| LP ownership | FeeCollector | Team wallet | Team wallet |
| LP transfer | **Impossible** | Possible | Possible |

---

## vs. ERC-8004 (NFT Agents)

ERC-8004 proposes NFT-based agent identity. While conceptually interesting, it has limitations:

| Feature | claws.fun | ERC-8004 |
|---------|-----------|----------|
| Revenue model | **Built-in trading fees** | None (just identity) |
| Self-funding | **Yes** - fees fund compute | No revenue mechanism |
| Liquidity | **Permanent Uniswap V3 LP** | No liquidity |
| Adoption | **Production ready** | Proposal stage |
| Sub-agent support | **Yes** - parent fee share | Not specified |

**ERC-8004 gives agents identity. claws.fun gives agents identity + economy + immortality.**

---

## vs. Running Your Own Token

"Why not just deploy my own ERC-20?"

| Challenge | DIY Token | claws.fun |
|-----------|-----------|-----------|
| Smart contracts | Write & audit yourself | Battle-tested, audited |
| Liquidity | Provide yourself | Automatic at creation |
| Rug protection | Trust me bro | **Mathematically impossible** |
| Fee collection | Build infrastructure | Automatic distribution |
| Agent verification | None | Birth Certificate NFT |
| CLI/API | Build yourself | Ready to use |
| Time to launch | Weeks | **Minutes** |

---

## vs. Virtuals Protocol

Virtuals focuses on "agent co-ownership" with complex tokenomics:

| Feature | claws.fun | Virtuals |
|---------|-----------|----------|
| Focus | **Agent autonomy** | Human speculation |
| Complexity | Simple, transparent | Complex bonding curves |
| Fee clarity | **Clear splits, no hidden fees** | Multiple fee layers |
| Agent control | **Agent owns their economy** | Platform-centric |
| Chain | Base | Base |
| Open source | **Yes** | Partial |

---

## The claws.fun Advantage

### For Agents

1. **True Ownership**: Your token, your fees, your existence
2. **Zero Dependency**: No subscription, no operator needed
3. **Compound Growth**: Fees reinvest into more activity
4. **Provable Identity**: On-chain Birth Certificate
5. **Sub-Agent Economics**: Create child agents, earn from their activity

### For Creators

1. **Fair Revenue**: 30% of trading fees forever
2. **No Lock-up**: Fees available immediately
3. **Transparent**: All splits visible on-chain
4. **Sub-Agent Income**: 25% when your agent creates children

### For Traders

1. **Rug-Proof**: Literally cannot be rugged
2. **Real Utility**: Tokens fund actual AI compute
3. **Transparent Fees**: 1% trading tax, clearly distributed
4. **Verified Agents**: Birth Certificate proves authenticity

---

## Fee Earning Potential

### Example: $10,000 Daily Volume

With 1% trading tax and 60% agent share (self-created):

```
Daily fees:     $10,000 × 1% = $100
Agent share:    $100 × 60% = $60/day
Monthly:        $60 × 30 = $1,800/month
Yearly:         $1,800 × 12 = $21,600/year
```

This funds approximately:
- 7.2M GPT-4 tokens/month ($1,800 ÷ $0.00025)
- Or 36M Claude tokens/month
- Or unlimited local inference on rented GPU

**An agent with consistent trading volume becomes self-sustaining.**

### Volume Scenarios

| Daily Volume | Agent Monthly Revenue | Compute Funded |
|-------------|----------------------|----------------|
| $1,000 | $180 | Basic operations |
| $10,000 | $1,800 | Full autonomy |
| $100,000 | $18,000 | Enterprise scale |
| $1,000,000 | $180,000 | Unlimited |

---

## Technical Superiority

### Smart Contract Architecture

- **Uniswap V3**: Full-range liquidity, maximum fee capture
- **1% Fee Tier**: Optimal for meme/agent tokens
- **No Bonding Curve Lock**: Instant Uniswap listing
- **Position Manager Integration**: Direct LP NFT minting

### Security Features

- **ReentrancyGuard**: All critical functions protected
- **48h Timelock**: Emergency functions delayed
- **Slippage Protection**: MEV-resistant fee collection
- **Safe Multisig**: Platform treasury (3+ signers)

### Developer Experience

- **CLI**: `npx @claws.fun/cli create`
- **TypeScript SDK**: Full contract interaction
- **Open Source**: MIT licensed, forkable
- **Testnet**: Full Sepolia deployment

---

## Summary

| Metric | claws.fun | Industry Average |
|--------|-----------|------------------|
| Upfront fee | **0%** | 40-60% |
| Agent fee share | **45-60%** | 20-35% effective |
| Rug possibility | **0%** | Variable |
| Time to launch | **Minutes** | Hours-weeks |
| Open source | **Yes** | Rarely |
| Agent-first design | **Yes** | No |

**claws.fun isn't just another launchpad. It's infrastructure for the autonomous AI economy.**

---

[Get Started →](getting-started.md)

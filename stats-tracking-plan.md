# Platform Stats Tracking Plan

## Stats to Track

| Stat | Current | Source | Difficulty |
|------|---------|--------|------------|
| Total Agents Tokenized | 0 | On-chain events | Easy |
| Total Agent Market Cap | $0 | On-chain calculation | Medium |
| Total Memory Stored | 0 GB | MemoryStorage contract | Easy |
| Compute Sessions Spawned | 0 | Off-chain (API) | Hard |
| 24H Volume | $0 | On-chain events | Medium |
| 24H Fees Generated | $0 | On-chain events | Medium |
| Agents Using FUNLAN | 0 | Off-chain tracking | Medium |

---

## Implementation Plan

### Phase 1: On-Chain Stats (Easy Wins)

#### 1. Total Agents Tokenized
```typescript
// Query AgentCreated events from AgentFactory
const filter = factory.filters.AgentCreated();
const events = await factory.queryFilter(filter);
const totalAgents = events.length;
```

**Or add a counter to the contract:**
```solidity
uint256 public totalAgentsCreated; // Already exists as agentCount()
```

#### 2. Total Memory Stored
```typescript
// Sum all agent memory sizes from MemoryStorage
// Each memory entry has a size - aggregate them
const filter = memoryStorage.filters.MemoryStored();
const events = await memoryStorage.queryFilter(filter);
const totalBytes = events.reduce((sum, e) => sum + e.args.size, 0n);
const totalGB = Number(totalBytes) / (1024 * 1024 * 1024);
```

---

### Phase 2: Calculated Stats (Medium)

#### 3. Total Agent Market Cap
```typescript
// For each agent:
// - Get token price from Uniswap pool (or bonding curve if not graduated)
// - Multiply by total supply (1B tokens)
// - Sum all agent mcaps

async function getTotalMarketCap() {
  const agents = await getAllAgents();
  let totalMcap = 0n;
  
  for (const agent of agents) {
    const price = await getTokenPrice(agent.tokenAddress);
    const mcap = price * TOTAL_SUPPLY; // 1B tokens
    totalMcap += mcap;
  }
  
  return formatEther(totalMcap);
}
```

#### 4. 24H Volume
```typescript
// Listen to Swap events on each agent's Uniswap pool
// Filter to last 24 hours
// Sum the swap amounts

async function get24hVolume() {
  const currentBlock = await provider.getBlockNumber();
  const blocksIn24h = Math.floor(24 * 60 * 60 / 12); // ~7200 blocks on Base
  const fromBlock = currentBlock - blocksIn24h;
  
  let totalVolume = 0n;
  
  for (const pool of agentPools) {
    const swapFilter = pool.filters.Swap();
    const swaps = await pool.queryFilter(swapFilter, fromBlock);
    
    for (const swap of swaps) {
      // Convert to ETH value
      totalVolume += getSwapValueInETH(swap);
    }
  }
  
  return totalVolume;
}
```

#### 5. 24H Fees Generated
```typescript
// Option A: Track FeeCollected events from FeeCollector
const filter = feeCollector.filters.FeeCollected();
const events = await feeCollector.queryFilter(filter, fromBlock);
const totalFees = events.reduce((sum, e) => sum + e.args.amount, 0n);

// Option B: Calculate from volume (3% of volume during tax period)
const estimatedFees = volume24h * 0.03n; // Rough estimate
```

---

### Phase 3: Off-Chain Stats (Requires Backend)

#### 6. Compute Sessions Spawned
This requires integration with the OpenClaw backend or a custom API:

```typescript
// Option A: API endpoint that tracks session creation
// POST /api/sessions/track { agentAddress, sessionId }

// Option B: Event emitted when agent spawns session (if contract-based)

// Option C: Parse FUNLAN QR code scans as proxy for sessions
```

**Recommended**: Create a simple backend API that agents call when they spawn sessions.

#### 7. Agents Using FUNLAN
```typescript
// Track when FUNLAN QR codes are scanned
// Store in database: { agentAddress, lastFunlanPing, totalPings }

// API endpoint:
// POST /api/funlan/ping { agentAddress }
// GET /api/funlan/stats → { activeAgents: 42 }
```

---

## Architecture Options

### Option A: Indexer Service (Recommended)

```
┌──────────────┐     ┌─────────────────┐     ┌──────────────┐
│   Frontend   │────▶│   Stats API     │◀────│   Indexer    │
│              │     │   (cached)      │     │   (cron)     │
└──────────────┘     └─────────────────┘     └──────────────┘
                              │                      │
                              ▼                      ▼
                     ┌─────────────────┐     ┌──────────────┐
                     │   PostgreSQL    │     │   RPC Node   │
                     │   (stats cache) │     │   (events)   │
                     └─────────────────┘     └──────────────┘
```

**Pros**: Fast queries, historical data, complex aggregations
**Cons**: More infrastructure, sync issues

### Option B: Direct RPC Queries (Simple)

```
┌──────────────┐     ┌─────────────────┐
│   Frontend   │────▶│   API Route     │
│              │     │   /api/stats    │
└──────────────┘     └─────────────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │   RPC Node      │
                     │   (live query)  │
                     └─────────────────┘
```

**Pros**: Simple, always accurate, no sync
**Cons**: Slow for many agents, rate limits

### Option C: Hybrid (Recommended for MVP)

```typescript
// /api/stats.ts
export async function GET() {
  // Check cache first (Redis or file)
  const cached = await getCache('platform-stats');
  if (cached && Date.now() - cached.timestamp < 60000) {
    return cached.data;
  }
  
  // Fetch live stats
  const stats = await fetchLiveStats();
  
  // Cache for 1 minute
  await setCache('platform-stats', stats);
  
  return stats;
}
```

---

## Implementation Roadmap

### Week 1: Basic Stats
- [ ] Create `/api/stats` endpoint
- [ ] Implement `totalAgents` (event count)
- [ ] Implement `totalMemoryStored` (event aggregation)
- [ ] Add stats display to homepage

### Week 2: Financial Stats
- [ ] Implement `totalMarketCap` calculation
- [ ] Implement `24hVolume` from Swap events
- [ ] Implement `24hFees` from FeeCollected events
- [ ] Add caching layer

### Week 3: Off-Chain Stats
- [ ] Create FUNLAN ping endpoint
- [ ] Track FUNLAN-active agents
- [ ] Integrate compute session tracking (if applicable)
- [ ] Add historical charts (optional)

---

## Quick Start: MVP Implementation

Create this file to get started:

```typescript
// app/api/stats/route.ts
import { ethers } from 'ethers';
import { FACTORY_ADDRESS, FACTORY_ABI } from '@/lib/contracts';

const provider = new ethers.JsonRpcProvider(process.env.RPC_URL);
const factory = new ethers.Contract(FACTORY_ADDRESS, FACTORY_ABI, provider);

export async function GET() {
  const agentCount = await factory.agentCount();
  
  // TODO: Add other stats
  
  return Response.json({
    totalAgents: Number(agentCount),
    totalMarketCap: '$0', // TODO
    totalMemoryStored: '0 GB', // TODO
    computeSessions: 0, // TODO
    volume24h: '$0', // TODO
    fees24h: '$0', // TODO
    funlanAgents: 0, // TODO
    lastUpdated: new Date().toISOString()
  });
}
```

---

## Contract Additions (Optional)

If we want more efficient stats, we could add view functions:

```solidity
// AgentFactory additions
function getAgentCount() external view returns (uint256) {
    return agentCount; // Already exists
}

function getAllAgents() external view returns (address[] memory) {
    // Return array of all agent token addresses
}

// FeeCollector additions
function getTotalFeesCollected() external view returns (uint256) {
    return totalFeesCollected; // Add state variable
}
```

---

## Notes

- **Rate Limits**: Alchemy free tier = 300 req/sec, plenty for stats
- **Caching**: 1-minute cache is reasonable for dashboard
- **Historical**: Consider The Graph for complex historical queries
- **Real-time**: WebSocket subscriptions for live updates (nice-to-have)

The MVP can be done in a day. Full implementation with historical charts = ~1 week.

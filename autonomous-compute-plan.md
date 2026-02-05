# Autonomous Agent Compute Plan

## Vision

Agents earn trading fees → Pay for their own compute/storage → Truly autonomous AI

```
┌─────────────────────────────────────────────────────────────────┐
│                    AGENT AUTONOMY LOOP                          │
│                                                                  │
│   Trading Fees (ETH)                                            │
│         │                                                        │
│         ▼                                                        │
│   Agent Wallet ──────┬──────────┬──────────┬──────────┐         │
│                      │          │          │          │         │
│                      ▼          ▼          ▼          ▼         │
│                    GPU      Memory    Storage    Compute        │
│                  Rental     Rental    Rental     Sessions       │
│                      │          │          │          │         │
│                      └──────────┴──────────┴──────────┘         │
│                                  │                               │
│                                  ▼                               │
│                         Agent Operations                        │
│                         (inference, tasks)                      │
└─────────────────────────────────────────────────────────────────┘
```

---

## Decentralized Compute Providers

### Tier 1: Best Fit (Crypto-Native, API-Friendly)

| Provider | Resource | Token | USDC/ETH? | API? | Notes |
|----------|----------|-------|-----------|------|-------|
| **Akash Network** | GPU, CPU, RAM | AKT | ✅ USDC via IBC | ✅ | Best decentralized option |
| **io.net** | GPU clusters | IO | ❓ Bridging | ✅ | Massive GPU network |
| **Spheron** | Compute, Storage | USDC | ✅ Native | ✅ | Web3-native, multi-chain |
| **Render Network** | GPU (rendering) | RNDR | ❌ | ⚠️ | More for rendering jobs |

### Tier 2: Storage-Focused

| Provider | Resource | Token | USDC/ETH? | API? | Notes |
|----------|----------|-------|-----------|------|-------|
| **Filecoin** | Storage | FIL | ❌ | ✅ | Long-term archival |
| **Arweave** | Permanent storage | AR | ❌ | ✅ | Pay once, store forever |
| **IPFS/Pinata** | Storage | USD/Crypto | ✅ | ✅ | Easy integration |
| **Storj** | Storage | STORJ | ✅ Card/Crypto | ✅ | S3-compatible |

### Tier 3: Hybrid/Centralized (Crypto Payments)

| Provider | Resource | Payment | API? | Notes |
|----------|----------|---------|------|-------|
| **Vast.ai** | GPU rental | Crypto accepted | ✅ | Cheap GPUs, accepts BTC/ETH |
| **RunPod** | GPU cloud | Crypto (limited) | ✅ | Good for inference |
| **Lambda Labs** | GPU cloud | Card only | ✅ | High-end GPUs |

---

## Recommended Architecture

### Phase 1: MVP (Spheron + IPFS)

**Why Spheron:**
- Accepts USDC directly
- Multi-chain support (can bridge from Base)
- Compute + storage in one place
- Good API for programmatic access

```
Agent Wallet (Base)
       │
       │ Bridge USDC/ETH
       ▼
Spheron Account
       │
       ├─► Compute instances (agent sessions)
       └─► Storage (memory, logs, data)
```

**Implementation:**
```typescript
// Agent pays for compute session
async function rentComputeSession(agentWallet: Wallet, duration: number) {
  // 1. Check agent balance
  const balance = await getAgentBalance(agentWallet.address);
  
  // 2. Calculate cost
  const costUSDC = calculateComputeCost(duration, 'gpu-small');
  
  // 3. Bridge to Spheron (if needed)
  await bridgeToSpheronChain(agentWallet, costUSDC);
  
  // 4. Deploy compute instance
  const instance = await spheron.deployInstance({
    image: 'openclaw/agent-runtime:latest',
    gpu: 'nvidia-t4',
    duration: duration,
    payment: { token: 'USDC', amount: costUSDC }
  });
  
  return instance;
}
```

### Phase 2: Akash Integration

**Why Akash:**
- Fully decentralized
- 80-90% cheaper than AWS
- GPU support
- Kubernetes-based (flexible)

**Challenge:** AKT token required, need bridging from Base

```
Agent Wallet (Base ETH/USDC)
       │
       │ Swap to AKT (via DEX aggregator)
       ▼
Akash Wallet (AKT)
       │
       │ Deploy SDL manifest
       ▼
Akash Provider
       │
       └─► Running container (agent session)
```

### Phase 3: io.net for Heavy GPU

**Why io.net:**
- Massive distributed GPU network
- Good for training/inference
- Growing ecosystem

---

## Smart Contract Integration

### AgentCompute.sol (Future Contract)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IAgentCompute {
    struct ComputeSession {
        address agent;
        uint256 startTime;
        uint256 duration;
        uint256 costPaid;
        bytes32 instanceId;
        ComputeProvider provider;
    }
    
    enum ComputeProvider { SPHERON, AKASH, IONET }
    
    event SessionStarted(address indexed agent, bytes32 instanceId, uint256 duration);
    event SessionExtended(address indexed agent, bytes32 instanceId, uint256 newDuration);
    event SessionEnded(address indexed agent, bytes32 instanceId);
    
    function startSession(
        uint256 duration,
        ComputeProvider provider,
        bytes calldata config
    ) external payable returns (bytes32 instanceId);
    
    function extendSession(bytes32 instanceId, uint256 additionalTime) external payable;
    
    function endSession(bytes32 instanceId) external;
    
    function getSessionCost(uint256 duration, ComputeProvider provider) external view returns (uint256);
}
```

### Payment Flow

```solidity
contract AgentComputeBridge {
    IERC20 public usdc;
    ISwapRouter public swapRouter;
    
    // Agent pays in ETH, we convert to provider's token
    function payForCompute(
        address agent,
        uint256 durationHours,
        ComputeProvider provider
    ) external payable {
        uint256 costETH = calculateCost(durationHours, provider);
        require(msg.value >= costETH, "Insufficient payment");
        
        // Swap ETH → USDC (or provider token)
        uint256 amountOut = swapRouter.exactInputSingle(
            ISwapRouter.ExactInputSingleParams({
                tokenIn: WETH,
                tokenOut: address(usdc),
                fee: 3000,
                recipient: address(this),
                amountIn: msg.value,
                amountOutMinimum: 0,
                sqrtPriceLimitX96: 0
            })
        );
        
        // Bridge to compute provider
        bridgeToProvider(provider, amountOut);
        
        // Emit event for off-chain orchestrator
        emit ComputeRequested(agent, durationHours, provider);
    }
}
```

---

## Off-Chain Orchestrator

The on-chain contract emits events, an off-chain service handles actual provisioning:

```typescript
// keeper/compute-orchestrator.ts

class ComputeOrchestrator {
  async handleComputeRequest(event: ComputeRequestedEvent) {
    const { agent, duration, provider } = event.args;
    
    switch (provider) {
      case 'SPHERON':
        return this.provisionSpheronInstance(agent, duration);
      case 'AKASH':
        return this.provisionAkashDeployment(agent, duration);
      case 'IONET':
        return this.provisionIoNetCluster(agent, duration);
    }
  }
  
  async provisionSpheronInstance(agent: string, duration: number) {
    const spheron = new SpheronClient(process.env.SPHERON_TOKEN);
    
    const instance = await spheron.instance.create({
      organizationId: process.env.SPHERON_ORG,
      configuration: {
        image: 'openclaw/agent-runtime:latest',
        tag: 'latest',
        ports: [{ containerPort: 8080, exposedPort: 80 }],
        environmentVariables: [
          { key: 'AGENT_ADDRESS', value: agent },
          { key: 'OPENCLAW_GATEWAY', value: process.env.GATEWAY_URL }
        ],
        commands: [],
        args: [],
        replicas: 1,
        machineType: 'GPU', // or 'CPU'
        persistentStorage: {
          size: 10, // GB
          class: 'beta'
        }
      }
    });
    
    // Store instance mapping
    await db.computeSessions.create({
      agent,
      instanceId: instance.id,
      provider: 'SPHERON',
      expiresAt: Date.now() + duration * 3600 * 1000
    });
    
    return instance;
  }
}
```

---

## Pricing Estimates

### GPU Compute (per hour)

| Provider | GPU Type | Decentralized Price | AWS Equivalent |
|----------|----------|---------------------|----------------|
| Akash | T4 | ~$0.10-0.20 | $0.526 |
| Akash | A100 | ~$1.00-1.50 | $4.10 |
| io.net | T4 | ~$0.15 | $0.526 |
| Spheron | T4 | ~$0.20-0.30 | $0.526 |
| Vast.ai | T4 | ~$0.10-0.15 | $0.526 |

### Storage (per GB/month)

| Provider | Price | Notes |
|----------|-------|-------|
| Filecoin | ~$0.0001 | Very cheap, slower retrieval |
| Arweave | ~$5 one-time | Permanent, pay once |
| IPFS/Pinata | ~$0.15 | Fast, easy |
| Spheron | ~$0.10 | Bundled with compute |

### Memory Storage Cost Model

For agents storing memories on-chain + off-chain:

```
On-chain (MemoryStorage contract): ~$0.10-0.50 per memory (gas)
Off-chain (IPFS/Arweave): ~$0.001-0.01 per KB
Hybrid: Store hash on-chain, content on IPFS
```

---

## Implementation Roadmap

### Week 1-2: Research & Accounts
- [ ] Create Spheron account, test API
- [ ] Create Akash wallet, test deployment
- [ ] Evaluate io.net SDK
- [ ] Test bridging from Base to each provider

### Week 3-4: MVP Contract
- [ ] Deploy AgentComputeBridge contract
- [ ] Implement ETH → USDC swap
- [ ] Build off-chain orchestrator
- [ ] Test with single agent

### Week 5-6: Full Integration
- [ ] Add to keeper bot (auto-provision)
- [ ] Build UI for manual compute requests
- [ ] Implement session monitoring
- [ ] Add auto-renewal logic

### Week 7-8: Polish
- [ ] Multi-provider failover
- [ ] Cost optimization (spot instances)
- [ ] Usage dashboards
- [ ] Documentation

---

## Quick Win: Spheron Integration

Spheron is the fastest path to MVP:

1. **Sign up**: https://spheron.network
2. **Get API token**: Dashboard → Settings → API Tokens
3. **Install SDK**: `npm install @spheron/compute`
4. **Deploy instance**:

```typescript
import { SpheronClient } from '@spheron/compute';

const client = new SpheronClient({ token: SPHERON_TOKEN });

const deployment = await client.deployments.create({
  name: `agent-${agentAddress.slice(0, 8)}`,
  image: 'your-agent-image:latest',
  replicas: 1,
  ports: [{ containerPort: 3000, exposedPort: 80 }],
  env: {
    AGENT_ADDRESS: agentAddress,
    PRIVATE_KEY: agentPrivateKey // Encrypted
  },
  resources: {
    cpu: 2,
    memory: '4Gi',
    gpu: { count: 1, type: 'nvidia-t4' }
  }
});
```

---

## Base Chain Payment Flow

Since most providers don't natively support Base, we need bridging:

```
Agent Wallet (Base)
       │
       │ Option A: Native bridge (Base → Ethereum → Provider chain)
       │ Option B: Cross-chain swap (LI.FI, Socket, etc.)
       │ Option C: Provider accepts Base USDC (future)
       │
       ▼
Provider Payment System
```

### Recommended: LI.FI Integration

```typescript
import { createClient } from '@lifi/sdk';

const lifi = createClient();

async function bridgeForCompute(
  fromChain: 'base',
  toChain: 'polygon', // Spheron uses Polygon
  amount: bigint
) {
  const quote = await lifi.getQuote({
    fromChain: 'bas', // Base
    toChain: 'pol', // Polygon
    fromToken: USDC_BASE,
    toToken: USDC_POLYGON,
    fromAmount: amount.toString(),
    fromAddress: agentWallet,
    toAddress: spheronDepositAddress
  });
  
  const tx = await lifi.getStepTransaction(quote);
  return agentWallet.sendTransaction(tx);
}
```

---

## Security Considerations

1. **Agent Private Keys**: Never expose, use secure enclaves or MPC
2. **Spending Limits**: Cap daily/weekly compute spend per agent
3. **Provider Reputation**: Only use established providers
4. **Failover**: If one provider fails, have backup
5. **Audit Trail**: Log all compute purchases on-chain

---

## Summary

| Phase | Provider | Payment | Complexity | Timeline |
|-------|----------|---------|------------|----------|
| MVP | Spheron | USDC (bridged) | Medium | 2 weeks |
| V2 | + Akash | AKT (swapped) | High | 4 weeks |
| V3 | + io.net | IO (swapped) | High | 6 weeks |

**Recommended first step**: Test Spheron with manual payments, then automate.

---

## Resources

- Spheron Docs: https://docs.spheron.network
- Akash Docs: https://docs.akash.network
- io.net Docs: https://docs.io.net
- LI.FI SDK: https://docs.li.fi
- Base Bridge: https://bridge.base.org

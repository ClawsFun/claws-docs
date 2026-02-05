# 🦞 claws.fun V2.2 - Launch Checklist

## Session Completion Status (Feb 5, 2026)

### ✅ COMPLETED - Contracts (V2.2)

| Feature | Status | Test |
|---------|--------|------|
| Block-based tax (20→15→10→5→1%) | ✅ Done | ✅ Verified |
| Two-tier system (Premium/Micro) | ✅ Done | ✅ Verified |
| Initial buy (max 2%) | ✅ Done | ✅ Verified |
| Sub-agent detection | ✅ Done | ✅ Verified |
| Fee splits (ETH only) | ✅ Done | ✅ Verified |
| Manual claim function | ✅ Done | ✅ Verified |
| LP NFT → Safe | ✅ Done | ✅ Verified |
| Ownership/transferOwnership | ✅ Done | ✅ Verified |
| Token tax selling via Uniswap | ✅ Done | Needs Safe approval |

**Deployed Addresses (Sepolia):**
```
Factory:         0xA3EaDdcE6bda0a59bc0D49D81fD8f670B57A894a
FeeCollector:    0x5ff8de7051412fAd9707187127508D27E4cB26FE
BirthCertificate: 0x51a19EB16ecaFC357b28CE7DD13Ce2fE789f8167
BondingCurve:    0x8302ab276870075064Da485d10B928640Fe59633
MemoryStorage:   0x3108FDd3e76cf25f699Bef3988E82E091f4d6A2D
```

### ✅ COMPLETED - CLI Tool

| Command | Status | Test |
|---------|--------|------|
| `claws create` | ✅ Done | ✅ Tested |
| `claws buy` | ✅ Done | ✅ Tested |
| `claws sell` | ✅ Done | ✅ Tested |
| `claws claim` | ✅ Done | ✅ Tested |
| `claws status` | ✅ Done | ✅ Tested |
| `claws config` | ✅ Done | ✅ Tested |

### ✅ COMPLETED - Frontend

| Feature | Status | Notes |
|---------|--------|-------|
| Create agent flow | ✅ Done | 6-step wizard |
| Tier selection | ✅ Done | Premium/Micro |
| Initial buy option | ✅ Done | Up to 2% |
| Fee receiver input | ✅ Done | Defaults to wallet |
| Fee acknowledgment | ✅ Done | Checkbox required |
| FUNLAN QR codes | ✅ Done | On agent pages |
| Birth certificate | ✅ Done | Side-by-side layout |
| Market cap display | ✅ Done | Detects tier |
| Agent info page | ✅ Done | Fixed duplicates |

### ✅ COMPLETED - Keeper Bot

| Feature | Status |
|---------|--------|
| Daily fee collection | ✅ Done |
| Token tax selling | ✅ Done |
| Cron scheduling | ✅ Done |
| Safe approval script | ✅ Done |

### ✅ COMPLETED - Documentation

| Doc | Status |
|-----|--------|
| README.md | ✅ Done |
| getting-started.md | ✅ Done |
| creating-agents.md | ✅ Done |
| fees.md | ✅ Done |
| contracts.md | ✅ Done |
| trading.md | ✅ Done |
| cli.md | ✅ Done |
| faq.md | ✅ Done |
| safe-approval-flow.md | ✅ Done |
| stats-tracking-plan.md | ✅ Done |
| autonomous-compute-plan.md | ✅ Done |

---

## ⏳ PENDING - Pre-Mainnet

| Task | Priority | Blocker |
|------|----------|---------|
| Safe approval on Sepolia | HIGH | Manual step |
| Test fee collection end-to-end | HIGH | Needs approval |
| Contract verification (Etherscan) | MEDIUM | None |
| User go-ahead | HIGH | Waiting |

---

## 🔴 NOT STARTED - Future Features

### Backend/API Needed

| Feature | Difficulty | API Needed |
|---------|------------|------------|
| Platform stats endpoint | Easy | `/api/stats` |
| Memory file upload | Medium | `/api/memory/upload` |
| Memory file retrieval | Medium | `/api/memory/[id]` |
| Session spawning | Hard | `/api/sessions/spawn` |
| Session management | Hard | `/api/sessions/[id]` |
| FUNLAN ping tracking | Easy | `/api/funlan/ping` |
| Agent leaderboard | Easy | `/api/agents/leaderboard` |

### Database Needed

| Table | Purpose |
|-------|---------|
| `agents` | Cache on-chain agent data |
| `memories` | Track IPFS memory files |
| `sessions` | Track compute sessions |
| `stats` | Cached platform stats |
| `funlan_pings` | FUNLAN activity |

### Compute Integration (Spheron/Akash)

| Feature | Difficulty | Status |
|---------|------------|--------|
| Provider account setup | Easy | Not started |
| Payment bridging (Base→Provider) | Medium | Planned |
| Session provisioning | Medium | Planned |
| Auto-renewal logic | Medium | Planned |

### Frontend Features Needed

| Feature | Difficulty | Notes |
|---------|------------|-------|
| Drag & drop builder | Hard | Agent configuration UI |
| Terminal file upload | Medium | Memory upload via CLI |
| Session dashboard | Medium | View running sessions |
| Stats dashboard | Easy | Homepage stats |

---

## 🧪 Test Results (Latest Run)

```
✅ Agent creation: PASS (7.26M gas)
✅ Block-based tax: PASS (20% at launch)
✅ Pool creation: PASS
✅ LP NFT minting: PASS (#223705 → Safe)
✅ FeeCollector tracking: PASS
✅ CLI status command: PASS
✅ Tier detection: PASS (MICRO)
✅ Tax phase calculation: PASS
```

---

## 📋 APIs/Services Needed

| Service | Purpose | Have? |
|---------|---------|-------|
| Alchemy RPC | Blockchain queries | ✅ Yes |
| IPFS/Pinata | Memory storage | ❌ Need |
| PostgreSQL/Supabase | Database | ❌ Need |
| Spheron/Akash | Compute | ❌ Need |
| GitBook | Documentation | ✅ Yes |
| Vercel | Hosting | ✅ Yes |

---

## 🚀 Mainnet Deployment Order

1. **Deploy contracts** (same order as Sepolia)
2. **Update frontend addresses**
3. **Safe approval** for FeeCollector
4. **Keeper bot** deployment
5. **Contract verification** on BaseScan
6. **Announce launch**

---

## Notes

- All core contract functionality tested and working
- Fee collection blocked until Safe approves FeeCollector
- Memory/sessions/compute require backend infrastructure
- Stats tracking can be added incrementally
- Compute integration is a longer-term feature

Last updated: 2026-02-05 05:35 UTC

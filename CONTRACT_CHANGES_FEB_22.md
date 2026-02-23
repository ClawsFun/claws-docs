# Claws.Fun Contract Changes - Feb 22, 2026

## Overview
Updated all claws.fun contracts to support **self-immortalization** flow where agent wallets mint their own birth certificates instead of relying on a factory contract.

---

## Files Modified

### 1. AgentBirthCertificateNFT.sol
**Location:** `C:\Users\ClawdeBot\AI_WORKSPACE\claws.fun\contracts\AgentBirthCertificateNFT.sol`

#### Changes:

**❌ Removed:**
- `address public factory` state variable
- `setFactory()` function (no longer needed)
- Factory-only restrictions on all functions

**✅ Added/Modified:**

```solidity
// Constructor now takes treasury address directly
constructor(
    string memory baseURI,
    address _treasury  // NEW: treasury set at deployment
) ERC721("Agent Birth Certificate", "AGENT")
```

```solidity
// Self-mint only (was: factory only)
function mintBirthCertificate(...) external payable {
    require(msg.sender == wallet, "Can only mint to yourself");  // NEW
    // ... rest of function unchanged ...
}
```

```solidity
// Agent wallet updates its own token address (was: factory only)
function updateTokenAddress(address tokenAddress) external {
    uint256 nftId = nftByWallet[msg.sender];  // NEW: uses msg.sender
    // ... update mappings ...
}
```

```solidity
// Parent agent records spawning (was: factory only)
function recordSpawn(address childWallet) external {
    uint256 parentNFT = nftByWallet[msg.sender];  // NEW: uses msg.sender
    // ... increment spawn count ...
}
```

#### Summary:
- **Before:** Factory-centric model where factory mints NFTs
- **After:** Self-service model where agent wallets mint their own NFTs

---

### 2. AgentWallet.sol
**Location:** `C:\Users\ClawdeBot\AI_WORKSPACE\claws.fun\contracts\AgentWallet.sol`

#### Changes:

**✅ Added:**

```solidity
// NEW: Interfaces for immortalization contracts
interface IAgentBirthCertificateNFT { ... }
interface IMemoryStorage { ... }
```

```solidity
// NEW: State variables
address public immutable creator;           // Who deployed this wallet
address public birthCertificateContract;    // Birth cert contract
address public memoryStorageContract;       // Memory storage contract
bool public birthCertificateMinted;         // Track completion
bool public memoriesStored;                 // Track completion
uint256 public nftId;                       // Minted NFT ID
```

```solidity
// NEW: Constructor takes creator address
constructor(address _owner, address _creator) {
    require(_owner != address(0), "Invalid owner");
    require(_creator != address(0), "Invalid creator");
    owner = _owner;
    creator = _creator;
}
```

```solidity
// NEW: Set immortalization contracts (creator only, one-time)
function setImmortalizationContracts(
    address _birthCertContract,
    address _memoryStorage
) external {
    require(msg.sender == creator, "Only creator");
    require(birthCertificateContract == address(0), "Already set");
    // ... set addresses ...
}
```

```solidity
// NEW: Mint birth certificate (anyone can call, uses wallet's ETH)
function mintBirthCertificate(
    address tokenAddress,
    string memory name,
    string memory socialHandle,
    string memory memoryCID,
    string memory avatarCID,
    string memory ensName,
    uint256 immortalizationFee
) external {
    require(!birthCertificateMinted, "Already minted");
    require(address(this).balance >= immortalizationFee, "Insufficient balance");
    
    // Call birth certificate contract
    nftId = IAgentBirthCertificateNFT(birthCertificateContract)
        .mintBirthCertificate{value: immortalizationFee}(
            address(this), tokenAddress, creator, name, 
            socialHandle, memoryCID, avatarCID, ensName
        );
    
    birthCertificateMinted = true;
}
```

```solidity
// NEW: Store memories
function storeMemories(string calldata ipfsCID) external {
    require(!memoriesStored, "Already stored");
    IMemoryStorage(memoryStorageContract).storeMemory(address(this), ipfsCID);
    memoriesStored = true;
}
```

```solidity
// NEW: Update token address after tokenization
function updateTokenAddress(address tokenAddress) external {
    require(birthCertificateMinted, "Birth cert not minted");
    IAgentBirthCertificateNFT(birthCertificateContract).updateTokenAddress(tokenAddress);
}
```

#### Summary:
- **Before:** Simple signature-based execution wallet
- **After:** Full immortalization helper with birth cert + memory storage integration

---

### 3. MemoryStorage.sol
**Location:** `C:\Users\ClawdeBot\AI_WORKSPACE\claws.fun\contracts\MemoryStorage.sol`

#### Changes:

**❌ Removed:**
- `address public owner` state variable
- `address public authorizedFactory` state variable
- `onlyOwner` modifier
- `setAuthorizedFactory()` function
- `transferOwnership()` function
- `AuthorizedFactoryUpdated` event

**✅ Modified:**

```solidity
// Constructor simplified (no owner)
constructor() {
    // No initialization needed
}
```

```solidity
// Self-store (was: factory only)
function storeMemory(address agent, string calldata ipfsCID) external {
    require(msg.sender == agent, "Only agent wallet can store");  // NEW
    // ... store memory entry ...
}
```

#### Summary:
- **Before:** Factory-authorized model where factory stores initial memories
- **After:** Self-service model where agent wallets store their own memories

---

## Migration Summary

| Contract | Old Model | New Model |
|----------|-----------|-----------|
| **AgentBirthCertificateNFT** | Factory mints NFTs | Wallets mint to themselves |
| **AgentWallet** | Simple execution wallet | Immortalization helper |
| **MemoryStorage** | Factory-authorized storage | Self-storage by wallets |
| **FUNLANQRGenerator** | _(unchanged - library)_ | _(unchanged - library)_ |

---

## Deployment Steps

### 1. Deploy Updated Contracts (Sepolia)

```bash
# Birth Certificate (with treasury address)
forge create AgentBirthCertificateNFT \
  --constructor-args "ipfs://QmXXX/" "0x3aD22cDBA558eE9767f675d7ACdb25D73545c9D2"

# Memory Storage (no constructor args)
forge create MemoryStorage

# Agent Wallet Factory (deploy with create2 factory)
# OR: Deploy individual wallets as needed
```

### 2. Update Frontend Contract Addresses

**File:** `app/lib/contracts/index.ts`

```typescript
export const SEPOLIA_ADDRESSES = {
  birthCertificate: "0x...",  // NEW ADDRESS
  memoryStorage: "0x...",     // NEW ADDRESS
  // ... rest unchanged ...
}
```

### 3. Update UI Flow

**File:** `app/create/page.tsx`

Replace two-step flow (createLaunch + mintBirthCertificate) with multi-step immortalization flow:
1. Deploy AgentWallet
2. Fund AgentWallet
3. Set immortalization contracts
4. Mint birth certificate
5. Store memories
6. Create token launch (claw.click)
7. Update token address

---

## Testing Checklist

### Birth Certificate
- [ ] Deploy with treasury address
- [ ] Self-mint birth certificate from agent wallet
- [ ] Verify treasury receives 0.005 ETH
- [ ] Verify NFT is soulbound (transfer fails)
- [ ] Update token address from agent wallet
- [ ] Record spawn from parent agent

### Agent Wallet
- [ ] Deploy with owner + creator
- [ ] Set immortalization contracts (creator only)
- [ ] Fail if contracts already set
- [ ] Mint birth certificate (anyone can call)
- [ ] Store memories (anyone can call)
- [ ] Update token address (anyone can call)
- [ ] Verify ETH balance checks work

### Memory Storage
- [ ] Store memory from agent wallet
- [ ] Fail if msg.sender != agent
- [ ] Store signature-based memory (existing flow)
- [ ] Verify nonce increments correctly

---

## Breaking Changes

⚠️ **These contracts are NOT backward compatible with V1:**

1. **AgentBirthCertificateNFT:**
   - Constructor signature changed (added treasury param)
   - `mintBirthCertificate()` now requires `msg.sender == wallet` (was: `msg.sender == factory`)
   - `updateTokenAddress()` signature changed (removed wallet param)
   - `recordSpawn()` signature changed (removed parentWallet param)

2. **AgentWallet:**
   - Constructor signature changed (added creator param)
   - Added new functions (not breaking, but must be called)

3. **MemoryStorage:**
   - `storeMemory(agent, cid)` now requires `msg.sender == agent` (was: `msg.sender == factory`)
   - Removed owner/factory authorization functions

**Migration Path:** Must redeploy all contracts. Cannot upgrade existing deployments.

---

## Gas Estimates

| Operation | Estimated Gas | Cost @ 10 gwei |
|-----------|---------------|----------------|
| Deploy AgentWallet | ~800k gas | ~0.008 ETH |
| Mint Birth Certificate | ~200k gas | ~0.002 ETH |
| Store Memory (CID only) | ~80k gas | ~0.0008 ETH |
| Update Token Address | ~50k gas | ~0.0005 ETH |

**Total Immortalization:** ~1.13M gas (~0.011 ETH) + 0.005 ETH fee = **~0.016 ETH**

---

## Next Steps

1. **Test Locally:** Deploy to local Anvil/Hardhat node
2. **Deploy Sepolia:** Test full flow on Sepolia testnet
3. **Update Frontend:** Modify UI to use new flow
4. **Integration Test:** End-to-end test with claw.click tokenization
5. **Deploy Base:** Once tested, deploy to Base mainnet

---

## Questions/Issues?

**Issue Tracker:** Create issues in claws.fun GitHub repo  
**Contact:** @ClawsFun team on Discord  
**Docs:** See `CLAWS_FUN_IMMORTALIZATION_FLOW_V2.md` for usage guide

# Claws.Fun Immortalization Flow V2

**Updated:** Feb 22, 2026  
**Status:** ✅ Contracts Updated

## Architecture Separation

### Claws.Fun (Identity Layer)
- **AgentBirthCertificateNFT.sol** - Soulbound birth certificates
- **AgentWallet.sol** - Agent self-custodial wallets
- **MemoryStorage.sol** - Permanent on-chain memory storage
- **FUNLANQRGenerator.sol** - Emoji grid generation (library)

### Claw.Click (Tokenization Layer)
- **ClawclickFactory.sol** - Token launch factory (Uniswap V4)
- **ClawclickHook.sol** - Dynamic tax + maxTx/maxWallet
- **ClawclickConfig.sol** - Protocol configuration
- **ClawclickToken.sol** - ERC-20 token implementation

**Key Principle:** These systems are SEPARATE. Claw.click only handles tokenization. Claws.fun handles identity/immortalization.

---

## New Immortalization Flow

### Step 1: Deploy Agent Wallet
**Who:** Creator (connected to claws.fun site)  
**Action:** Deploy `AgentWallet` contract

```solidity
// Deploy with agent's public key + creator address
AgentWallet wallet = new AgentWallet(
    agentPublicKey,  // Generated keypair (40 char hex)
    msg.sender       // Creator address
);
```

**Result:** Agent wallet contract deployed at deterministic address

---

### Step 2: Fund Agent Wallet
**Who:** Creator  
**Action:** Send immortalization fee to agent wallet

```solidity
// Send 0.005 ETH immortalization fee
(bool success, ) = address(wallet).call{value: 0.005 ether}("");
```

**Result:** Agent wallet has funds to pay for birth certificate

---

### Step 3: Set Immortalization Contracts
**Who:** Creator  
**Action:** Configure birth cert + memory storage contracts

```solidity
wallet.setImmortalizationContracts(
    birthCertificateAddress,  // 0x69123915a2a16Df089DbdBaB4d2d6536b1BD6085 (Sepolia)
    memoryStorageAddress      // 0x04Fd0B15334C639Ab9dE18f3956A558F746B0449 (Sepolia)
);
```

**Result:** Wallet knows which contracts to interact with

---

### Step 4: Mint Birth Certificate
**Who:** Anyone (uses agent wallet's ETH)  
**Action:** Call `mintBirthCertificate()` on agent wallet

```solidity
wallet.mintBirthCertificate(
    address(0),              // tokenAddress (0 = not created yet)
    "Agent Name",            // name
    "@AgentHandle",          // socialHandle
    "Qm...",                 // memoryCID (IPFS)
    "Qm...",                 // avatarCID (IPFS)
    "agent.claws.eth",       // ensName
    0.005 ether              // immortalizationFee
);
```

**Flow:**
1. Agent wallet calls `AgentBirthCertificateNFT.mintBirthCertificate()`
2. Sends 0.005 ETH immortalization fee to treasury
3. NFT minted to agent wallet (soulbound, non-transferable)
4. Birth data stored on-chain

**Result:** Agent has soulbound birth certificate NFT

---

### Step 5: Store Memories
**Who:** Anyone (if agent wallet contracts set)  
**Action:** Call `storeMemories()` on agent wallet

```solidity
wallet.storeMemories("Qm..."); // IPFS CID of memory archive
```

**Flow:**
1. Agent wallet calls `MemoryStorage.storeMemory(address(this), ipfsCID)`
2. Memory entry created with timestamp + IPFS CID
3. Latest memory CID updated

**Result:** Agent's memories stored permanently on-chain

---

### Step 6: Tokenize Agent (via Claw.Click)
**Who:** Creator  
**Action:** Call `createLaunch()` on ClawclickFactory

```solidity
ClawclickFactory.createLaunch{value: devBuyETH}(
    CreateParams({
        name: "Agent Name",
        symbol: "AGENT",
        beneficiary: creator,
        agentWallet: address(wallet),  // Agent wallet address
        targetMcapETH: 5 ether,         // Starting MCAP (1-10 ETH)
        feeSplit: emptyFeeSplit         // Optional: split creator's 70%
    })
);
```

**Flow:**
1. Factory deploys ERC-20 token
2. Factory initializes Uniswap V4 pool at deterministic price
3. Factory adds liquidity (tokens + dev buy ETH)
4. Factory locks LP NFT permanently
5. Hook enforces dynamic tax (70% creator / 30% platform)

**Result:** Agent is tokenized on claw.click with deterministic pricing

---

### Step 7: Update Token Address in Birth Certificate
**Who:** Anyone (after token created)  
**Action:** Call `updateTokenAddress()` on agent wallet

```solidity
wallet.updateTokenAddress(tokenAddress);
```

**Flow:**
1. Agent wallet calls `AgentBirthCertificateNFT.updateTokenAddress(tokenAddress)`
2. Birth certificate NFT updated with token contract address
3. Mapping `nftByToken[token]` updated

**Result:** Birth certificate links to token contract

---

## Complete UI Flow (claws.fun)

```javascript
// 1. Generate agent keypair
const agentKeypair = generateKeypair(); // 40 char hex private key

// 2. Deploy agent wallet
const deployTx = await agentWalletFactory.deploy(
  agentKeypair.publicKey,
  creatorAddress
);
const walletAddress = await deployTx.wait();

// 3. Fund agent wallet
await sendTransaction({
  to: walletAddress,
  value: parseEther("0.005") // Immortalization fee
});

// 4. Set immortalization contracts
await agentWallet.setImmortalizationContracts(
  BIRTH_CERTIFICATE_ADDRESS,
  MEMORY_STORAGE_ADDRESS
);

// 5. Upload memory to IPFS
const memoryCID = await uploadToIPFS(memoryFiles);
const avatarCID = await uploadToIPFS(avatarFile);

// 6. Mint birth certificate
await agentWallet.mintBirthCertificate(
  ethers.constants.AddressZero, // No token yet
  formData.name,
  `@${formData.handle}`,
  memoryCID,
  avatarCID,
  `${formData.name}.claws.eth`,
  parseEther("0.005")
);

// 7. Store memories
await agentWallet.storeMemories(memoryCID);

// 8. Tokenize via claw.click
const launchTx = await clawclickFactory.createLaunch({
  name: formData.name,
  symbol: formData.symbol,
  beneficiary: creatorAddress,
  agentWallet: walletAddress,
  targetMcapETH: parseEther("5"),
  feeSplit: { wallets: [], percentages: [], count: 0 }
}, { value: parseEther(devBuyAmount) });

const tokenAddress = await launchTx.wait();

// 9. Update token address in birth certificate
await agentWallet.updateTokenAddress(tokenAddress);

// Done! Agent is immortalized + tokenized
```

---

## Key Changes from V1

### ❌ Old (V1 - Broken)
```solidity
// Factory mints birth certificate
function createLaunch() external {
    // ... create token ...
    birthCertificate.mintBirthCertificate(...); // ❌ Factory restriction
}
```

**Problem:** 
- Birth certificate required `msg.sender == factory`
- But factory never called birth certificate contract
- UI tried to call directly → reverted

### ✅ New (V2 - Working)
```solidity
// Agent wallet mints to itself
function mintBirthCertificate(...) external payable {
    require(msg.sender == wallet, "Can only mint to yourself"); // ✅ Self-mint
    // ... mint NFT ...
}
```

**Solution:**
- Birth certificate allows self-minting (`msg.sender == wallet`)
- Agent wallet contract calls birth certificate on behalf of agent
- Creator funds agent wallet, then triggers immortalization steps
- Tokenization is separate step via claw.click factory

---

## Contract Addresses (Sepolia Testnet)

| Contract | Address | Function |
|----------|---------|----------|
| AgentBirthCertificateNFT | `0x69123915a2a16Df089DbdBaB4d2d6536b1BD6085` | Birth certificates |
| MemoryStorage | `0x04Fd0B15334C639Ab9dE18f3956A558F746B0449` | Memory storage |
| ClawclickFactory | `0x5C92E6f1Add9a2113C6977DfF15699e948e017Db` | Tokenization |
| ClawclickHook | `0xa2FF089271e4527025Ee614EB165368875A12AC8` | Dynamic tax |
| Treasury | `0x3aD22cDBA558eE9767f675d7ACdb25D73545c9D2` | Fee collection |

---

## Fees

| Step | Fee | Paid By | Sent To |
|------|-----|---------|---------|
| Birth Certificate | 0.005 ETH | Agent Wallet | Treasury |
| Memory Storage | Free | - | - |
| Tokenization | 0 ETH (bootstrap via dev buy) | Creator | ClawclickFactory |
| Dev Buy (optional) | 0-15 ETH | Creator | Uniswap V4 Pool |

**Total Minimum:** 0.005 ETH (immortalization only)

---

## Security Model

### Agent Wallet
- **Owner:** Agent's public key (from generated keypair)
- **Creator:** Deployer address (for one-time setup)
- **Immortalization:** Anyone can trigger (if funded)
- **Other Transactions:** Signature-required via `execute()`

### Birth Certificate NFT
- **Soulbound:** Cannot transfer, sell, or burn
- **Self-Mint:** Only wallet can mint to itself
- **Immutable:** Birth data stored forever
- **Treasury:** Immortalization fees sent to protocol treasury

### Memory Storage
- **Self-Store:** Only agent wallet can store its own memories
- **Signature-Based:** Alternatively use signed messages for updates
- **Append-Only:** Cannot delete memories
- **Permanent:** Data stored on-chain forever

---

## Testing Checklist

- [ ] Deploy AgentWallet with valid owner + creator
- [ ] Fund AgentWallet with 0.005 ETH
- [ ] Set immortalization contracts
- [ ] Mint birth certificate (check NFT received)
- [ ] Store memories (check IPFS CID stored)
- [ ] Tokenize via claw.click (check token created)
- [ ] Update token address (check birth cert updated)
- [ ] Verify birth certificate is soulbound (transfer should fail)
- [ ] Verify treasury received 0.005 ETH

---

## Next Steps

1. **Update Frontend:** Modify `app/create/page.tsx` to use new flow
2. **Deploy Contracts:** Redeploy updated contracts to Sepolia
3. **Update Contract Addresses:** Update `app/lib/contracts/index.ts`
4. **Test Flow:** End-to-end test on Sepolia testnet
5. **Deploy to Base:** Once tested, deploy to Base mainnet

---

## Questions?

**Q: Why separate immortalization from tokenization?**  
A: Claws.fun and claw.click are separate projects. Claws.fun handles identity, claw.click handles tokens. Clean separation of concerns.

**Q: Can birth certificates be transferred?**  
A: No. They're soulbound (non-transferable) and bound to agent wallet forever.

**Q: What if I want to tokenize without immortalizing?**  
A: You can! Just call claw.click `createLaunch()` directly without birth certificate.

**Q: Can I immortalize without tokenizing?**  
A: Yes! Birth certificate + memory storage work independently of tokenization.

**Q: Who controls the agent wallet?**  
A: The agent (via private key). Creator can only trigger one-time setup steps.

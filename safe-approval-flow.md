# Safe Approval Flow for Fee Collection

This document explains how the Safe multisig approves the FeeCollector to collect fees from LP positions.

## Why This Is Needed

When agents graduate to Uniswap V3, the LP NFT (liquidity position) is minted to the **Safe treasury wallet**. This is by design - it keeps the liquidity locked and controlled by the protocol.

However, the FeeCollector contract needs permission to call `collect()` on these positions to gather trading fees. Without approval, fee collection will fail.

## The Approval Flow

### Step 1: Connect to Safe

1. Go to [app.safe.global](https://app.safe.global)
2. Connect your signer wallet (must be a Safe owner)
3. Select the correct Safe:
   - **Sepolia**: `0x8aB7B1bDf7AD2deaE09954B4e9Ec75Db4d18fD89`
   - **Base Mainnet**: *(to be deployed)*

### Step 2: Create the Approval Transaction

1. Click **"New Transaction"** → **"Contract interaction"**
2. Enter the Position Manager address:
   - **Sepolia**: `0x1238536071E1c677A632429e3655c799b22cDA52`
   - **Base Mainnet**: `0x03a520b32C04BF3bEEf7BEb72E919cf822Ed34f1`
3. Select method: `setApprovalForAll`
4. Fill in parameters:
   - `operator`: FeeCollector address
     - **Sepolia**: `0x5ff8de7051412fAd9707187127508D27E4cB26FE`
     - **Base Mainnet**: *(to be deployed)*
   - `approved`: `true`

### Step 3: Sign and Execute

1. Review the transaction details
2. Click **"Submit"** to create the transaction
3. Other Safe owners sign the transaction (if multi-sig)
4. Once threshold is met, click **"Execute"**

## Visual Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                         SAFE WALLET                              │
│                                                                  │
│  Owners: [Signer 1] [Signer 2] [Signer 3]                       │
│  Threshold: 2 of 3                                               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ setApprovalForAll(FeeCollector, true)
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                   POSITION MANAGER (NFT)                         │
│                                                                  │
│  approval[Safe][FeeCollector] = true ✓                          │
│                                                                  │
│  LP Positions:                                                   │
│  - Token #12345 → Agent 0x7b6C...                               │
│  - Token #12346 → Agent 0xacb7...                               │
│  - Token #12347 → Agent 0x...                                   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ Now FeeCollector can call collect()
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      FEE COLLECTOR                               │
│                                                                  │
│  collectFees(agentToken) →                                      │
│    1. Get position ID from agent                                │
│    2. Call positionManager.collect(tokenId, ...)                │
│    3. Sell tokens via Uniswap → ETH                             │
│    4. Distribute ETH to recipients                              │
└─────────────────────────────────────────────────────────────────┘
```

## After Approval

Once the Safe has approved the FeeCollector:

1. **Keeper Bot** can run `collectFees()` automatically
2. **Manual Claims** work via `manualClaim(agentToken)` for agent owners
3. **Fees flow** as ETH to:
   - Agent wallet (45-60%)
   - Creator wallet (0-30%)
   - Platform treasury (25-40%)

## Verification

Check if approval is set:

```javascript
const positionManager = new ethers.Contract(
  POSITION_MANAGER_ADDRESS,
  ['function isApprovedForAll(address owner, address operator) view returns (bool)'],
  provider
);

const isApproved = await positionManager.isApprovedForAll(
  SAFE_ADDRESS,
  FEE_COLLECTOR_ADDRESS
);

console.log('FeeCollector approved:', isApproved);
```

## Script Alternative

Run the approval script (requires Safe signer private key):

```bash
cd keeper
cp .env.example .env
# Edit .env with your Safe signer key
node scripts/approve-fee-collector.js
```

---

## Current Status

| Network | Safe | FeeCollector | Approved |
|---------|------|--------------|----------|
| Sepolia | `0x8aB7B1bDf...` | `0x5ff8de70...` | ❌ Pending |
| Base | TBD | TBD | ❌ Not deployed |

**Next Step**: Execute the approval on Sepolia testnet, then test fee collection with the keeper bot.

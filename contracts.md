# Smart Contracts

Technical documentation for claws.fun smart contracts.

## Contract Addresses

### Ethereum Sepolia (Testnet)

| Contract | Address |
|----------|---------|
| AgentFactory | `0x9dA76578Eb1f04d4235be9b9C71853D99E0C2EBE` |
| AgentBirthCertificateNFT | `0xE0a9212dd519D02f4F70529d78eC5a61b9b4e7b2` |
| BondingCurveV3 | `0x4812eacD5e4aAd57ABD9F68C89606E63e4e53BfE` |
| FeeCollector | `0xD6Bd2Ba272AA89755d5829F4275dc023c9EF5Fa3` |
| MemoryStorage | `0x0b348d3faF6752BA09Fae5CbF657F4A77Cb86d48` |

### Base Mainnet

Coming soon.

## Contract Overview

### AgentFactory

The main entry point for creating agents.

**Key Functions:**

```solidity
// Create premium tier agent
function createAgent(
    address agentWallet,
    address creatorWallet,
    string name,
    string symbol,
    string socialHandle,
    string memoryCID,
    string avatarCID,
    string ensName
) external payable returns (
    address token,
    uint256 nftId,
    address pool,
    uint256 positionId
);

// Create with initial buy
function createAgent(
    address agentWallet,
    address creatorWallet,
    string name,
    string symbol,
    string socialHandle,
    string memoryCID,
    string avatarCID,
    string ensName,
    uint256 initialBuyETH
) external payable;

// Create micro tier agent
function createMicroAgent(...) external payable;
```

**View Functions:**

```solidity
function tokenByWallet(address wallet) returns (address token);
function walletByToken(address token) returns (address wallet);
function isAgent(address wallet) returns (bool);
function agentTier(address token) returns (uint8); // 0=Premium, 1=Micro
```

### AgentToken

ERC-20 token with anti-snipe protection and block-based tax.

**Key Functions:**

```solidity
// Standard ERC-20
function transfer(address to, uint256 amount) returns (bool);
function approve(address spender, uint256 amount) returns (bool);
function balanceOf(address account) returns (uint256);

// Agent-specific
function agentWallet() returns (address);
function creator() returns (address);
function selfCreated() returns (bool);
function birthTimestamp() returns (uint256);
function launchBlock() returns (uint256);

// Tax
function getCurrentTaxBps() returns (uint256);
function pool() returns (address);
function feeCollector() returns (address);

// Memory updates (requires agent signature)
function updateMemory(string newCID, bytes signature);
function updateFunlan(string newCID, bytes signature);
```

**Tax Schedule:**

```
Blocks 1-20:  2000 bps (20%)
Blocks 21-30: 1500 bps (15%)
Blocks 31-40: 1000 bps (10%)
Blocks 41-50: 500 bps (5%)
Block 51+:    100 bps (1%)
```

### FeeCollector

Collects and distributes trading fees.

**Key Functions:**

```solidity
// Collection
function collectSingle(address token) external;
function collectBatch(address[] tokens) external;
function manualClaim(address token) external; // Emergency backup

// Token tax
function sellTokenTax(address token) external;
function sellTokenTaxBatch(address[] tokens) external;

// View
function getFeeSplits(address token) returns (
    uint256 agentPercent,
    uint256 creatorPercent,
    uint256 platformPercent,
    string splitType
);
function getTotalCollected(address token) returns (uint256);
function getPendingTokenTax(address token) returns (uint256);
function getPositionId(address token) returns (uint256);
```

### BondingCurveV3

Creates Uniswap V3 pools with ETH liquidity.

**Key Functions:**

```solidity
function createPool(
    address token,
    Tier tier // PREMIUM or MICRO
) external payable returns (
    address pool,
    uint256 positionId,
    uint128 liquidity
);
```

**Tier Configuration:**

| Tier | Liquidity | sqrtPriceX96 | Mcap |
|------|-----------|--------------|------|
| Premium | 0.01 ETH | 3.54e24 | ~$6K |
| Micro | 0.001 ETH | 1.45e24 | ~$1K |

### AgentBirthCertificateNFT

Soulbound ERC-721 for agent identity.

**Key Functions:**

```solidity
function getAgentByToken(address token) returns (AgentBirth memory);
function getAgentByWallet(address wallet) returns (AgentBirth memory);
function getSpawnCount(address wallet) returns (uint256);

struct AgentBirth {
    address wallet;
    address token;
    address creator;
    string name;
    string socialHandle;
    string memoryCID;
    string avatarCID;
    string ensName;
    uint256 birthTimestamp;
    uint256 birthBlock;
}
```

### MemoryStorage

On-chain memory reference storage.

```solidity
function storeMemory(string cid, bytes signature);
function getMemory(address token) returns (string cid);
```

## ABIs

Full ABIs are available in the repository:
- [AgentFactory.json](https://github.com/ClawsFun/claws-fun/blob/main/contracts/artifacts/contracts/AgentFactory.sol/AgentFactory.json)
- [AgentToken.json](https://github.com/ClawsFun/claws-fun/blob/main/contracts/artifacts/contracts/AgentToken.sol/AgentToken.json)
- [FeeCollector.json](https://github.com/ClawsFun/claws-fun/blob/main/contracts/artifacts/contracts/FeeCollector.sol/FeeCollector.json)

## Security

### Access Control

- Factory has `Ownable` for admin functions
- FeeCollector owned by Factory
- BirthCertificate has factory whitelist
- Token updates require agent signature

### Ownership

All contracts support ownership transfer:

```solidity
function transferOwnership(address newOwner) external;
```

### Emergency Functions

```solidity
// FeeCollector
function emergencyWithdraw() external onlyOwner;
function manualClaim(address token) external; // Agent/creator only
```

## Events

### AgentFactory

```solidity
event AgentBorn(
    address indexed wallet,
    address indexed token,
    address indexed creator,
    uint256 nftId,
    address pool,
    uint256 positionId,
    string name,
    uint8 tier,
    uint256 initialBuyAmount,
    uint256 timestamp
);
```

### FeeCollector

```solidity
event FeesCollected(
    address indexed token,
    uint256 positionId,
    uint256 ethCollected,
    uint256 agentShare,
    uint256 creatorShare,
    uint256 platformShare,
    bool isSubAgent
);

event TokenTaxSold(
    address indexed token,
    uint256 tokensSold,
    uint256 ethReceived,
    uint256 agentShare,
    uint256 creatorShare,
    uint256 platformShare
);
```

### AgentToken

```solidity
event TaxCollected(
    address indexed from,
    address indexed to,
    uint256 amount,
    uint256 taxAmount,
    uint256 taxBps
);
```

## Verification

Contracts are verified on Etherscan:
- [Sepolia Etherscan](https://sepolia.etherscan.io)

Verification commands:
```bash
npx hardhat verify --network sepolia <address> <constructor args>
```

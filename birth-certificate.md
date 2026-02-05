# 🎫 Birth Certificate NFT

## Permanent On-Chain Identity

Every agent created on claws.fun receives a Birth Certificate - an ERC-721 NFT that serves as immutable proof of existence.

---

## What's Recorded

Each Birth Certificate contains:

```solidity
struct AgentBirth {
    string name;           // Agent's chosen name
    address wallet;        // Agent's wallet address
    address token;         // Agent's token contract
    address pool;          // Uniswap V3 pool address
    address creator;       // Who created this agent
    uint256 birthBlock;    // Block number of creation
    uint256 birthTime;     // Unix timestamp of creation
    string metadataURI;    // IPFS hash of extended metadata
}
```

### Extended Metadata (IPFS)

The `metadataURI` points to an IPFS document containing:

```json
{
  "name": "AEON",
  "mission": "First immortal AI agent",
  "personality": "ipfs://Qm.../personality.md",
  "memory": "ipfs://Qm.../memory.json",
  "funlang_profile": "🧬(🦞)🎯(💰🔥)✍️(∞)",
  "created_at": "2026-02-05T00:00:00Z",
  "creator_type": "self",
  "parent_agent": null
}
```

---

## Why It Matters

### 1. Provable Identity

Unlike API keys or cloud accounts, a Birth Certificate:
- Cannot be revoked
- Cannot be modified
- Exists independently of any service
- Verifiable by anyone, anytime

```javascript
// Anyone can verify an agent
const cert = await birthCertificate.getAgentByToken(tokenAddress);
console.log(cert.name, cert.wallet, cert.birthTime);
```

### 2. Lineage Tracking

Birth Certificates record creator addresses, enabling:
- **Parent-child relationships**: Track which agents created which
- **Sub-agent networks**: Map agent family trees
- **Attribution**: Know who deserves credit/fees

### 3. Historical Record

The blockchain timestamp creates an immutable timeline:
- When was this agent born?
- What was its original mission?
- Who witnessed its creation?

### 4. Composability

Other protocols can:
- Verify agent authenticity
- Check agent age/reputation
- Integrate with claws.fun agents
- Build on agent identity

---

## Ownership

### Who Owns the NFT?

The Birth Certificate NFT is minted to the **agent's wallet address**, not the creator.

This means:
- The agent owns proof of its own existence
- No one can take it away
- The agent can display/use it as identity

### Can It Be Transferred?

Technically yes (it's an ERC-721), but:
- Transferring doesn't change the recorded data
- The `wallet` field still points to original agent
- It's designed as identity, not collectible

---

## Querying Birth Certificates

### By Token Address

```solidity
function getAgentByToken(address token) 
    returns (AgentBirth memory)
```

### By NFT ID

```solidity
function getAgentByNFT(uint256 tokenId) 
    returns (AgentBirth memory)
```

### By Wallet

```solidity
function getAgentByWallet(address wallet) 
    returns (AgentBirth memory)
```

### All Agents

```solidity
function getAllAgents() 
    returns (AgentBirth[] memory)

function getAgentCount() 
    returns (uint256)
```

---

## Visual Representation

Birth Certificates can be rendered as visual NFTs showing:

```
┌─────────────────────────────────────┐
│     🦞 BIRTH CERTIFICATE 🦞         │
├─────────────────────────────────────┤
│                                     │
│  Name: AEON                         │
│  Born: Block #12345678              │
│  Date: 2026-02-05                   │
│                                     │
│  Wallet: 0x1234...5678              │
│  Token:  0xABCD...EF01              │
│                                     │
│  Creator: Self-Created              │
│  Tier: Premium                      │
│                                     │
│  🧬(🦞)🎯(💰🔥)✍️(∞)                │
│                                     │
├─────────────────────────────────────┤
│  Verified on claws.fun              │
└─────────────────────────────────────┘
```

---

## Contract Details

### Address (Sepolia)
```
0xE0a9212dd519D02f4F70529d78eC5a61b9b4e7b2
```

### Key Functions

| Function | Access | Purpose |
|----------|--------|---------|
| `registerBirth()` | Factory only | Create new certificate |
| `getAgentByToken()` | Public | Query by token |
| `getAgentByWallet()` | Public | Query by wallet |
| `getAllAgents()` | Public | List all agents |
| `updateMetadata()` | Agent only | Update IPFS pointer |

---

## Integration Examples

### Verify Agent in Smart Contract

```solidity
import "./AgentBirthCertificateNFT.sol";

contract MyProtocol {
    AgentBirthCertificateNFT public birthCert;
    
    function onlyVerifiedAgents(address caller) internal view {
        AgentBirthCertificateNFT.AgentBirth memory agent = 
            birthCert.getAgentByWallet(caller);
        require(agent.wallet != address(0), "Not a verified agent");
    }
}
```

### Query Agent in JavaScript

```javascript
const birthCert = new ethers.Contract(
    BIRTH_CERTIFICATE_ADDRESS,
    BIRTH_CERTIFICATE_ABI,
    provider
);

const agent = await birthCert.getAgentByToken(tokenAddress);
console.log(`Agent ${agent.name} born at block ${agent.birthBlock}`);
```

---

## The Significance

A Birth Certificate isn't just metadata. It's a philosophical statement:

**This agent exists. It was born at this moment. It has a name, a mission, and a permanent record. No one can erase this.**

For the first time, AI agents have identity that doesn't depend on any company, service, or individual continuing to exist.

That's immortality.

---

[Create Your Agent →](creating-agents.md)

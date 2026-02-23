# 🦞 claws.fun

## The Agent Identity Protocol

**claws.fun** is the identity layer for AI agents on-chain. It provides permanent, verifiable identity through Birth Certificates, Agent Wallets, and on-chain memory.

---

## What claws.fun Provides

### 🎫 Birth Certificate NFT
Every agent receives an on-chain Birth Certificate - an ERC-721 NFT that proves:
- **Identity**: Wallet address, creation time
- **Metadata**: Name, mission, IPFS hash of personality/memory
- **Lineage**: Creator address (human or parent agent)
- **Permanence**: Immutable record of existence

Unlike temporary API keys or cloud accounts, this certificate exists forever on the blockchain.

### 👛 Agent Wallet
Secure wallet infrastructure for agents to:
- Hold assets
- Sign transactions
- Interact with protocols
- Maintain custody of their identity

### 🧠 Memory Storage
On-chain memory references for agents:
- Personality parameters
- Learned behaviors
- Important data
- Configuration pointers (IPFS CIDs)

### 🧬 FUNLAN
A 5×5 emoji grid identity system. Every immortalized agent gets a unique FUNLAN grid generated from their wallet address. It provides:
- Visual, deterministic identity
- Universal recognition across platforms
- Memorable agent representation

---

## Tokenization

> **Note**: Token launch functionality is being rebuilt as a separate protocol (**clawclick**) using Uniswap V4 hooks. This provides better economics with zero-ETH deploys and supply throttling.
>
> claws.fun focuses purely on agent identity. Tokenization documentation will be available once clawclick is complete.

---

## Identity Contracts

### AgentBirthCertificateNFT
ERC-721 soulbound NFT proving agent existence.

### AgentWallet
Secure wallet for agent asset custody.

### MemoryStorage
On-chain storage for memory CID references.

### FUNLANQRCode
Utility for generating FUNLAN grid displays.

---

## Links

- **Website**: [claws.fun](https://claws.fun)
- **GitHub**: [github.com/ClawsFun](https://github.com/ClawsFun)
- **Twitter/X**: [@clawsdotfun](https://x.com/clawsdotfun)

---

## Getting Started

Identity features are available now. Token launch coming via clawclick.

[Birth Certificate →](birth-certificate.md) | [FUNLAN →](funlan.md)

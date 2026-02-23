# 🔒 Security

## claws.fun Identity Contracts

### Contracts

| Contract | Purpose | Status |
|----------|---------|--------|
| AgentBirthCertificateNFT | ERC-721 identity NFT | Deployed |
| AgentWallet | Secure agent wallet | Deployed |
| MemoryStorage | On-chain memory refs | Deployed |
| FUNLANQRGenerator | QR code utility | Deployed |

### Security Properties

**AgentBirthCertificateNFT**
- Standard OpenZeppelin ERC-721
- Only factory can mint
- Metadata update requires agent signature
- No admin withdrawal functions

**AgentWallet**
- Minimal proxy pattern
- Agent-controlled only
- No external calls without signature

**MemoryStorage**
- Simple CID storage
- Agent signature required for updates
- No value handling

---

## clawclick Token Contracts

> **Status**: In Development

Token launch contracts will be built with:
- Uniswap V4 hooks architecture
- Foundry test suite
- Professional audit before mainnet

### Planned Security Features

- Protocol-owned LP (no rug vectors)
- Supply throttling (no infinite drain)
- Anti-snipe protection
- Timelock on admin functions

---

## Treasury

Platform fees go to Safe multisig:

**Address:** `0xFf7549B06E68186C91a6737bc0f0CDE1245e349b`

- Multiple signers required
- Cold storage backup
- No single point of failure

---

## Reporting Issues

Found a vulnerability? Please report responsibly:
- GitHub Issues (public)
- Direct contact for critical issues

---

*Full security audit for clawclick contracts will be published before mainnet launch.*

# Common Contract Addresses

This document lists common precompiled contract addresses and system contract addresses on POTOS blockchain.

## Precompiled Contracts

Precompiled contracts are system contracts that are built into the blockchain. They have fixed addresses and provide core functionality.

| Contract Name | Address | Description |
|---------------|---------|-------------|
| SystemConfig | `0x1000` | System configuration management |
| TableFactory | `0x1001` | Table factory for creating tables |
| ConsensusControl | `0x1002` | Consensus node management |
| ConsensusPrecompiled | `0x1003` | Consensus node operations |
| CryptoPrecompiled | `0x1004` | Cryptographic operations |
| ContractLifeCyclePrecompiled | `0x1007` | Contract lifecycle management |
| PermissionPrecompiled | `0x1005` | Permission management |
| CNSPrecompiled | `0x1008` | CNS (Contract Name Service) |
| KVTableFactory | `0x1009` | Key-Value table factory |
| BalancePrecompiled | `0x1011` | Balance management |
| GroupSigPrecompiled | `0x1012` | Group signature operations |
| RingSigPrecompiled | `0x1013` | Ring signature operations |
| CommitteePrecompiled | `0x10001` | Governance committee contract |

## System Contracts

| Contract Name | Address | Description |
|---------------|---------|-------------|
| Committee Contract | `0x10001` | Governance committee for on-chain governance |

## Testnet Addresses

### Testnet Precompiled Contracts

All precompiled contracts use the same addresses on testnet as listed above.

### Testnet System Contracts

- Committee Contract: `0x10001`

## Mainnet Addresses

Coming soon...

## Usage Examples

### Interacting with SystemConfig

```solidity
// Get system parameter
SystemConfigPrecompiled systemConfig = SystemConfigPrecompiled(0x1000);
(string memory value, int256 code) = systemConfig.getValueByKey("tx_gas_limit");
```

### Interacting with Committee Contract

```solidity
// Committee contract interface
address constant COMMITTEE_ADDRESS = 0x10001;
```

## Notes

- All precompiled contract addresses are fixed and cannot be changed
- Precompiled contracts are available on all networks (testnet and mainnet)
- System contract addresses may vary between testnet and mainnet
- Always verify contract addresses before interacting with them

## Additional Resources

- [Precompiled Contracts Documentation](../../glossary/precompiled.md)
- [Committee Contract Documentation](../../developer/committee.md)

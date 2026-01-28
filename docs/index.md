---
myst:
  html_meta:
    "description lang=en": |
      POTOS level 1 blockchain documentation for testnet and mainnet connectivity.
html_theme.sidebar_secondary.remove: true
---

```{toctree}
:glob:
:hidden:

concepts/index
glossary/index
builder/index
nodes/index
references/index
contributor/index
community/index
```

# POTOS

POTOS (Portal of the Orient Symposium)  is a next-generation blockchain solution tailored for the Web3 era. Built on the open-source [FISCO BCOS](https://github.com/FISCO-BCOS/FISCO-BCOS) framework, POTOS is designed to deliver a regulatory-friendly, high-performance, and cost-efficient blockchain platform. With its foundation in Hong Kong, POTOS is driving innovation and fostering sustainable growth in the Web3 ecosystem, both locally and globally.

## Introduction

- [What is POTOS?](./concepts/potos.md)
- [What is FISCO BCOS?](./concepts/fisco-bcos.md)
- [Academic Paper](https://dl.acm.org/doi/10.1145/3581784.3607053)

## Technical Glossary

- [Accounts](./glossary/accounts.md)
- [Transactions](./glossary/transactions.md)
- [Transaction fees](./glossary/transaction-fees.md)
- [Nodes](./glossary/nodes.md)
- [Consensus algorithm](./glossary/consensus.md)

## For Developers

- [Foundation Setup](./builder/foundation_setup.md) - Setup your development environment for POTOS.
- [Connect to the Network](./builder/connect_network.md) - Connect to the POTOS network.
- [Get POT(Gas Token) from Faucet](./builder/faucet.md) - Get POT(Gas Token) from Faucet.
- [Deploy smart contract with Remix](./builder/deploy_with_remix.md) - Deploy smart contract with Remix.
- [Deploy smart contract with Hardhat](./builder/deploy_with_hardhat.md) - Deploy smart contract with Hardhat.
- [Build Your First dApp using Scaffold-ETH 2](./builder/build_dapp_with_scaffold.md) - Build Your First dApp using Scaffold-ETH 2.
- [Participate in DAO Governance](./builder/participate_dao.md) - Participate in DAO Governance.
- [Blockchain Explorer](./builder/explorer_usage.md) - Blockchain Explorer.

## For Validators

Coming soon...

## For Contributors

- [Contributor Guide](./contributor/index.md) - Technical documentation for node developers
  - [Build from Source](./contributor/compile_from_source.md) - Compile FISCO BCOS from source code
  - [Build a Local Chain](./contributor/build_chain.md) - Set up a local blockchain network
- [Contribution Guide](./community/contribute.md) - Learn how to contribute to POTOS

## Additional Resources

- [RPC Endpoints](./references/rpc_endpoints.md) - RPC endpoints for public access.
- [RPC API](./references/rpc_api.md) - RPC API reference.
- [GitHub Repository](https://github.com/FISCO-BCOS/FISCO-BCOS) - Source code and issue tracking.

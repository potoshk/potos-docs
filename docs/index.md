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
developer/index
references/index
```

# POTOS

POTOS (Portal of the Orient Symposium)  is a next-generation blockchain solution tailored for the Web3 era. Built on the open-source [FISCO BCOS](https://github.com/FISCO-BCOS/FISCO-BCOS) framework, POTOS is designed to deliver a regulatory-friendly, high-performance, and cost-efficient blockchain platform. With its foundation in Hong Kong, POTOS is driving innovation and fostering sustainable growth in the Web3 ecosystem, both locally and globally.

## Introduction

- [What is POTOS?](./concepts/potos.md)
- [What is FISCO BCOS?](./concepts/fisco-bcos.md)
- [Academic Paper](https://dl.acm.org/doi/10.1145/3581784.3607053)

## Technical Glossary

- [Accounts](./glossary/accounts.md) - Introduction to blockchain accounts and wallets.
- [Transactions](./glossary/transactions.md) - Explanation of how transactions work on the blockchain.
- [Transaction fees](./glossary/transaction-fees.md) - Overview of transaction fees and their significance.
- [Nodes](./glossary/nodes.md) - Description of node types and their responsibilities.
- [Consensus algorithm](./glossary/consensus.md) - Overview of consensus mechanisms and their role in blockchain.

## For Developers

- [Foundation Setup](./developer/foundation_setup.md) - Setup your development environment for POTOS.
- [Connect to the Network](./developer/connect_network.md) - Learn how to set up your wallet (such as MetaMask) and connect to the POTOS Testnet or mainnet. Step-by-step instructions are provided for both automatic and manual configuration methods.
- [Get POT(Gas Token) from Faucet](./developer/faucet.md) - Discover how to obtain free POT tokens (Gas Token) from our faucet service so you can deploy and test contracts on the testnet.
- [Deploy smart contract with Remix](./developer/deploy_with_remix.md) - Use the Remix online IDE to quickly write, compile, and deploy smart contracts onto POTOS, ideal for beginners and fast prototypes.
- [Deploy smart contract with Hardhat](./developer/deploy_with_hardhat.md) - Set up a powerful local development workflow using Hardhat, enabling automated testing, deployment scripts, and advanced smart contract management on POTOS.
- [Build Your First dApp using Scaffold-ETH 2](./developer/build_dapp_with_scaffold.md) - Follow a hands-on guide to build and connect your first full-stack decentralized application (dApp) using Scaffold-ETH 2 on POTOS.
- [Participate in DAO Governance](./developer/participate_dao.md) - Learn how to join and contribute to on-chain governance via DAOs on the POTOS blockchain, including proposal creation and voting.
- [Blockchain Explorer](./developer/explorer_usage.md) - Get familiar with using the POTOS blockchain explorer to view transactions, smart contracts, accounts, and real-time network activity.

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

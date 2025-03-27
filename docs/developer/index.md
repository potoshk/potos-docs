---
myst:
  html_meta:
    "description lang=en": |
      Development steps on FISCO BCOS.
---

# Usage

## Before you start

This guide will help you get started with developing on the FISCO BCOS.

It will guide you through the process of setting up your development environment, creating a new project, and deploying it to the BCOS.

And more，you can also learn how to participate in consensus and governance on the FISCO BCOS.

## Prerequisites

Before you start developing on the FISCO BCOS, you need to have the following prerequisites:

- Basic knowledge of blockchain technology, see more in [Intro to blockchain](../concepts/blockchain.md), [Intro to wallet](../concepts/wallet.md).
- Understanding advanced concepts of the FISCO BCOS, see more in [](../glossary/index.md).
- Learning Solidity smart contract language, see more in [Solidity](./solidity.md) chapter.

## For Web3.0 developers

If you are new to the FISCO BCOS, you can start with the following steps, which will help you get started with developing on the FISCO BCOS.

### Step 1: Set up your smart contract environment

[Remix](https://remix.ethereum.org) is one of the most famous browser-based IDE (Integrated Development Environment) for developing `Solidity` contracts. It provides a lot of features to help you write, test, and deploy your smart contracts.

For more details: [Connecting Remix to FISCO BCOS](./remix_usage.md)

### Step 2: Connect to the FISCO BCOS

Crypto wallet is the bridge between reality and the blockchain. It allows you to interact with the blockchain, such as sending transactions, deploying smart contracts, and so on.

[MetaMask](https://metamask.io/) is a popular crypto wallet in Web3.0. And it is also compatible with the FISCO BCOS.

For more details: [Connecting MetaMask to FISCO BCOS](./wallet_usage.md)

### Step 3: Deploy your smart contract

After you have set up your smart contract environment, implemented your smart contract, and connected to the FISCO BCOS, you can deploy your smart contract to the FISCO BCOS.

During Step 2, you have connected to the FISCO BCOS using MetaMask. Now you can deploy your smart contract to the FISCO BCOS using Remix.

Here is another simple example to deploy a smart contract using [Hardhat](https://hardhat.org/), which is a smart-contract development environment. [Deploy a smart contract using Hardhat](./hardhat_usage.md).

After you have deployed your smart contract to the FISCO BCOS, you can check the transaction on the FISCO BCOS explorer. Check the usage of explorer: [FISCO BCOS Explorer](./explorer_usage.md)

### Step 4: Develop your DApp

After you have deployed your smart contract to the FISCO BCOS, you can develop your [DApp](../concepts/dapp.md) to interact with the smart contract.

Here is a tutorial of build a first DApp: [Build a first DApp](./dapp_guide.md).

## For nodes maintainers

If you want to build a blockchain instance of FISCO BCOS in your own environment, or run a node of FISCO BCOS, to be a observator, or to participate in consensus and governance, you can start with the following steps.

### Step 1: Build a chain

Follow the [Build a chain](./build_chain.md) guide to build a simple 4-node blockchain.

### Step 2: Run a node

For you to run a node, you can follow the [guide](./run_node.md).

- If you want to participate in consensus and governance, you can follow the [Run a consensus node](./run_consensus.md) guide.
- If you want to be a observer, you can follow the [Run a observer node](./run_observer.md) guide.

More information of node types instructions in [Node types](../glossary/nodes.md).

### Step 3: Configuration and management

After you have built a chain and run a node, you can follow the [Configuration](./config.md) guide to configure and manage your blockchain instance.

And you can also follow the [Management of chain](./management.md) guide to manage your node.

## For FISCO BCOS contributors

Thank you for considering making FISCO BCOS even better! Your contributions are most welcome and appreciated.

To get started, refer to our [CONTRIBUTING](https://github.com/WeTechHK/Universal-BCOS/blob/i18n/CONTRIBUTING.md) guide for a detailed walk-through of the contribution process.

For any contributors interested in developing new features for FISCO BCOS, your participation is highly welcome. You can refer to this document for compiling the source code: [Compile FISCO BCOS from source code](./compile_from_source.md).

```{toctree}
:maxdepth: 1

config
explorer_usage
wallet_usage
remix_usage
hardhat_usage
```

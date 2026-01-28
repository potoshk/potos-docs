# Intro to blockchain

Blockchain technology has revolutionized digital transactions by introducing a decentralized and secure method of recording and transferring value. This doc delves into the essence of blockchain, its defining characteristics, the rationale behind its necessity, and the mechanics of its operation.

## What is Blockchain?

In simple words, a blockchain is a distributed and immutable digital ledger that records transactions and tracks the movement of digital assets across its network. Initially developed for financial assets, blockchains have expanded into various applications, embodying the concept of '[Web3](./web3.md)'.

Blockchains are built upon these fundamental pillars:

1. **Decentralization:** Blockchains operate without a central authority, distributing control among network participants. This decentralization ensures transparency, security, and censorship resistance.
2. **Immutability:** Once recorded, data on a blockchain cannot be altered or deleted. This feature ensures the integrity and trustworthiness of the ledger, making it tamper-proof.
3. **Transparency:** All transactions on a blockchain are visible to network participants, promoting accountability and trust among users.

FISCO BCOS is a next-generation blockchain infrastructure tailored for the [Web3](./web3.md) economy, which features a highly open network with robust Web3 capabilities and seamless user experience.

## Why Blockchain?

The traditional digital transaction models face challenges such as the double-spending problem, where the same digital asset could be fraudulently spent twice. Blockchain elegantly solves this issue without the need for a central authority to verify transactions, enabling secure, transparent, and anonymous peer-to-peer value exchange.

## Types of Blockchain

Blockchains can be categorized based on their accessibility and control:

- Private and Permissioned Blockchains

A permissioned blockchain is run by a **single entity**, such as a company or government, which restricts who can participate and operate a node. This is a completely centralized system and it allows entities to protect users’ identities and data. As a result, these systems are preferred by governments or trade groups aiming to keep control over the system and its data.

Examples include private blockchains like Hyperledger, designed for secure and private data sharing within controlled environments.

- Consortium Blockchains

Consortium blockchains are also permissioned blockchains, but instead of being governed by a single entity, it’s a group of organizations responsible for its management.

A good example of consortium blockchain is [FISCO-BCOS](https://github.com/FISCO-BCOS/FISCO-BCOS). POTOS has been created to serve as a trusted infrastructure for the digital economy.

- Public and Permissionless Blockchains

Public and permissionless blockchains are open to anyone to join and operate a node, offering a higher level of security through decentralization. Public blockchains like Bitcoin exemplify transparency, security, and auditability.

## How Does a Blockchain Work?

A blockchain network operates through nodes that validate and store transactions in blocks. Each block contains a cryptographic hash that links it to the previous block, ensuring that any change in the chain is detectable by the entire network.

Nodes use cryptographic hashes to secure transaction data, converting it into a string of characters that represent the transaction. This hash also includes information from the previous block, creating a chain reaction of dependencies for security.

### Consensus Mechanism

The consensus mechanism is crucial for blockchain networks as it allows nodes to agree on the validity of new blocks. This mechanism ensures data consistency across the network, preventing unauthorized modifications.

[More on Consensus](../glossary/consensus.md)

## Smart Contracts

Smart contracts are self-executing contracts with the terms of the agreement directly written into code. They operate on the blockchain, providing trustless, transparent, and automated execution of predefined conditions.

### Origin and Concept

The term "smart contract" was first introduced by Nick Szabo in 1994, envisioning a digital marketplace where transactions could occur without intermediaries. These contracts are now a reality on platforms like FISCO BCOS.

### Execution and Deployment

Developers can deploy smart contracts on the blockchain, making them accessible to users for execution. This process involves a fee paid to the network, ensuring the sustainability of the ecosystem.

### Practical Applications

Smart contracts enable the creation of complex applications and services, including marketplaces, financial instruments, and games, by leveraging the blockchain's data layer.

## Other essential Blockchain Terminology

### Native Utility Token

Utility tokens provide access to specific products or services within a blockchain ecosystem. They differ from payment tokens as they do not offer ownership or investment stakes.

### Ethereum Virtual Machine (EVM)

The EVM is a global virtual computer that maintains the state of the blockchain. It allows for the execution of code that modifies its state, reflecting the blockchain's dynamic nature.

### Nodes

Nodes are the physical machines that store and communicate the state of the EVM. They ensure that all participants have synchronized information and validate new state changes.

[More on nodes](../glossary/nodes.md)

### Accounts

Accounts are the storage locations for utility tokens. They can be initialized, credited, and used to transfer tokens, with balances recorded in the EVM's state.

[More on accounts](../glossary/accounts.md)

### Transactions

Transactions are requests for code execution on the EVM. Once validated and executed, they result in a state change that is committed and broadcasted across the network.

Transactions are batched into blocks for efficient processing. Blocks typically contain multiple transactions, ensuring the scalability of the blockchain network.

[More on transactions](../glossary/transactions.md)

## References

Szabo, N. (1994). Smart Contracts. [Online] Available: https://www.fon.hum.uva.nl/rob/Courses/InformationInSpeech/CDROM/Literature/LOTwinterschool2006/szabo.best.vwh.net/smart.contracts.html. [Accessed: Oct. 10, 2024].

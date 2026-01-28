# Transactions

## Prerequisites

To help you better understand this page, we recommend you first read [Accounts](./accounts.md) and our [introduction to POTOS](../concepts/potos.md).

## What's a transaction?

A POTOS transaction is initiated by an externally-owned account (EOA), which is managed by a human user rather than a contract, and cryptographically signed by EOA.

Transactions initiated by the EOA are included in the blockchain network only after processes of broadcasting, sealing, execution, and validation. In POTOS, any node can broadcast transactions, but only consensus nodes can execute and validate the transcations.


## Transaction data structure

POTOS supports multiple types of transactions: POTOS transactions and Web3 transactions. The former uses TARS encoding to enhance transaction upgradability, the latter ensures better compatibility with Web3 ecosystem tools.

A typical basic transaction has fields as shown below:

| Field     | Description                                                                                                                                                                                                                                                                                                        |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| from      | The address of the sender                                                                                                                                                                                                                          |
| to        | The receiving address                                                                                                                                      |
| input     | Data attached to the transaction, used for transaction execution, which contains excution args, and encoded according to the [ABI specs](https://docs.soliditylang.org/en/latest/abi-spec.html#formal-specification-of-the-encoding). If this transaction is doing a transfer action, this field will be empty. |
| signature | The cryptographic signature of transaction, what is signed by sender using private key.                                                                                                                                                                                                                                           |
| nonce     | A value used to uniquely identify a sender’s transaction count, primarily used to prevent transaction replay.                                                                                                                                                                                                                                                          |
| value     | The amout of POTOS utility token to be transferred.                                                                                                                                                                                                                                                                |
| gasLimit  | The maximum amount of transaction fee the transaction is allowed to use. **Note:** gasLimit in POTOS is controlled in POTOS system config for now, so this field set manually is not supported.                                                                                                                    |
| gasPrice  | A multiplier to get how much the sender will pay per gas using tokens. The amount of tokens the sender will pay is calculated via `gasUsed` * `gasPrice`. **Note:** gasPrice in POTOS is controlled in POTOS system config for now, so this field set manually is not supported.                                              |

### POTOS transaction

POTOS transaction is based on the basic transaction fields, with additional fields to support POTOS-specific features. Based on all the transaction fields mentioned above, the following fields have been added:

| Field      | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| blockLimit | The maximum block height the transaction is allowed to be included in |
| extraData  | Additional data attached to the transaction                 |
| abi        | ABI json string of contract, set when deploying contract, used to assist in transaction parsing        |
| chainID    | Unlike Web3 chainID, POTOS chainID is a readable string     |
| groupID    | Stand for the group name                                        |

### Web3 transaction compatibility

For embracing [Ethereum Json RPC](https://ethereum.org/en/developers/docs/apis/json-rpc/#json-rpc-methods), POTOS introduces Web3 transaction  to support Ethereum compatibility, so that applications developed based on the Ethereum toolchains can be seamlessly deployed on POTOS.

## Transaction lifecycle

Based on high-performance consensus, execution, and storage mechanisms, POTOS achieves **second-level transaction confirmation latency**. The lifecycle of a transaction mainly includes the following:

- **Transaction creation**: EOA creates transactions by invoking client toolkits, all necessary fields in the transaction must be set.

- **Transaction verify**: The created transaction is sent to the blockchain node through client toolkits. Before inserting the transaction into the transaction pool, the transaction verify is required, including signature verification, nonce checks, and so on.

- **Transaction broadcasting**: To ensure that the transaction reaches the majority of nodes and increases the transaction success probability, after transaction is successfully verified, it will be broadcast to other nodes via the P2P protocol.

- **Transaction sealing**: Responsible for sealing the successfully verified transactions in the transaction pool into blocks, only the consensus leader executes this process. In POTOS, all consensus nodes take turns serving as the leader.

- **Transaction execution**: After the leader generates a block, it broadcasts the block to all other consensus nodes in the form of a consensus proposal. The proposal verification process involves block execution. POTOS supports parallel execution of transactions to achieve higher performance.

- **Consensus**: POTOS adopts the PBFT consensus algorithm and plans to upgrade to the more scalable PoS-rPBFT algorithm in the future. 

- **Transaction persistence**: After consensus finalize, transactions are persistent to the storage, submitted transactions will be removed from the transaction pool, and a receipt will be pushed to the client toolkits. Of course, users can also poll for transaction receipts based on the transaction hash.
# Nodes

POTOS is a distributed network of computers running software that can verify blocks and transaction data (known as nodes).

## Node types

There are three types of nodes:

- **Consensus Node**: Nodes participate in validation of transactions and blocks
- **Observer Node**: Nodes that sync and store all ledger data, including transactions, blocks and states
- **Light Node**: Nodes only sync block headers and support SPV functions

### Consensus node

Consensus nodes are responsible for proposing and validating new blocks

- Store the complete blockchain data
- Actively participate in block validation by verifying all blocks and states
- Provide services such as transaction submission, contract invocation, and ledger queries


### Observer node

Observer nodes only do not participate in the consensus process but can pull full blocks from consensus nodes.

- Similar to the consensus nodes, the observer nodes store all the blockchain data 
- Pull blocks from consensus nodes, execute and validate locally. Only blocks, transactions, and states that pass validation can be committed.
- Provide services such as transaction submission, contract invocation, and ledger queries.

### Light node

Light nodes sync block headers, Merkle proofs, and other information from consensus/observer nodes. Mainly used to verify the existence of transactions and receipts.

- Sync and store block headers only
- Provide services such as transaction submission, contract invocation, and ledger queries(for data such as transactions, blocks, and states that are not stored locally, retrieve them from full nodes via the P2P network.).

## Running your own node

Interested in running your own POTOS node? 

Please visit [How to run POTOS observer nodes](../developer/run_observer.md) for more details.

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
- Respond to client requests


### Observer node

Observer nodes 
Observer nodes are full nodes that verify every block from genesis but do not have to participate producing new blocks.

- Stores full blockchain data as Consensus node
- Sync blocks from Consensus node and verifies all blocks and states.
- Serves the network and provides data on request

### Light node

The light node can then independently verify the data they receive against the state roots in the block headers. Light nodes enable users to participate in the POTOS network without the powerful hardware or high bandwidth required to run full nodes.

- Stores block headers only
- Sync block headers from Consensus node and verifies merkle proof
- Serves the network and retransmission request to full nodes

## Running your own node

Interested in running your own POTOS node?

For a beginner-friendly introduction visit our [How to run POTOS observer nodes](../developer/run_observer.md) page to learn more.

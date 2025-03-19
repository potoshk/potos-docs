# Nodes

FISCO BCOS is a distributed network of computers (known as nodes) running software that can verify blocks and transaction data. The software must be run on your own environment to turn it into an FISCO BCOS node.

## Node types

There are three types of nodes:

- Consensus Node
- Observer Node
- Light Node

### Consensus node

Consensus nodes are responsible for validating transactions and producing new blocks. Also, consensus nodes do a block-by-block validation of the blockchain, including downloading and verifying the block body and state data for each block.

- Stores full blockchain data (although this is periodically pruned so a full node does not store all state data back to genesis)
- Participates in block validation, verifies all blocks and states.
- Serves the network and provides data on request.

### Observer node

Observer nodes are full nodes that verify every block from genesis but do not have to participate producing new blocks.

- Stores full blockchain data as Consensus node
- Sync blocks from Consensus node and verifies all blocks and states.
- Serves the network and provides data on request.

### Light node

The light node can then independently verify the data they receive against the state roots in the block headers. Light nodes enable users to participate in the FISCO BCOS network without the powerful hardware or high bandwidth required to run full nodes.

- Stores block headers only
- Sync block headers from Consensus node and verifies merkle proof
- Serves the network and retransmission request to full nodes

## Running your own node

Interested in running your own FISCO BCOS node?

For a beginner-friendly introduction visit our [run a node](../develop/run_node.md) page to learn more.

# Run a node

POTOS provides three types of nodes: consensus nodes, observer nodes and light nodes.

- Consensus nodes are responsible for validating transactions and producing new blocks.
- Observer nodes are full nodes that verify every block from genesis but do not have to participate producing new blocks.
- Light nodes only download block headers and request other information from consensus nodes or observer nodes.

In this section, you will learn how to run a consensus node, observer node and light node on your own environment.

## Run a consensus node

Running a consensus node is the most common way to participate in the POTOS network. As core components of the network, consensus nodes play a crucial role in the blockchain's governance by validating transactions and proposing new blocks. Running a consensus node can enhance your influence on POTOS network, and shape the future direction of the blockchain together.

For more information, see [Run a consensus node](./run_consensus.md).

## Run a observer node

Running a observer node can maintain a complete copy of the blockchain, ensuring that every transaction and block since the genesis block is verified, which is beneficial for users requiring full data integrity. And more, observer nodes is ideal for users who need to analyze blockchain data in depth and provide an additional layer of security by cross-checking the blockchain's history without the obligation to produce new blocks.

For more information, see [Run a observer node](./run_observer.md).

## Run a light node

Running a light node is a lightweight way to participate in the POTOS network. It require minimal resources, making them suitable for users with limited computational power or those who wish to minimize their computation consumption.

For more information, see [Run a light node](./run_lightnode.md).

```{toctree}
:maxdepth: 1
:hidden:

run_consensus
run_observer
run_lightnode
```

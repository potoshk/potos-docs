# Consensus algorithm

Blockchain system guarantees system consistency through consensus algorithm. In theory, consensus is a fundamental problem in distributed computing and multi-agent systems is to achieve overall system reliability in the presence of a number of faulty processes, which often requires coordinating processes to reach consensus, or agree on some data value that is needed during computation.

In regard to the POTOS blockchain, the process is formalized, and reaching consensus means that most of the nodes on the network agree on the global state of the network.

POTOS currently supports PBFT(Practical Byzantine Fault Tolerance)and PoS-rPBFT algorithms.

## Types of consensus mechanisms

According to whether or not to tolerate [Byzantine fault](https://en.wikipedia.org/wiki/Byzantine_fault), consensus algorithms can be classified as Crash Fault Tolerance(CFT) algorithms and Byzantine Fault Tolerance(BFT) algorithm:

- CFT class algorithm is common fault-tolerant algorithm. When the system failure, server crash and other common failures, can still reach a consensus on a proposal. The classic algorithm includes Paxos, Raft, etc. This kind of algorithm has better performance, faster processing speed, can tolerate no more than half of the failed nodes.

- BFT class algorithm : In addition to tolerating ordinary failures in the system consensus process of CFT, but also tolerating deliberate deception by some nodes(such as falsifying transaction execution results)Such as Byzantine errors, classical algorithms include PBFT, etc. which have poor performance and can tolerate no more than one-third of faulty nodes。

## PBFT

PBFT consensus algorithms uses cryptographic algorithms such as signature, signature verification, and hash to ensure tamper-proof, forgery-proof, and non-repudiation in the messaging process, and reduce the complexity of the Byzantine fault-tolerant algorithm from the exponential level to the polynomial level.

### Important concepts of PBFT

Node type, node ID, node index and view are key concepts of PBFT consensus algorithm。For basic concepts of blockchain systems, please refer to Key Concepts.

#### Node type

- Leader/Primary: Consensus node, responsible for packaging transactions into blocks and block consensus, each round of consensus process has and only one leader, in order to prevent the leader from forging blocks, after each round of PBFT consensus, will switch the leader；

- Replica: Replica node, which is responsible for block consensus. There are multiple Replica nodes in each round of consensus. The process of each Replica node is similar；

- Observer: The observer node, which is responsible for obtaining the latest block from the consensus node or the replica node, and after executing and verifying the block execution result, the resulting block is on the chain.

Leader and Replica are collectively referred to as consensus nodes.

#### Node ID & & Node Index

In order to prevent nodes from doing evil, each consensus node in the PBFT consensus process signs the messages it sends and checks the signatures of the received message packets, so each node maintains a key pair, the private key is used to sign the messages it sends, and the public key is used as the node ID to identify and check the signatures.

**Node ID:** Consensus node signature public key and consensus node unique identifier, usually a 64-byte binary string, other nodes use the node ID of the message packet sender to verify the message packet

Considering that the node ID is very long, including this field in the consensus message will consume part of the network bandwidth, POTOS introduces a node index, each consensus node maintains a public consensus node list, and the node index records the position of each consensus node ID in this list:

**Node index:** The position of each consensus node ID in this list of common node IDs

#### Consensus View

The PBFT consensus algorithm uses views to record the consensus status of each node, and the same view node maintains the same list of Leader and Replicas nodes.

When the leader fails, a view switch occurs. If the view switch is successful(At least 2*f+1 node reaches the same view), select a new leader based on the new view, and the new leader starts to produce new block, otherwise continue to switch views until most of the nodes in the network(Greater than or equal to 2*f+1)Achieve consistent view.

In the POTOS system, the formula for calculating the leader index is as follows:

```text
leader_idx = (view + block_number) mod node_num
```

### Core processes

PBFT consensus mainly includes three stages: Pre-prepare, Prepare, and Commit:

- **Pre-prepare:** Responsible for executing blocks, generating signature packets, and broadcasting the signature packets to all consensus nodes;

- **Prepare:** responsible for collecting signature packages, a node collects full '2*f+1' after the signature package, indicating that it has reached the state of being able to submit blocks, start broadcasting the Commit package;

- **Commit:** responsible for collecting Commit packages, a node collects full '2*f+1' Commit packages the latest locally commit block to the database。

![](../images/consensus/pbft_process.png)

### View change processes

The following figure simply view change process where blockchain contains 4(3*f+1, f=1)’ nodes, and the third node(node3) is the Byzantine node.

![](../images/consensus/pbft_view.png)

## PoS-rPBFT

Consensus algorithms based on the principle of distributed consistency, such as BFT and CFT consensus algorithms, have the advantages of low transaction confirmation delay, final consistency, high throughput, and no power consumption.

However, the complexity of communication in these algorithms is related to the size of the nodes. So the size of the network that can be supported is limited, which is greatly limits the scalability of the blockchain nodes。

POTOS proposes the PoS-rPBFT consensus algorithm, which aims to minimize the impact of node size on the consensus algorithm while preserving the high performance, high throughput, high consistency, and security of BFT-like consensus algorithms.

### Node Type

- Consensus committee nodes: the node executes the PBFT consensus process. Committee nodes will be rotated after a certain block height.

- Candidate consensus node: do not paticipates the consensus process, verify blocks and check whether the consensus node is legal. After a certain block height, the consensus committee nodes will be rotated based on the election weight.

### Core process

![](../images/consensus/rpbft.png)

The rPBFT algorithm selects only a number of consensus nodes for each round of consensus process, and periodically replaces the consensus nodes according to the block height to ensure system security, including two system parameters:

- 'epoch_sealer_num': the number of nodes participating in the consensus process in each round of consensus. You can dynamically configure this parameter by sending transactions on the console.

- 'epoch_block_num': The consensus node replacement cycle. To prevent the selected consensus nodes from being associated with each other, the rPBFT replaces a consensus node for each epoch_block_num block. You can dynamically configure this parameter by issuing transactions on the console

These two configuration items are recorded in the system configuration table. The configuration table mainly includes three fields: configuration keyword, configuration corresponding value, and effective block height. The effective block height records the latest effective block height of the latest configuration value. For example, set ‘epoch_sealer_num’ and ‘epoch_block_num’ to 4 and 10000 respectively in a 100-block transaction.

#### Blockchain initialization

During blockchain initialization, rPBFT needs to select 'epoch_sealer_num' consensus nodes to participate in consensus among consensus members. Currently, the initial implementation is to select nodes with indexes from 0 to 'epoch_sealer_num'-1 to participate in the consensus of the previous 'epoch_block_num' blocks.

#### PBFT runs in consensus committee

The selected 'epoch_sealer_num' consensus member nodes run the PBFT consensus algorithm to verify node synchronization and verify the blocks generated by the consensus of these consensus member nodes.

- Checked list of block signatures: each block must contain the signatures of at least two-thirds of the consensus members.

- Check the execution result of the block: the execution result of the local block must be consistent with the execution result recorded by the consensus committee in the block header.

#### Dynamic rotating of consensus committee

To ensure system security, the rPBFT algorithm removes a node from the consensus member list as a validation node and adds a validation node to the consensus member list after each 'epoch_block_num' block, as shown in the following figure:

![](../images/consensus/epoch_rotating.png)

In the current implementation of the rPBFT algorithm, the consensus member list nodes are replaced by validation nodes in turn. If the current ordered consensus committee node list is `CommitteeSealersList` and the total number of consensus nodes is `N`, then after the consensus `epoch_block_num` blocks, the `CommitteeSealersList [0]` will be removed from the consensus member list and added to the index `(CommitteeSealersList[0].IDX + epoch_sealer_num) % N` to consensus member list。Round `i` replacement cycle, remove `CommitteeSealersList [i% epoch _ sealer _ num]` from the list of consensus members and add the index as `(CommitteeSealersList[i%epoch_sealer_num].IDX + epoch_sealer_num) % N` to consensus member list。

# How to run POTOS observer nodes

This document is intended to help community users run POTOS observer nodes step-by-step.

Currently, **only access to the POTOS testnet is available**; access to the POTOS mainnet will be come soon. Stay tuned!


## System requirements

Running an observer node requires the following system requirements:

| Specifications | Recommended            | Lowest               |
|----------------|------------------------|----------------------|
| CPU            | 16 cores               | 4 cores              |
| Memory         | 64 GB                  | 8 GB                 |
| Storage        | > 4 TB SSD, 4,000 Mbps | > 1 TB SSD, 800 Mbps |
| Network        | 10 Gbps                | 10 Mbps              |


### Storage Estimation

To address storage expansion, the requirement is determined by both the block size and transaction volume. Assuming **average transaction size of 200 bytes** and with each block containing approximately **100 transactions**, the storage needed per block stands at 20 KB. 

With a block interval set at 1 second, this translates to a daily storage requirement of 20 KB multiplied by 24 hours by 3600 seconds, **equating to 1.65 GB per day**. 

This calculation underscores the importance of considering both the frequency and size of transactions when planning for storage capacity.

### Operation System

- (Recommended) AWS Amazon Linux2, or other linux-based environments in mainstream cloud services provider.
- (Recommended) Ubuntu 22.04 LTS in x86_64 or arm64.
- CentOS 7/8 in x86_64 ir arm 64.
- macOS 13.0 or later in  x86_64 or arm64.

## Deploy a observer node step-by-step

### Step 1. Download POTOS build-essentials

Download the `build_chain.sh` from the [github release](https://github.com/FISCO-BCOS/FISCO-BCOS/releases).

```bash
# Create a directory to store the node binary, scripts, configuration, and data
$ mkdir -p ~/fisco && cd ~/fisco
# download the build_chain.sh for building nodes
$ curl -#LO https://github.com/FISCO-BCOS/FISCO-BCOS/releases/download/v3.14.0/build_chain.sh && chmod u+x build_chain.sh

# download config.genesis and bootstrap-nodes config of POTOS testnet
$ curl -#LO https://github.com/potoshk/potos-docs/releases/download/v1.0.0/POTOS-testnet.tgz
$ tar -xvf POTOS-testnet.tgz
```

### Step 2. Generate the node

Generate the node config using downloaded `build_chain.sh`:

```bash
# generate a self observer node
$ bash build_chain.sh -C generate-template-package -o self-observer-node -G POTOS-testnet -k 0

# generate the node private key
$ bash build_chain.sh -C generate_cert -o self-observer-node/node0/conf

# The node directory structure is as follows:
$ tree self-observer-node
self-observer-node
├── fisco-bcos
├── node0
│   ├── conf
│   │   ├── ca
│   │   │   ├── ca.crt
│   │   │   ├── ca.key
│   │   │   ├── ca.srl
│   │   │   └── cert.cnf
│   │   ├── ca.crt
│   │   ├── cert.cnf
│   │   ├── node.nodeid
│   │   ├── node.pem
│   │   ├── ssl.crt
│   │   ├── ssl.key
│   │   └── ssl.nodeid
│   ├── config.genesis
│   ├── config.ini
│   ├── nodes.json
│   ├── start.sh
│   └── stop.sh
├── start_all.sh
└── stop_all.sh
```


### Step 3. Start the observer node

Run the `start_all.sh` script to start the observer node.

Before starting the node, please ensure that port `30300`, `20200` and `8545` are not occupied by other services

If you want to modify the configuration of the node, you can edit the `config.ini` file of the node directory, check more details in the [Configure observer node](../developer/config.md) section.



```bash
$ cd ~/fisco/self-observer-node
# Start observer node
$ bash start_all.sh
# Observer node pid is 98622
 node0 start successfully pid=98622

$ ps -aux | grep bcos
501 98622     1   0 10:36 ttys019   13:33.97 ~/fisco/self-observer-node/node0/../fisco-bcos -c config.ini -g config.genesis
```

After starting the observer node, it will start to synchronize the block data. You can grep the log file to check the synchronization status.

```bash
$ tail -f node0/log/* | grep Report

# Many logs will be printed, and the following is an example of the log
info|2025-4-6 22:36:31.415281|[CONSENSUS][PBFT][METRIC]^^^^^^^^Report,sealer=2,txs=1,committedIndex=203,consNum=204
```


### Step 4. Connecting MetaMask to Observer Node

After the observer node is started, it will start a JSON-RPC service on port `8545`. You can connect MetaMask to the observer node by adding a custom RPC.

If you do not have a wallet yet, create an account using MetaMask (choose any Ethereum-compatible account type) [here](https://metamask.io/download/). For more information on MetaMask, please refer [here](https://docs.metamask.io/).

To access the observer node network, follow steps below to configure MetaMask:

- Open the “Network” setting, click “Add a network”:

    ![](../_static/developer/connect_1.png)

- Click “Add a network manually”:

    ![](../_static/developer/connect_2.png)

- Fill in required information for the local observer node network and click “Save”.
  - The default chain ID for FISCO BCOS is `20200` and the default RPC URL is `http://127.0.0.1:8545`. The `Network name` and `Currency symbol` are not required, you can fill in any name you like.

    ![](../_static/developer/connect_3.png)

- You will see the observer node network added to the list.

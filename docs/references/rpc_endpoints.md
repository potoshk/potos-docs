# JSON-RPC Endpoints

## Overview

Publicly exposed JSON-RPC endpoints allow you to test and run your blockchain products by providing interaction with the POTOS network without running your own node.

Running your own POTOS Endpoint Node is not simple. It requires technical expertise, monitoring, and computing resources. It comes with the cost of maintaining storage, network bandwidth, as well as having to divert engineering time and resources; nodes must be kept up to date and health checked regularly.

Hence, the main benefit of using an existing Public EN is that it allows you to solely focus on building and testing your blockchain product without the distraction of maintaining infrastructure to connect and interact with the POTOS network.

## POTOS Networks

POTOS has two networks: **Testnet** and **Mainnet**.

- **Testnet** is for testing and development purposes.
- **Mainnet** is for production purposes.

| Network Information | POTOS Mainnet          | POTOS Testnet                  |
|---------------------|------------------------|--------------------------------|
| Network name        | POTOS Mainnet          | POTOS Testnet                  |
| RPC URL             | https://rpc.potos.hk/  | https://rpc-testnet.potos.hk/  |
| Chain ID            | 60603                  | 60600                          |
| Currency symbol     | POT                    | POT                            |
| Block explorer URL  | https://scan.potos.hk/ | https://scan-testnet.potos.hk/ |

If RPC endpoints or chain settings are updated in the future, you can find the latest information on [ChainList](https://chainlist.org): [POTOS Mainnet](https://chainlist.org/chain/60603) and [POTOS Testnet](https://chainlist.org/chain/60600).

## Usage Example

You can use these endpoints with any JSON-RPC compatible client. Here is an example using curl against the Testnet:

```bash
curl -X POST https://rpc-testnet.potos.hk/ \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": 1
  }'
```

For Mainnet, replace the URL with `https://rpc.potos.hk/`.

# RPC API Reference

This document provides detailed information about the JSON RPC API structure and supported methods.

## RPC Structure

POTOS supports JSON RPC 2.0 specification. All RPC calls follow this structure:

```json
{
  "jsonrpc": "2.0",
  "method": "method_name",
  "params": [...],
  "id": 1
}
```

## Supported Methods

### Standard Ethereum JSON RPC Methods

POTOS supports most standard Ethereum JSON RPC methods:

#### Blockchain Methods

- `eth_blockNumber` - Returns the number of the most recent block
- `eth_getBlockByHash` - Returns information about a block by hash
- `eth_getBlockByNumber` - Returns information about a block by number
- `eth_getBlockTransactionCountByHash` - Returns the number of transactions in a block by hash
- `eth_getBlockTransactionCountByNumber` - Returns the number of transactions in a block by number
- `eth_getTransactionByHash` - Returns transaction information by hash
- `eth_getTransactionByBlockHashAndIndex` - Returns transaction information by block hash and index
- `eth_getTransactionByBlockNumberAndIndex` - Returns transaction information by block number and index
- `eth_getTransactionReceipt` - Returns transaction receipt by hash

#### Account Methods

- `eth_getBalance` - Returns the balance of an account
- `eth_getCode` - Returns code at a given address
- `eth_getStorageAt` - Returns the value from a storage position at a given address

#### Transaction Methods

- `eth_sendTransaction` - Creates a new message call transaction or a contract creation
- `eth_sendRawTransaction` - Sends a signed transaction
- `eth_estimateGas` - Estimates the gas needed for a transaction
- `eth_getTransactionCount` - Returns the number of transactions sent from an address

#### State Methods

- `eth_call` - Executes a new message call immediately without creating a transaction
- `eth_getLogs` - Returns logs matching the given filter

#### Network Methods

- `eth_chainId` - Returns the chain ID
- `net_version` - Returns the network ID
- `net_listening` - Returns true if client is actively listening for network connections
- `net_peerCount` - Returns number of peers currently connected to the client

#### Utility Methods

- `web3_clientVersion` - Returns the current client version
- `web3_sha3` - Returns Keccak-256 hash of the given data

### FISCO BCOS Specific Methods

POTOS, built on FISCO BCOS, also supports additional methods:

- `getBlockNumber` - Returns the current block number
- `getPbftView` - Returns the current PBFT view
- `getConsensusStatus` - Returns consensus status information
- `getSystemConfigByKey` - Returns system configuration by key
- `getNodeVersion` - Returns the node version

## Response Format

All RPC responses follow this structure:

```json
{
  "jsonrpc": "2.0",
  "result": ...,
  "id": 1
}
```

In case of an error:

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Error message"
  },
  "id": 1
}
```

## Rate Limiting

RPC endpoints may have rate limiting in place. Please refer to the specific endpoint documentation for rate limit details.

## Authentication

Some endpoints may require authentication. Please check with the network operator for authentication requirements.

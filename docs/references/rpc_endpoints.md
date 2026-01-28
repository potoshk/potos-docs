# JSON RPC Endpoints

This document lists the available JSON RPC endpoints for POTOS networks.

## Testnet

- **RPC URL**: `https://rpc-testnet.potos.hk/`
- **Chain ID**: `60600`
- **Block Explorer**: `https://scan-testnet.potos.hk/`

## Mainnet

Coming soon...

## Usage Example

You can use these endpoints with any JSON RPC compatible client. Here's an example using curl:

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

## Connecting with MetaMask

To connect MetaMask to POTOS Testnet:

1. Open MetaMask
2. Click on the network selector
3. Select "Add Network"
4. Enter the following information:
   - Network Name: POTOS Testnet
   - RPC URL: `https://rpc-testnet.potos.hk/`
   - Chain ID: `60600`
   - Currency Symbol: POT
   - Block Explorer URL: `https://scan-testnet.potos.hk/`

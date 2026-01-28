# Transaction Error Codes

This document lists common transaction error codes and their meanings on POTOS blockchain.

## Error Code Format

Transaction errors are returned in the following format:

```json
{
  "code": -32000,
  "message": "Error description",
  "data": {
    "errorCode": "0x...",
    "errorMessage": "Detailed error message"
  }
}
```

## Common Error Codes

### Gas Related Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| `0x01` | Out of gas | Increase gas limit |
| `0x02` | Gas price too low | Increase gas price |
| `0x03` | Gas limit exceeded | Reduce transaction complexity or increase gas limit |

### Transaction Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| `0x10` | Invalid transaction | Check transaction parameters |
| `0x11` | Nonce too low | Use correct nonce |
| `0x12` | Nonce too high | Use correct nonce |
| `0x13` | Insufficient balance | Ensure account has sufficient balance |
| `0x14` | Transaction already in pool | Wait for transaction to be processed |

### Contract Execution Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| `0x20` | Revert | Check contract logic and parameters |
| `0x21` | Invalid opcode | Check contract bytecode |
| `0x22` | Stack overflow | Reduce contract complexity |
| `0x23` | Stack underflow | Check contract logic |

### Permission Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| `0x30` | Permission denied | Check account permissions |
| `0x31` | Contract not authorized | Authorize contract access |
| `0x32` | Account not authorized | Authorize account access |

### System Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| `0x40` | System configuration error | Contact network administrator |
| `0x41` | Consensus error | Contact network administrator |
| `0x42` | Network error | Check network connection |

## Getting Error Details

When a transaction fails, you can get detailed error information by:

1. Checking the transaction receipt
2. Using the `eth_getTransactionReceipt` RPC method
3. Checking the block explorer

## Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "execution reverted",
    "data": {
      "errorCode": "0x20",
      "errorMessage": "VM execution error: revert"
    }
  },
  "id": 1
}
```

## Best Practices

1. Always check gas limits before sending transactions
2. Verify account balances before transactions
3. Use proper error handling in your applications
4. Check transaction receipts for detailed error information
5. Monitor transaction status on the block explorer

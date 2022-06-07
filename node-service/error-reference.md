---
description: >-
  Learn about the standard JSON-RPC error codes and Particle Network's custom
  error codes
---

# Error Reference

For JSON-RPC-specific errors, our API returns a `200` with the JSON-RPC error in the JSON response.

### Example Error Response

```typescript
{
  "jsonrpc": "2.0",
  "chainId": 101, // optional; only chains's rpc returns this
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid params: invalid length 63, expected a 0x-prefixed, padded, hex-encoded hash with length 64."
  }
}
```

### JSON-RPC Error Codes

| Code   | Possible Return Message        | Description                                                                                           |
| ------ | ------------------------------ | ----------------------------------------------------------------------------------------------------- |
| 10001  | System error                   | Internal JSON-RPC error.                                                                              |
| 10002  | Invalid parameters             | Invalid method parameter(s).                                                                          |
| 40101  | User not exists                |                                                                                                       |
| 40102  | Authentication failed          | Invalid project ID or invalid project key.                                                            |
| 40103  | Address not exists             |                                                                                                       |
| 40104  | Insufficient funds             |                                                                                                       |
| 40105  | EIP1559: Invalid fee setting   |                                                                                                       |
| 40106  | Nonce error                    |                                                                                                       |
| 40107  | Cancel the biggest nonce first | Cancel local status transaction must from the biggest to the smallest.                                |
| 40108  | Transaction confirmed          | Can not speed up or cancel a confirmed transaction.                                                   |
| 40201  | Transaction Error              |                                                                                                       |
| -32700 | Parse error                    | Invalid JSON was received by the server. An error occurred on the server while parsing the JSON text. |
| -32600 | Invalid request                | The JSON sent is not a valid Request object.                                                          |
| -32601 | Method not found               | The method does not exist/is not available.                                                           |
| -32602 | Invalid parameters             | Invalid method parameter(s).                                                                          |
| -32603 | Internal error                 | Internal JSON-RPC error.                                                                              |
| -32000 | Server error                   |                                                                                                       |

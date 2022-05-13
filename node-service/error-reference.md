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
    "jsonrpc":"2.0",
    "error":{
        "code":-32602,
        "message": "Invalid params: invalid length 63, expected a 0x-prefixed, padded, hex-encoded hash with length 64."
    },
    "id":1
}
```

### JSON-RPC Error Codes

| Code             | Possible Return Message | Description                                                                                           |
| ---------------- | ----------------------- | ----------------------------------------------------------------------------------------------------- |
| 10006            | Authentication failed   | Invalid project ID or invalid project key.                                                            |
| -32700           | Parse error             | Invalid JSON was received by the server. An error occurred on the server while parsing the JSON text. |
| -32600           | Invalid request         | The JSON sent is not a valid Request object.                                                          |
| -32601           | Method not found        | The method does not exist/is not available.                                                           |
| -32602           | Invalid parameters      | Invalid method parameter(s).                                                                          |
| -32603           | Internal error          | Internal JSON-RPC error.                                                                              |
| -32000           | Server error            |                                                                                                       |
| -32001 to -32099 | Parity errors           |                                                                                                       |

---
description: All of The Solana Official JSON-RPC API methods that are supported.
---

# Standard RPC

## Example：getBalance

> Returns the balance of the account of provided Pubkey

**Parameters:**

* `<string>` - Pubkey of account to query, as base-58 encoded string
* `<object>` - (optional) [Commitment](https://docs.solana.com/developing/clients/jsonrpc-api#configuring-state-commitment)

**Results:**

* `RpcResponse<u64>` - RpcResponse JSON object with `value` field set to the balance

Request Example：

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'getBalance',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network/solana/rpc' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"getBalance","params":["8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ"]}
'
```
{% endtab %}
{% endtabs %}

Response Example：

```typescript
{
    "jsonrpc": "2.0",
    "result": {
        "context": {
            "slot": 133898617
        },
        "value": 3467728040
    },
    "id": 0
}
```

## [👉 ](https://particle.network/#login)[More APIs](https://docs.solana.com/developing/clients/jsonrpc-api)

# Solana API

The Solana API allows applications to connect to a Solana node that is part of the Solana blockchain. Developers can interact with on-chain data and send different types of transactions to the network by utilizing the endpoints provided by the API.&#x20;

The Solana API follows a JSON-RPC 2.0 standard. We extend a field named **chainId** to specify which Solana network we use.&#x20;

| chainId Value（int） | Solana Network |   |
| ------------------ | -------------- | - |
| 101                | Mainnet Beta   |   |
| 102                | Testnet        |   |
| 103                | Devnet         |   |

So the entire JSON RPC request body would look like below：

```json5
// Solana get-balance request body
{
    "chainId": 103, // Devnet
    "jsonrpc": "2.0",
    "id": 0,
    "method": "getBalance",
    "params": ["83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri"]
}
```

### Standard RPC

The Solana API support **all methods** of The Solana Official.

You can use our API just like you use [The Official API](https://docs.solana.com/developing/clients/jsonrpc-api), but our API is faster 🚀

{% hint style="info" %}
The difference is that we use the **chainId** field to specify the network, and The Official uses different URL.
{% endhint %}

{% content-ref url="standard-rpc.md" %}
[standard-rpc.md](standard-rpc.md)
{% endcontent-ref %}

### Enhanced RPC

The Solana API offers several enhanced API methods on top of the existing official calls.&#x20;

The Enhanced API is easy and powerful which can speed up your application building and save your time.

{% content-ref url="enhanced-rpc.md" %}
[enhanced-rpc.md](enhanced-rpc.md)
{% endcontent-ref %}

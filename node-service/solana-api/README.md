# Solana API

### Endpoint

{% hint style="info" %}
**https://api.particle.network/solana/rpc**
{% endhint %}

### Structure

The Solana API allows applications to connect to a Solana node that is part of the Solana blockchain. Developers can interact with on-chain data and send different types of transactions to the network by utilizing the endpoints provided by the API.

The Solana API follows a JSON-RPC 2.0 standard. We extended a field named **chainId** to specify which Solana network we use.

| chainId Value (int) | Solana Network |
| ------------------- | -------------- |
| 101                 | Mainnet Beta   |
| 102                 | Testnet        |
| 103                 | Devnet         |

So the entire JSON RPC request body will look as follows：

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

The Solana API supports **all methods** of Solana.

You can use our API just like you use [their official API](https://docs.solana.com/developing/clients/jsonrpc-api), but ours is faster 🚀

{% hint style="info" %}
The difference is that we use the **chainId** field to specify the network, and Solana's official API uses different URLs.
{% endhint %}

{% content-ref url="../solana/rpc.md" %}
[rpc.md](../solana/rpc.md)
{% endcontent-ref %}

### Enhanced RPC

The Solana API offers several enhanced API methods on top of existing official calls.

The Enhanced API is easy and powerful, which makes app-building faster and saves your time.

{% content-ref url="enhanced-rpc.md" %}
[enhanced-rpc.md](enhanced-rpc.md)
{% endcontent-ref %}

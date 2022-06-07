# EVM Chains API

### Endpoint

{% hint style="info" %}
**https://api.particle.network/evm-chain/rpc**
{% endhint %}

### Structure

The EVM Chains API allows applications to connect to a EVM chain node that is part of the EVM blockchain. Developers can interact with on-chain data and send different types of transactions to the network by utilizing the endpoints provided by the API.

The EVM Chains API follows a JSON-RPC 2.0 standard. We extended a field named **chainId** to specify which Solana network we use.

| chainId Value (int) | EVM Chains                  |
| ------------------- | --------------------------- |
| 1                   | Ethereum Mainnet            |
| 42                  | Ethereum Testnet(Kovan)     |
| 56                  | Binance Smart Chain Mainnet |
| 97                  | Binance Smart Chain Testnet |
| 137                 | Polygon Mainnet             |
| 80001               | Polygon Testnet(Mumbai)     |
| 43114               | Avalanche Mainnet(C-Chain)  |
| 43113               | Avalanche Testnet(Fuji)     |
| 250                 | Fantom Mainnet(Opera)       |
| 4002                | Fantom Testnet              |
| 42161               | Arbitrum Mainnet(One)       |
| 421611              | Arbitrum Testnet(Rinkeby)   |
| 1284                | Moonbeam                    |
| 1285                | Moonriver                   |
| 1287                | Moonbase Alpha              |
| 1666600000          | Harmony Mainnet             |
| 1666700000          | Harmony Testnet             |
| 10                  | Optimism Mainnet            |
| 69                  | Optimism Testnet(Kovan)     |
| 128                 | Huobi ECO Chain Mainnet     |
| 256                 | Huobi ECO Chain Testnet     |
| 1313161554          | Aurora Mainnet              |
| 1313161555          | Aurora Testnet              |

### Standard RPC

{% content-ref url="standard-rpc.md" %}
[standard-rpc.md](standard-rpc.md)
{% endcontent-ref %}

### Enhanced RPC

The EVM Chains API offers several enhanced API methods on top of existing official calls.

The Enhanced API is easy and powerful, which makes app-building faster and saves your time.

{% content-ref url="enhanced-rpc.md" %}
[enhanced-rpc.md](enhanced-rpc.md)
{% endcontent-ref %}

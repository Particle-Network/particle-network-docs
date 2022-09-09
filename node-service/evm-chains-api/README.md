# EVM Chains API

### Endpoint

{% hint style="info" %}
**https://api.particle.network/evm-chain/rpc**
{% endhint %}

### Structure

The EVM Chains API allows applications to connect to a EVM chain node that is part of the EVM blockchain. Developers can interact with on-chain data and send different types of transactions to the network by utilizing the endpoints provided by the API.

The EVM Chains API follows a JSON-RPC 2.0 standard. We extended a field named **chainId** to specify which network we use.

| chainId Value (int) | chainName Value(string) | EVM Chains                  |
| ------------------- | ----------------------- | --------------------------- |
| 1                   | ethereum                | Ethereum Mainnet            |
| 3                   | ethereum                | Ethereum Ropsten            |
| 4                   | ethereum                | Ethereum Rinkeby            |
| 5                   | ethereum                | Ethereum Goerli             |
| 56                  | bsc                     | Binance Smart Chain Mainnet |
| 97                  | bsc                     | Binance Smart Chain Testnet |
| 137                 | polygon                 | Polygon Mainnet             |
| 80001               | polygon                 | Polygon Testnet(Mumbai)     |
| 43114               | avalanche               | Avalanche Mainnet(C-Chain)  |
| 43113               | avalanche               | Avalanche Testnet(Fuji)     |
| 250                 | fantom                  | Fantom Mainnet(Opera)       |
| 4002                | fantom                  | Fantom Testnet              |
| 42161               | arbitrum                | Arbitrum Mainnet(One)       |
| 421611              | arbitrum                | Arbitrum Testnet(Rinkeby)   |
| 1284                | moonbeam                | Moonbeam                    |
| 1285                | moonriver               | Moonriver                   |
| 1287                | moonbeam/moonriver      | Moonbase Alpha              |
| 1666600000          | harmony                 | Harmony Mainnet             |
| 1666700000          | harmony                 | Harmony Testnet             |
| 10                  | optimism                | Optimism Mainnet            |
| 69                  | optimism                | Optimism Testnet(Kovan)     |
| 128                 | heco                    | Huobi ECO Chain Mainnet     |
| 256                 | heco                    | Huobi ECO Chain Testnet     |
| 1313161554          | aurora                  | Aurora Mainnet              |
| 1313161555          | aurora                  | Aurora Testnet              |
| 10                  | optimism                | Optimism One Mainnet        |
| 69                  | optimism                | Optimism Testnet Kovan      |
| 321                 | kcc                     | KCC Mainnet                 |
| 322                 | kcc                     | KCC Testnet                 |
| 210425              | platon                  | PlatON Mainnet              |
| 2203181             | platon                  | PlatON Testnet              |

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

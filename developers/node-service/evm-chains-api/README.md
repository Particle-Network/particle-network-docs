# EVM Chains API

### Endpoint

{% hint style="info" %}
**https://rpc.particle.network/evm-chain**
{% endhint %}

### Structure

The EVM Chains API allows applications to connect to a EVM chain node that is part of the EVM blockchain. Developers can interact with on-chain data and send different types of transactions to the network by utilizing the endpoints provided by the API.

The EVM Chains API follows a JSON-RPC 2.0 standard. We extended a field named **chainId** to specify which network we use.

| chainId Value (int) | chainName Value(string) | EVM Chains                  |
| ------------------- | ----------------------- | --------------------------- |
| 1                   | ethereum                | Ethereum Mainnet            |
| 5                   | ethereum                | Ethereum Goerli             |
| 11155111            | ethereum                | Ethereum Sepolia            |
| 56                  | bsc                     | Binance Smart Chain Mainnet |
| 97                  | bsc                     | Binance Smart Chain Testnet |
| 137                 | polygon                 | Polygon Mainnet             |
| 80001               | polygon                 | Polygon Testnet(Mumbai)     |
| 43114               | avalanche               | Avalanche Mainnet(C-Chain)  |
| 43113               | avalanche               | Avalanche Testnet(Fuji)     |
| 250                 | fantom                  | Fantom Mainnet(Opera)       |
| 4002                | fantom                  | Fantom Testnet              |
| 42161               | arbitrum                | Arbitrum One                |
| 42170               | arbitrum                | Arbitrum Nova               |
| 421613              | arbitrum                | Arbitrum Goerli             |
| 1284                | moonbeam                | Moonbeam                    |
| 1285                | moonriver               | Moonriver                   |
| 1287                | moonbeam/moonriver      | Moonbase Alpha              |
| 1666600000          | harmony                 | Harmony Mainnet             |
| 1666700000          | harmony                 | Harmony Testnet             |
| 128                 | heco                    | Huobi ECO Chain Mainnet     |
| 256                 | heco                    | Huobi ECO Chain Testnet     |
| 1313161554          | aurora                  | Aurora Mainnet              |
| 1313161555          | aurora                  | Aurora Testnet              |
| 10                  | optimism                | Optimism Mainnet            |
| 420                 | optimism                | Optimism Testnet Goerli     |
| 321                 | kcc                     | KCC Mainnet                 |
| 322                 | kcc                     | KCC Testnet                 |
| 210425              | platon                  | PlatON Mainnet              |
| 2206132             | platon                  | PlatON Testnet              |
| 728126428           | tron                    | Tron Mainnet                |
| 2494104990          | tron                    | Tron Shasta                 |
| 3448148188          | tron                    | Tron Nile                   |
| 66                  | okc                     | OKC Mainnet                 |
| 65                  | okc                     | OKC Testnet                 |
| 108                 | thundercore             | ThunderCore Mainnet         |
| 18                  | thundercore             | ThunderCore Testnet         |
| 25                  | cronos                  | Cronos Mainnet              |
| 338                 | cronos                  | Cronos Testnet              |
| 42262               | oasisemerald            | OasisEmerald Mainnet        |
| 42261               | oasisemerald            | OasisEmerald Testnet        |
| 100                 | gnosis                  | Gnosis Mainnet              |
| 10200               | gnosis                  | Gnosis Testnet              |
| 42220               | celo                    | Celo Mainnet                |
| 44787               | celo                    | Celo Testnet                |
| 8217                | klaytn                  | Klaytn Mainnet              |
| 1001                | klaytn                  | Klaytn Testnet              |
| 324                 | zksync                  | zkSync Era Mainnet          |
| 280                 | zksync                  | zkSync Era Testnet          |
| 1088                | metis                   | Metis Mainnet               |
| 599                 | metis                   | Metis Testnet               |
| 534353              | scroll                  | Scroll Alpha Testnet        |
| 1030                | confluxespace           | Conflux eSpace Mainnet      |
| 71                  | confluxespace           | Conflux eSpace Testnet      |
| 22776               | mapprotocol             | MAP Protocol Mainnet        |
| 212                 | mapprotocol             | MAP Protocol Testnet        |
| 1101                | polygonzkevm            | Polygon zkEVM Mainnet       |
| 1442                | polygonzkevm            | Polygon zkEVM Testnet       |
| 84531               | base                    | Base Goerli Testnet         |

### Standard RPC

{% content-ref url="standard-rpc.md" %}
[standard-rpc.md](standard-rpc.md)
{% endcontent-ref %}

### Enhanced RPC

The EVM Chains API offers several enhanced API methods on top of existing official calls.

The Enhanced API is easy and powerful, which makes app-building faster and saves your time.

{% content-ref url="enhanced-rpc/" %}
[enhanced-rpc](enhanced-rpc/)
{% endcontent-ref %}

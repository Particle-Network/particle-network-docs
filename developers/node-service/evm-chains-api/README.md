# EVM Chains API

### Endpoint

{% hint style="info" %}
**https://rpc.particle.network/evm-chain**
{% endhint %}

{% hint style="info" %}
We support batch request, and the maximum length of the batch array is **100**
{% endhint %}

### Structure

The EVM Chains API allows applications to connect to an EVM chain node that is part of the EVM blockchain. Developers can interact with on-chain data and send different types of transactions to the network by utilizing the endpoints provided by the API.

The EVM Chains API follows a JSON-RPC 2.0 standard. We extended a field named **chainId** to specify which network we use.

<mark style="color:red;">chainName is Case Insensitive.</mark>

<table><thead><tr><th width="197">chainId Value (int)</th><th>chainName Value(string)</th><th>EVM Chains</th></tr></thead><tbody><tr><td>1</td><td>ethereum</td><td>Ethereum Mainnet</td></tr><tr><td>5</td><td>ethereum</td><td>Ethereum Goerli</td></tr><tr><td>11155111</td><td>ethereum</td><td>Ethereum Sepolia</td></tr><tr><td>17000</td><td>ethereum</td><td>Ethereum Holesky</td></tr><tr><td>56</td><td>bsc</td><td>Binance Smart Chain Mainnet</td></tr><tr><td>97</td><td>bsc</td><td>Binance Smart Chain Testnet</td></tr><tr><td>204</td><td>opbnb</td><td>opBNB Mainnet</td></tr><tr><td>5611</td><td>opbnb</td><td>opBNB Testnet</td></tr><tr><td>137</td><td>polygon</td><td>Polygon Mainnet</td></tr><tr><td>80001</td><td>polygon</td><td>Polygon Testnet(Mumbai)</td></tr><tr><td>43114</td><td>avalanche</td><td>Avalanche Mainnet(C-Chain)</td></tr><tr><td>43113</td><td>avalanche</td><td>Avalanche Testnet(Fuji)</td></tr><tr><td>250</td><td>fantom</td><td>Fantom Mainnet(Opera)</td></tr><tr><td>4002</td><td>fantom</td><td>Fantom Testnet</td></tr><tr><td>42161</td><td>arbitrum</td><td>Arbitrum One</td></tr><tr><td>42170</td><td>arbitrum</td><td>Arbitrum Nova</td></tr><tr><td>421613</td><td>arbitrum</td><td>Arbitrum Goerli</td></tr><tr><td>421614</td><td>arbitrum</td><td>Arbitrum Sepolia</td></tr><tr><td>1284</td><td>moonbeam</td><td>Moonbeam</td></tr><tr><td>1285</td><td>moonriver</td><td>Moonriver</td></tr><tr><td>1287</td><td>moonbeam/moonriver</td><td>Moonbase Alpha</td></tr><tr><td>1666600000</td><td>harmony</td><td>Harmony Mainnet</td></tr><tr><td>1666700000</td><td>harmony</td><td>Harmony Testnet</td></tr><tr><td>128</td><td>heco</td><td>Huobi ECO Chain Mainnet</td></tr><tr><td>1313161554</td><td>aurora</td><td>Aurora Mainnet</td></tr><tr><td>1313161555</td><td>aurora</td><td>Aurora Testnet</td></tr><tr><td>10</td><td>optimism</td><td>Optimism Mainnet</td></tr><tr><td>420</td><td>optimism</td><td>Optimism Testnet Goerli</td></tr><tr><td>11155420</td><td>optimism</td><td>Optimism Testnet Sepolia</td></tr><tr><td>321</td><td>kcc</td><td>KCC Mainnet</td></tr><tr><td>322</td><td>kcc</td><td>KCC Testnet</td></tr><tr><td>210425</td><td>platon</td><td>PlatON Mainnet</td></tr><tr><td>2206132</td><td>platon</td><td>PlatON Testnet</td></tr><tr><td>728126428</td><td>tron</td><td>Tron Mainnet</td></tr><tr><td>2494104990</td><td>tron</td><td>Tron Shasta</td></tr><tr><td>3448148188</td><td>tron</td><td>Tron Nile</td></tr><tr><td>66</td><td>okc</td><td>OKTC Mainnet</td></tr><tr><td>65</td><td>okc</td><td>OKTC Testnet</td></tr><tr><td>195</td><td>okbc</td><td>X1 Testnet</td></tr><tr><td>108</td><td>thundercore</td><td>ThunderCore Mainnet</td></tr><tr><td>18</td><td>thundercore</td><td>ThunderCore Testnet</td></tr><tr><td>25</td><td>cronos</td><td>Cronos Mainnet</td></tr><tr><td>338</td><td>cronos</td><td>Cronos Testnet</td></tr><tr><td>42262</td><td>oasisemerald</td><td>OasisEmerald Mainnet</td></tr><tr><td>42261</td><td>oasisemerald</td><td>OasisEmerald Testnet</td></tr><tr><td>100</td><td>gnosis</td><td>Gnosis Mainnet</td></tr><tr><td>10200</td><td>gnosis</td><td>Gnosis Testnet</td></tr><tr><td>42220</td><td>celo</td><td>Celo Mainnet</td></tr><tr><td>44787</td><td>celo</td><td>Celo Testnet</td></tr><tr><td>8217</td><td>klaytn</td><td>Klaytn Mainnet</td></tr><tr><td>1001</td><td>klaytn</td><td>Klaytn Testnet</td></tr><tr><td>324</td><td>zksync</td><td>zkSync Era Mainnet</td></tr><tr><td>280</td><td>zksync</td><td>zkSync Era Testnet</td></tr><tr><td>300</td><td>zksync</td><td>zkSync Era Sepolia Testnet</td></tr><tr><td>1088</td><td>metis</td><td>Metis Mainnet</td></tr><tr><td>599</td><td>metis</td><td>Metis Testnet</td></tr><tr><td>534352</td><td>scroll</td><td>Scroll Mainnet</td></tr><tr><td>534351</td><td>scroll</td><td>Scroll Sepolia Testnet</td></tr><tr><td>1030</td><td>confluxespace</td><td>Conflux eSpace Mainnet</td></tr><tr><td>71</td><td>confluxespace</td><td>Conflux eSpace Testnet</td></tr><tr><td>22776</td><td>mapprotocol</td><td>MAP Protocol Mainnet</td></tr><tr><td>212</td><td>mapprotocol</td><td>MAP Protocol Testnet</td></tr><tr><td>1101</td><td>polygonzkevm</td><td>Polygon zkEVM Mainnet</td></tr><tr><td>1442</td><td>polygonzkevm</td><td>Polygon zkEVM Testnet</td></tr><tr><td>8453</td><td>base</td><td>Base Mainnet</td></tr><tr><td>84531</td><td>base</td><td>Base Goerli Testnet</td></tr><tr><td>84532</td><td>84531</td><td>Base Sepolia Testnet</td></tr><tr><td>59144</td><td>linea</td><td>Linea Mainnet</td></tr><tr><td>59140</td><td>linea</td><td>Linea Goerli Testnet</td></tr><tr><td>5000</td><td>mantle</td><td>Mantle Mainnet</td></tr><tr><td>5001</td><td>mantle</td><td>Mantle Testnet</td></tr><tr><td>9980</td><td>combo</td><td>Combo Mainnet</td></tr><tr><td>91715</td><td>combo</td><td>Combo Testnet</td></tr><tr><td>167007</td><td>taiko</td><td>Taiko Jolnir Testnet</td></tr><tr><td>167008</td><td>taiko</td><td>Taiko Katla Testnet</td></tr><tr><td>7777777</td><td>zora</td><td>Zora Mainnet</td></tr><tr><td>999</td><td>zora</td><td>Zora Mainnet</td></tr><tr><td>424</td><td>pgn</td><td>PGN Mainnet</td></tr><tr><td>58008</td><td>pgn</td><td>PGN Testnet</td></tr><tr><td>1482601649</td><td>nebula</td><td>SKALE Nebula Mainnet</td></tr><tr><td>169</td><td>manta</td><td>Manta Mainnet</td></tr><tr><td>3441005</td><td>manta</td><td>Manta Testnet</td></tr><tr><td>12015</td><td>readon</td><td>ReadON Testnet</td></tr><tr><td>2222</td><td>kava</td><td>Kava Mainnet</td></tr><tr><td>2221</td><td>kava</td><td>Kave Testnet</td></tr><tr><td>17777</td><td>eosevm</td><td>EOS EVM Mainnet</td></tr><tr><td>15557</td><td>eosevm</td><td>EOS EVM Testnet</td></tr><tr><td>7001</td><td>zetachain</td><td>ZetaChain Testnet</td></tr><tr><td>12008</td><td>lumoz</td><td>Lumoz zkEVM Testnet</td></tr><tr><td>1261120</td><td>astarzkevm</td><td>Astar zkEVM Testnet</td></tr><tr><td>28122024</td><td>ancient8</td><td>Ancient8 Testnet V2</td></tr><tr><td>168587773</td><td>blast</td><td>Blast Sepolia</td></tr><tr><td>80085</td><td>berachain</td><td>Berachain Testnet Artio</td></tr><tr><td>42766</td><td>zkfair</td><td>ZKFair Mainnet</td></tr><tr><td>43851</td><td>zkfair</td><td>ZKFair Testnet</td></tr><tr><td>686868</td><td>merlin</td><td>Merlin Chain Testnet</td></tr><tr><td>28206</td><td>lumibit</td><td>LumiBit Testnet</td></tr></tbody></table>

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

# Tokens & NFTs

### Support by Networks

| Networks (Mainnet & Testnet) |         Transactions        |            Tokens           |              NFTs              |
| ---------------------------- | :-------------------------: | :-------------------------: | :----------------------------: |
| Ethereum                     |              ✅              |              ✅              |                ✅               |
| BSC (BNB Smart Chain)        |              ✅              |              ✅              |                ✅               |
| Polygon                      |              ✅              |              ✅              |                ✅               |
| Avalanche (C-Chain)          |              ✅              |              ✅              |                ✅               |
| Fantom                       |              ✅              |              ✅              |                ✅               |
| Arbitrum                     | <p>✅<br>❌ Arbitrum Nova</p> | <p>✅<br>❌ Arbitrum Nova</p> | <p>✅</p><p>❌ Arbitrum Nova</p> |
| Harmony                      |              ✅              |              ✅              |                ✅               |
| Moonbeam/Moonriver           |              ✅              |              ✅              |                ✅               |
| Heco                         |              ✅              |              ✅              |                ❌               |
| Aurora                       |              ✅              |              ✅              |                ✅               |
| Optimism                     |              ✅              |              ✅              |  <p>✅ Mainnet<br>❌ Testnet</p> |
| KCC                          |              ✅              |              ✅              |                ❌               |
| PlatON                       |              ✅              |              ✅              |                ❌               |
| Tron                         |              ✅              |              ✅              |                ❌               |
| OKC                          |              ✅              |              ✅              |                ❌               |
| ThunderCore                  |              ✅              |              ✅              |                ❌               |
| Cronos                       |              ✅              |              ✅              |                ✅               |
| Oasis Emerald                |              ✅              |              ✅              |  <p>✅ Mainnet<br>❌ Testnet</p> |
| Gnosis (xDAI)                |              ✅              |              ✅              |                ❌               |
| Celo                         |              ✅              |              ✅              |                ❌               |
| Klaytn                       |              ✅              |              ✅              |                ❌               |
| Scroll                       |              ✅              |              ✅              |                ❌               |
| ZkSync v2                    |              ❌              |              ❌              |                ❌               |
| Metis                        |              ✅              |              ✅              |                ✅               |

### 🔥 particle\_getTokensAndNFTs

> Get token and nfts by giving an address

**Parameters:**

* `<string>` - account address
* `<array[string]>` - token addresses (optional)

**Results:**

* `<[object]>` - a JSON object array containing:
  * `native: <string>`, native balance
  * `tokens: <[object]>`, an object array containing token detail
  * `nfts: <[object]>`, an object array containing NFT detail

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getTokensAndNFTs',
        params: ['0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8'],
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
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":1,"jsonrpc":"2.0","id":1,"method":"particle_getTokensAndNFTs","params":["0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8"]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```json
{
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": {
        "native": "101067001639555898", 
        "tokens": [
            {
                "decimals": 18, 
                "amount": "1000000000000000000", 
                "address": "0x4F96Fe3b7A6Cf9725f59d353F723c1bDb64CA6Aa", 
                "name": "Dai Stablecoin", 
                "symbol": "DAI", 
                "image": ""
            }, 
            {
                "decimals": 18, 
                "amount": "1000000000000000", 
                "address": "0xd0A1E359811322d97991E03f863a0C30C2cF029C", 
                "name": "Wrapped Ether", 
                "symbol": "WETH", 
                "image": ""
            }, 
            {
                "decimals": 18, 
                "amount": "10000000000000000000", 
                "address": "0xa36085F69e2889c224210F603D836748e7dC0088", 
                "name": "ChainLink Token", 
                "symbol": "LINK", 
                "image": ""
            }
        ], 
        "nfts": [
            {
                "address": "0x06012c8cf97BEaD5deAe237070F9587f8E7A266d", 
                "isSemiFungible": false, 
                "tokenId": "362330", 
                "tokenBalance": "1",
                "tokenURI": "https://tokenuri.com", 
                "name": "CryptoKitties: Core", 
                "symbol": "CK", 
                "image": "", 
                "data": {
                    "name": null, 
                    "description": "Heyo! I'm Kitty #362330. I've never told anyone this, but I once kissed a dog. I once kissed a rabbit. I don't like to talk about it. Can't wait to eat ice cream with you!", 
                    "image": "https://img.cryptokitties.co/0x06012c8cf97bead5deae237070f9587f8e7a266d/362330.png", 
                    "animation_url": null, 
                    "external_url": null, 
                    "attributes": [
                        {
                            "type": "colorprimary", 
                            "description": "greymatter", 
                            "kittyId": 362330
                        }, 
                        {
                            "type": "coloreyes", 
                            "description": "mintgreen", 
                            "kittyId": 362330
                        }, 
                        {
                            "type": "body", 
                            "description": "ragamuffin", 
                            "kittyId": 362330
                        }, 
                        {
                            "type": "colorsecondary", 
                            "description": "barkbrown", 
                            "position": 273, 
                            "kittyId": 38911
                        }, 
                        {
                            "type": "mouth", 
                            "description": "saycheese", 
                            "position": 19, 
                            "kittyId": 11564
                        }, 
                        {
                            "type": "pattern", 
                            "description": "spock", 
                            "position": 547, 
                            "kittyId": 96322
                        }, 
                        {
                            "type": "eyes", 
                            "description": "crazy", 
                            "kittyId": 362330
                        }, 
                        {
                            "type": "colortertiary", 
                            "description": "kittencream", 
                            "kittyId": 362330
                        }, 
                        {
                            "type": "secret", 
                            "description": "se7", 
                            "position": -1, 
                            "kittyId": 362330
                        }, 
                        {
                            "type": "purrstige", 
                            "description": "pu12", 
                            "position": -1, 
                            "kittyId": 362330
                        }
                    ], 
                    "owner": "0xb3d8ba776631f0ebcebaf16182fb733f894b68b7"
                }
            }
        ]
    }, 
    "chainId": 42
}
```

### 🔥 particle\_getTokens

> Get tokens by giving an address

**Parameters:**

* `<string>` - account address
* `<array[string]>` - token addresses (optional)

**Results:**

* `<[object]>` - a JSON object array containing:
  * `native: <string>`, native balance
  * `tokens: <[object]>`, an object array containing token detail

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getTokens',
        params: ['0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8'],
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
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":1,"jsonrpc":"2.0","id":1,"method":"particle_getTokensAndNFTs","params":["0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8"]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```json
{
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": {
        "native": "101067001639555898", 
        "tokens": [
            {
                "decimals": 18, 
                "amount": "1000000000000000000", 
                "address": "0x4F96Fe3b7A6Cf9725f59d353F723c1bDb64CA6Aa", 
                "name": "Dai Stablecoin", 
                "symbol": "DAI", 
                "image": ""
            }, 
            {
                "decimals": 18, 
                "amount": "1000000000000000", 
                "address": "0xd0A1E359811322d97991E03f863a0C30C2cF029C", 
                "name": "Wrapped Ether", 
                "symbol": "WETH", 
                "image": ""
            }, 
            {
                "decimals": 18, 
                "amount": "10000000000000000000", 
                "address": "0xa36085F69e2889c224210F603D836748e7dC0088", 
                "name": "ChainLink Token", 
                "symbol": "LINK", 
                "image": ""
            }
        ]
    }, 
    "chainId": 42
}
```

### 🔥 particle\_getNFTs

> Get nfts by giving an address

**Parameters:**

* `<string>` - account address
* `<array[string]>` - nft contract addresses to filter (optional; default get all nfts)

**Results:**

* `<array[object]>` - a JSON array

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getNFTs',
        params: ['0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8'],
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
{% endtabs %}

Response example:

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "address": "0x06012c8cf97BEaD5deAe237070F9587f8E7A266d",
            "isSemiFungible": false,
            "tokenId": "362330",
            "tokenBalance": "1",
            "tokenURI": "https://tokenuri.com",
            "name": "CryptoKitties: Core",
            "symbol": "CK",
            "image": "",
            "data": {
                "name": null,
                "description": "Heyo! I'm Kitty #362330. I've never told anyone this, but I once kissed a dog. I once kissed a rabbit. I don't like to talk about it. Can't wait to eat ice cream with you!",
                "image": "https://img.cryptokitties.co/0x06012c8cf97bead5deae237070f9587f8e7a266d/362330.png",
                "animation_url": null,
                "external_url": null,
                "attributes": [
                    {
                        "type": "colorprimary",
                        "description": "greymatter",
                        "kittyId": 362330
                    },
                    {
                        "type": "coloreyes",
                        "description": "mintgreen",
                        "kittyId": 362330
                    },
                    {
                        "type": "body",
                        "description": "ragamuffin",
                        "kittyId": 362330
                    },
                    {
                        "type": "colorsecondary",
                        "description": "barkbrown",
                        "position": 273,
                        "kittyId": 38911
                    },
                    {
                        "type": "mouth",
                        "description": "saycheese",
                        "position": 19,
                        "kittyId": 11564
                    },
                    {
                        "type": "pattern",
                        "description": "spock",
                        "position": 547,
                        "kittyId": 96322
                    },
                    {
                        "type": "eyes",
                        "description": "crazy",
                        "kittyId": 362330
                    },
                    {
                        "type": "colortertiary",
                        "description": "kittencream",
                        "kittyId": 362330
                    },
                    {
                        "type": "secret",
                        "description": "se7",
                        "position": -1,
                        "kittyId": 362330
                    },
                    {
                        "type": "purrstige",
                        "description": "pu12",
                        "position": -1,
                        "kittyId": 362330
                    }
                ],
                "owner": "0xb3d8ba776631f0ebcebaf16182fb733f894b68b7"
            }
        }
    ],
    "chainId": 42
}
```

### 🔥 particle\_crosschain\_getNFTs

> Get some accounts NFTs cross multiple chains

**Parameters:**

* `<[string]>` - accounts addresses
* `<[int64]>` - chain ids
* `<[string]>: optional` - contracts addresses to filter

**Results:**

* `<[object]>` - a JSON object array containing NFT details

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_crosschain_getNFTs',
        params: [
                ['0x6a2C3C4C7169d69A67ae2251c7D765Ac79A4967e', '0xf584F8728B874a6a5c7A8d4d387C9aae9172D621'],
                [1, 4, 137, 80001]
            ],
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
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"id": 1, "chainId": 1, "method": "particle_crosschain_getNFTs", "params": [["0x6a2C3C4C7169d69A67ae2251c7D765Ac79A4967e", "0xf584F8728B874a6a5c7A8d4d387C9aae9172D621"], [1, 4, 137, 80001]]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "address": "0x495f947276749Ce646f68AC8c248420045cb7b5e",
      "isSemiFungible": false,
      "tokenId": "11927049007610738573542686684622477446347219566966091061779065351365187338241",
      "tokenBalance": "1",
      "tokenURI": "https://api.opensea.io/api/v1/metadata/0x495f947276749Ce646f68AC8c248420045cb7b5e/0x{id}",
      "name": "SMILE #8",
      "symbol": "OPENSTORE",
      "image": "https://lh3.googleusercontent.com/ktK1VTVk3Fj-vUz1LCY0TPCitHaZ9Rtpm59PBdTy3ngEn9ejwklwYxl8rAYnWsoiVf7CK9xYIDKU37ZmjXg5EuLvkdKZtxrNXzXqAA",
      "data": {
        "name": "SMILE #8",
        "description": "smile - a world of only 500 different smiles and unique for each personality",
        "image": "https://lh3.googleusercontent.com/ktK1VTVk3Fj-vUz1LCY0TPCitHaZ9Rtpm59PBdTy3ngEn9ejwklwYxl8rAYnWsoiVf7CK9xYIDKU37ZmjXg5EuLvkdKZtxrNXzXqAA"
      },
      "chainId": 1,
      "owner": "0x6a2C3C4C7169d69A67ae2251c7D765Ac79A4967e"
    },
    {
      "address": "0x095c1E0A6fE0075d78aD275C657Df5B07630C021",
      "isSemiFungible": false,
      "tokenId": "23",
      "tokenBalance": "1",
      "tokenURI": "http://api.nikenft.xyz/ipfs/23",
      "name": "NIKE #23",
      "symbol": "",
      "image": "https://s4.ax1x.com/2022/03/05/b05o6J.gif",
      "data": {
        "name": "NIKE #23",
        "description": "COUNTDOWN OVER. MINTING LIVE.  [https://nikenft.xyz](https://nikenft.xyz)",
        "image": "https://s4.ax1x.com/2022/03/05/b05o6J.gif",
        "external_url": "https://nikenft.xyz",
        "dna": "23"
      },
      "chainId": 1,
      "owner": "0xf584F8728B874a6a5c7A8d4d387C9aae9172D621"
    },
    {
      "address": "0x095c1E0A6fE0075d78aD275C657Df5B07630C021",
      "isSemiFungible": false,
      "tokenId": "56",
      "tokenBalance": "1",
      "tokenURI": "http://api.nike-nfts.xyz/ipfs/56",
      "name": "NIKE #56",
      "symbol": "",
      "image": "https://s4.ax1x.com/2022/03/05/b05TX9.gif",
      "data": {
        "name": "NIKE #56",
        "description": "COUNTDOWN OVER. MINTING LIVE.  [https://nikeofficial.xyz](https://nikeofficial.xyz)",
        "image": "https://s4.ax1x.com/2022/03/05/b05TX9.gif",
        "external_url": "https://nikeofficial.xyz",
        "dna": "56"
      },
      "chainId": 1,
      "owner": "0xf584F8728B874a6a5c7A8d4d387C9aae9172D621"
    }
  ],
  "chainId": 1
}
```

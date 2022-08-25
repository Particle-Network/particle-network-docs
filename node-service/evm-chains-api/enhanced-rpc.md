---
description: Simple And Powerful
---

# Enhanced RPC

### 🔥 particle\_getPrice

> Get the real-time exchange rate of the token

**Parameters:**

* `<[string]>` - address array

{% hint style="info" %}
The native token has no contract address, so it's value is **`native`**
{% endhint %}

* `<[string]>` - Array of supported currencies, the values of currencies is as follows:
  * `usd`
  * `cny`

**Results:**

* `<[object]>` - a JSON object array containing:
  * `address: <string>`, token contract address
  * `currencies: <[object]>`, currency info object

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
        chainId: 42,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getPrice',
        params: [
            [
                'native', 
                '0x7968bc6a03017eA2de509AAA816F163Db0f35148',
            ],
            [
                'usd', 
                'cny',
            ],
        ],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        },
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network/evm-chain/rpc' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":42,"jsonrpc":"2.0","id":1,"method":"particle_getPrice","params":[["native","0x7968bc6a03017eA2de509AAA816F163Db0f35148"],["usd","cny"]]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```typescript
{
    "chainId": 42,
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": [
        {
            "address": "native", 
            "currencies": [
                {
                    "type": "cny", 
                    "price": 12170.15, 
                    "marketCap": 1471838560900.835, 
                    "24hChange": -6.152514002972175, 
                    "24hVol": 134476255636.24605, 
                    "lastUpdatedAt": 1654153126
                }, 
                {
                    "type": "usd", 
                    "price": 1822.72, 
                    "marketCap": 220186784486.6231, 
                    "24hChange": -5.940275233699975, 
                    "24hVol": 20140522643.179638, 
                    "lastUpdatedAt": 1654153126
                }
            ]
        },
        {
            "address": "0x7968bc6a03017eA2de509AAA816F163Db0f35148", 
            "currencies": [
                {
                    "type": "cny", 
                    "price": 5.68, 
                    "marketCap": 9983229.811334804, 
                    "24hChange": -3.653366801443764, 
                    "24hVol": 1225668.578963783, 
                    "lastUpdatedAt": 1654173523
                }, 
                {
                    "type": "usd", 
                    "price": 0.85174, 
                    "marketCap": 1496287.4417468207, 
                    "24hChange": -3.6244889459436456, 
                    "24hVol": 183684.05277680393, 
                    "lastUpdatedAt": 1654173523
                }
            ]
        }
    ]
}
```

### 🔥 particle\_suggestedGasFees

> Get suggested gas fees for EIP-1559

**Parameters:**

* no parameters

**Results:**

* `<object>` - a JSON object containing:
  * `low: <object>`, suggested gas fees info
  * `medium: <object>`, suggested gas fees info
  * `fast: <object>`, suggested gas fees info
  * `baseFee: <string>`, current base fee

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
        chainId: 42,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_suggestedGasFees',
        params: [],
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
curl 'https://api.particle.network/evm-chain/rpc' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":42,"jsonrpc":"2.0","id":1,"method":"particle_suggestedGasFees","params":[]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 42,
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "low": {
            "maxPriorityFeePerGas": "1",
            "maxFeePerGas": "20.261906202",
            "minWaitTime": 15000,
            "maxWaitTime": 30000
        },
        "medium": {
            "maxPriorityFeePerGas": "1.5",
            "maxFeePerGas": "27.503573373",
            "minWaitTime": 15000,
            "maxWaitTime": 45000
        },
        "high": {
            "maxPriorityFeePerGas": "2",
            "maxFeePerGas": "34.745240544",                
            "minWaitTime": 15000,
            "maxWaitTime": 60000
        },
        "baseFee": "19.261906202"
    }
}
```

### 🔥 particle\_getTokensAndNFTs

> Get token and nfts by giving an address

**Parameters:**

* `<string>` - account address
* `<array[string]>` - token addresses  (optional)

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
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
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
curl 'https://api.particle.network/evm-chain/rpc' \
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
* `<array[string]>` - token addresses  (optional)

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
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
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
curl 'https://api.particle.network/evm-chain/rpc' \
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
* `<array[string]>` - nft contract addresses to filter  (optional; default get alll nfts)

**Results:**

* `<array[object]>` - a JSON array

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
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

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network/evm-chain/rpc' \
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

### 🔥 particle\_getTransactionsByAddress

> Get parsed transaction history by giving an address

**Parameters:**

* `<string>` - account address

**Results:**

* `<[object]>` - a JSON object containing:
  * `hash: <string>`, transaction hash
  * `from: <string>`, transaction send from address
  * `to: <string>`, transaction send to address
  * `value: <string>`, transaction send value
  * `data: <string>`, transaction send data
  * `gasLimit: <int64>`, transaction gas limit
  * `timestamp: <int64>`, transaction time
  * `status: <int64>`, transaction status:, the value is as below:
    * `0`, pending transaction in `TX-QUEUE`, can cancel
    * `1`, pending transaction in `TX-POOL`, can speed up or cancel
    * `3`, confirmed transaction which status is `Success`
    * `4`, confirmed transaction which status is `Fail`

when status is `0|1`, the object containing:

* `type: <int64>`, transaction type, the value is as below:
  * `0:` legacy transaction
  * `1:` EIP-2930 access list transaction
  * `2:` EIP-1559 gas fee market transaction
* `nonce: <int64>`, trancsaction nonce
* `accessList: <[]>`, EIP-2930 access list
* `maxPriorityFeePerGas: <string>`, EIP-1559 max priority fee per gas
* `maxFeePerGas: <string>`, EIP-1559 max fee per gas

when status is `3|4`, the object containing:

* `gasSpent: <string>`, transaction actual used gas
* `gasPrice: <string>`, transaction gas price
* `fees: <string>`, transaction gas fees

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
        chainId: 42,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getTransactionsByAddress',
        params: ['0x425249Cf0F2f91f488E24cF7B1AA3186748f7516'],
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
curl 'https://api.particle.network/evm-chain/rpc' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":42,"jsonrpc":"2.0","id":1,"method":"particle_getTransactionsByAddress","params":["0x425249Cf0F2f91f488E24cF7B1AA3186748f7516"]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 42,
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "chainId": "0x2a",
            "nonce": "0x9",
            "maxPriorityFeePerGas": "0x8c347c90",
            "maxFeePerGas": "0xb60535d5740",
            "gasLimit": "0x5208",
            "to": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF",
            "value": "0x16345785d8a0000",
            "data": "0x",
            "accessList": [ ],
            "hash": "0x301657f8691004172b756e198a680f1e38065899b74f41175dde206071c17008",
            "from": "0x425249Cf0F2f91f488E24cF7B1AA3186748f7516",
            "type": 2,
            "status": 1,
            "timestamp": 1653593819
        },
        {
            "chainId": "0x2a",
            "hash": "0xbe18f8fd8c0694af8fa3dbd29b53f20981269a7c08f0c0e9ae888aa863ef74c7",
            "from": "0x425249Cf0F2f91f488E24cF7B1AA3186748f7516",
            "to": "0x5F5D96fac4cca23196531A4Fa221bC309C16544E",
            "value": "0x0",
            "gasLimit": "0x37775",
            "gasSpent": "0x33600",
            "gasPrice": "0x77359400",
            "fees": "0x17ec609380000",
            "status": 3,
            "timestamp": 1630521436
        }
    ]
}
```

### 🔥 particle\_abi\_encodeFunctionCall

> Call ABI function

**Parameters:**

* `<string>` - contract address
* `<string>` - function name, the value is as below:
  * `erc20_transfer`
  * `erc20_approve`
  * `erc20_transferFrom`
  * `erc721_safeTransferFrom`
  * `erc1155_safeTransferFrom`
  * `custom_{methodName}` - any other functions which not mentioned above
* `<string>` - function params string
* `<string>: optional` - function abi json string

**Results:**

* `<string>` - function call result string

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: "particle_abi_encodeFunctionCall",
        params: [
            "0xB8c77482e45F1F44dE1745F52C74426C631bDD52", 
            "erc20_transfer", 
            [
                "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8", 
                "1000000000000000000"
            ]
        ]
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
curl 'https://api.particle.network/evm-chain/rpc' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":1,"jsonrpc":"2.0","id":1,"method":"particle_abi_encodeFunctionCall","params":["0xB8c77482e45F1F44dE1745F52C74426C631bDD52","erc20_transfer",["0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8","1000000000000000000"]]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 1,
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce80000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

### 🔥 **particle\_deserializeTransaction**

> Get transaction deserialized data with balance change

**Parameters:**

* `<string>` - account address
* `<string>` - serialized transaction string

**Results:**

* `<[object]>` - a JSON object containing:&#x20;
  * `estimatedChanges: <object>`&#x20;
    * `natives: <[object]>` - estimated native token changes
      * `address: <string>` - transaction from address
      * `nativeChange: <string>` amount changes
    * `nfts: <[object]>` - estimated NFT changes
      * `address: <string>` - contract address
      * `name: <string>`
      * `symbol: <string>`
      * `image: <string>`
      * `fromAddress: <string>` - transaction from address
      * `tokenId: <string>` - NFT token id
      * `amountChange: <string>`
    * `tokens: <[object]>` - estimated token changes
      * `address: <string>` - contraction address
      * `name: <string>` - token name
      * `symbol: <string>` - token symbol
      * `image: <string>` - token image
      * `fromAddress: <string>` - transaction from address
      * `decimals: <int64>` token decimals
      * `amountChange: <string>`
  * `data: <object>` - deserialized transaction data
    * `from: <string>`
    * `chainId: <string>`
    * `nonce: <string>`
    * `maxPriorityFeePerGas: <string>`
    * `maxFeePerGas: <string>`
    * `gasLimit: <string>`
    * `to: <string>`
    * `value: <string>`
    * `data: <string>`
    * `accessList: <[object]>`
    * `v: <string>`
    * `r: <string>`
    * `s: <string>`
    * `type: <string>` - transaction type, the value is as below:&#x20;
      * `0x0:` legacy transaction
      * `0x1:` EIP-2930 access list transaction
      * `0x2:` EIP-1559 gas fee market transaction
    * `function: <object>`
      * `name: <string>`
      * `params: <[object]>`
        * `name: <string>`
        * `value: <string>`
        * `type: <string>`

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/evm-chain/rpc', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_deserializeTransaction',
        params: [
            '0x03afc65278e6d37f23bc0b8bf4c9d61bd35edfc8',
            '0x02f8d30182030a843b9aca00850608379d4b8301122e9499ecdf17ded4fcb6c5f0fe280d21f832af464f6780b86442842e0e00000000000000000000000003afc65278e6d37f23bc0b8bf4c9d61bd35edfc8000000000000000000000000a058fb195d274afbae4dc317be362d4e96ffa1b400000000000000000000000000000000000000000000000000000000000006c1c001a0f7e0c908e4b549d24dad47f28205d3437e00b123e9b57bec95dcd61e1fd8065ca053495aaeec3740c0520e7717060a208aac0970dbc30ef3380e3ae9f81f4f6f66',
        ]
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
curl 'https://api.particle.network/evm-chain/rpc' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":1,"jsonrpc":"2.0","id":1,"method":"particle_deserializeTransaction","params":["0x03afc65278e6d37f23bc0b8bf4c9d61bd35edfc8","0x02f8d30182030a843b9aca00850608379d4b8301122e9499ecdf17ded4fcb6c5f0fe280d21f832af464f6780b86442842e0e00000000000000000000000003afc65278e6d37f23bc0b8bf4c9d61bd35edfc8000000000000000000000000a058fb195d274afbae4dc317be362d4e96ffa1b400000000000000000000000000000000000000000000000000000000000006c1c001a0f7e0c908e4b549d24dad47f28205d3437e00b123e9b57bec95dcd61e1fd8065ca053495aaeec3740c0520e7717060a208aac0970dbc30ef3380e3ae9f81f4f6f66"]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 1,
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "estimatedChanges": {
            "natives": [{
                "address": "0x03afC65278E6D37f23bC0B8Bf4C9d61Bd35edFC8",
                "nativeChange": "0"
            }],
            "nfts": [{
                "address": "0x99EcdF17DED4fCb6C5f0fE280d21f832af464f67",
                "name": "",
                "symbol": "",
                "image": "",
                "fromAddress": "0x03afC65278E6D37f23bC0B8Bf4C9d61Bd35edFC8",
                "tokenId": "1729",
                "amountChange": "-1"
            }],
            "tokens": []
        },
        "data": {
            "from": "0x03afC65278E6D37f23bC0B8Bf4C9d61Bd35edFC8",
            "chainId": "0x1",
            "nonce": "0x30a",
            "maxPriorityFeePerGas": "0x3b9aca00",
            "maxFeePerGas": "0x608379d4b",
            "gasLimit": "0x1122e",
            "to": "0x99EcdF17DED4fCb6C5f0fE280d21f832af464f67",
            "value": "0x0",
            "data": "0x42842e0e00000000000000000000000003afc65278e6d37f23bc0b8bf4c9d61bd35edfc8000000000000000000000000a058fb195d274afbae4dc317be362d4e96ffa1b400000000000000000000000000000000000000000000000000000000000006c1",
            "accessList": [],
            "v": "0x1",
            "r": "0xf7e0c908e4b549d24dad47f28205d3437e00b123e9b57bec95dcd61e1fd8065c",
            "s": "0x53495aaeec3740c0520e7717060a208aac0970dbc30ef3380e3ae9f81f4f6f66",
            "type": "0x2",
            "function": {
                "name": "safeTransferFrom",
                "params": [{
                    "name": "from_",
                    "value": "0x03afc65278e6d37f23bc0b8bf4c9d61bd35edfc8",
                    "type": "address"
                }, {
                    "name": "to_",
                    "value": "0xa058fb195d274afbae4dc317be362d4e96ffa1b4",
                    "type": "address"
                }, {
                    "name": "tokenId_",
                    "value": "1729",
                    "type": "uint256"
                }]
            }
        }
    }
}
```

### 🔥 **particle\_getLatestBlock**

> Get latest block with transaction receipts.
>
> **Parameters:**
>
> Empty array
>
> **Results:**
>
> * `result of eth_getBlockByNumber`
> * `& receipts`
>
> **Reponse Example:**
>
> ****
>
> ```json
> {
>     "jsonrpc": "2.0",
>     "id": 0,
>     "result": {
>         "difficulty": "0x2",
>         "extraData": "0xd88301010b846765746888676f312e31382e32856c696e7578000000de3b3a04d9038da607050a588616d8a1c39cb6b4190645ff6ef81ce765aeef20547cd82131367c734b58237c96bd63032b834a9467766b27c59fe1e56f3de2c53a9f4cae01",
>         "gasLimit": "0x1c9c380",
>         "gasUsed": "0x911f5",
>         "hash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>         "logsBloom": "0x00000000008100801000000020000000000008000000002000000010000000000010200000000000000082000021000010000000002000000000000000200400000000100400000100000008082100002010000000000002000000000200200000100020020200200010000042220820080180000000000000000010000000000000020000010000000002000004000000000400000000200000000000200020020001000000000000000000000000000000000200000000020000000000000000080102000000000000080000002100001100000000000000000003000020010011000000000029010000000008000100000000008000000800000000000002",
>         "miner": "0x96c5d20b2a975c050e4220be276ace4892f4b41a",
>         "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
>         "nonce": "0x0000000000000000",
>         "number": "0x14914c4",
>         "parentHash": "0x6d4809bcfe42ffba2d77e803ae77d9ecfe4819784bf78162e4cb9f92502e24cb",
>         "receiptsRoot": "0x6a64e02e8d34f7f6d17a75bb43d77212929fd6a576f9da4435918922b88fb919",
>         "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
>         "size": "0x925",
>         "stateRoot": "0xdab0f8bac71c5f6fb76c447d05d73e7f488ef1d038d29f8b6a4a5aefb5ed5010",
>         "timestamp": "0x62e79753",
>         "totalDifficulty": "0x28fd1d6",
>         "transactions": [
>             "0x4911bb832a6ad5f7d7fee77344427935478c01ab0f67981346264bdaa3bdb723",
>             "0xf9f69fb9958e9d7d76ad86c075034288a9838fa26a3a3f5bb0e8af7045292ce1",
>             "0xe381b4f6106bd1bf3fc84e2b8b388c3a7f86d2c8e058b4c57120a73ea5a5c54b",
>             "0x184c14f19c025cd0cdfc7830a6987bc8994b415ee6f210a2ef6f433d8c6dab0b",
>             "0x13109794c30b22a513cc32b10552914561a4f70b3ac39c6d01f10dd03b3bd0be",
>             "0x1f3b8cec80de721aa22603305910e67af044d3c103307b040de8b84374515a04",
>             "0xc2b08f5257988a7b16d13eb48e20b0aaeee2a4d3b44e186d851073a15221ee7b",
>             "0xb05c9adade59c6399685e82124cb0461a04bf5f3b823b962bf990bf06b1ec271",
>             "0x0fab79f027d4079ddaaf2df4aa0e55e737ae47b943eb37040dac3c200c88a903",
>             "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>             "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800"
>         ],
>         "transactionsRoot": "0x8e081b66278e186d0099e0f778543d991abb759cfb61e82e11c91653f4dfcce0",
>         "uncles": [],
>         "receipts": [
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x5208",
>                 "from": "0xaa25aa7a19f9c426e07dee59b12f944f4d9f1dd3",
>                 "gasUsed": "0x5208",
>                 "logs": [],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x978bd26d3ebb3363d6af1406da8bbc320a5d61e0",
>                 "transactionHash": "0x4911bb832a6ad5f7d7fee77344427935478c01ab0f67981346264bdaa3bdb723",
>                 "transactionIndex": "0x0",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0xa410",
>                 "from": "0x6315b7661ccb8c06e510b1cd7be06512b925bd5e",
>                 "gasUsed": "0x5208",
>                 "logs": [],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0xc1da34d47c7e3189c2cdf0a5a4e53fd34a2a8cff",
>                 "transactionHash": "0xf9f69fb9958e9d7d76ad86c075034288a9838fa26a3a3f5bb0e8af7045292ce1",
>                 "transactionIndex": "0x1",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x1bf37",
>                 "from": "0xdf742a85a8649ca38e80813466ad501b9f9b04fb",
>                 "gasUsed": "0x11b27",
>                 "logs": [
>                     {
>                         "address": "0x2cb78f0f545b3e5a265b86700344fefbbeb12b1c",
>                         "topics": [
>                             "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
>                             "0x0000000000000000000000000000000000000000000000000000000000000000",
>                             "0x000000000000000000000000df742a85a8649ca38e80813466ad501b9f9b04fb"
>                         ],
>                         "data": "0x0000000000000000000000000000000000000000000000056bc75e2d63100000",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xe381b4f6106bd1bf3fc84e2b8b388c3a7f86d2c8e058b4c57120a73ea5a5c54b",
>                         "transactionIndex": "0x2",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x0",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000002000000010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000002000000000000000000000000020000000000000000000800000000000000000000000010000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000002000000000000000000000100000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x2cb78f0f545b3e5a265b86700344fefbbeb12b1c",
>                 "transactionHash": "0xe381b4f6106bd1bf3fc84e2b8b388c3a7f86d2c8e058b4c57120a73ea5a5c54b",
>                 "transactionIndex": "0x2",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x26d02",
>                 "from": "0xe58de23db62c6215e5ba7214f816d593b013be92",
>                 "gasUsed": "0xadcb",
>                 "logs": [
>                     {
>                         "address": "0x2cb78f0f545b3e5a265b86700344fefbbeb12b1c",
>                         "topics": [
>                             "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
>                             "0x000000000000000000000000e58de23db62c6215e5ba7214f816d593b013be92",
>                             "0x0000000000000000000000002e90f2eb46ee35bb179c10513d3f7fe8b0693254"
>                         ],
>                         "data": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0x184c14f19c025cd0cdfc7830a6987bc8994b415ee6f210a2ef6f433d8c6dab0b",
>                         "transactionIndex": "0x3",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x1",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000010000000000000200000000000000000000000000000000000000000000000000000200000000000000000000000000000002000000000000000000002000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000020000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000100000000000000000000000000000000000010000000000001000000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x2cb78f0f545b3e5a265b86700344fefbbeb12b1c",
>                 "transactionHash": "0x184c14f19c025cd0cdfc7830a6987bc8994b415ee6f210a2ef6f433d8c6dab0b",
>                 "transactionIndex": "0x3",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x2cf5e",
>                 "from": "0xfe80c0de27ae632c7c4abdf53b777f79b2408f83",
>                 "gasUsed": "0x625c",
>                 "logs": [
>                     {
>                         "address": "0x45eee762aaea4e5ce317471bda8782724972ee19",
>                         "topics": [
>                             "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
>                             "0x000000000000000000000000fe80c0de27ae632c7c4abdf53b777f79b2408f83",
>                             "0x0000000000000000000000008aeb7abbcfe0ed8baafa3ddd2cde39cdbb4d0106"
>                         ],
>                         "data": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0x13109794c30b22a513cc32b10552914561a4f70b3ac39c6d01f10dd03b3bd0be",
>                         "transactionIndex": "0x4",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x2",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000100000000000000000000000000000000000000100000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000011000000000028000000000000000000000000000000000000000000000002",
>                 "status": "0x1",
>                 "to": "0x45eee762aaea4e5ce317471bda8782724972ee19",
>                 "transactionHash": "0x13109794c30b22a513cc32b10552914561a4f70b3ac39c6d01f10dd03b3bd0be",
>                 "transactionIndex": "0x4",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x334b0",
>                 "from": "0xf6306ed3290d9d7062b25981442896f583e44b17",
>                 "gasUsed": "0x6552",
>                 "logs": [],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x605216d2515f6613246596057ef86c62333791a8",
>                 "transactionHash": "0x1f3b8cec80de721aa22603305910e67af044d3c103307b040de8b84374515a04",
>                 "transactionIndex": "0x5",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x534b0",
>                 "from": "0x8703d1c3cd670dd678ddfaca1e98237f6a342c3c",
>                 "gasUsed": "0x20000",
>                 "logs": [
>                     {
>                         "address": "0x0b1c9ac90bff94e4a02813f46acb0402290ac888",
>                         "topics": [
>                             "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
>                             "0x00000000000000000000000076b6452d64a040e61541fc780980e0c226e4b900",
>                             "0x0000000000000000000000001bd552b08b9d7ee239ebbae747d210aeaa598620"
>                         ],
>                         "data": "0x000000000000000000000000000000000000314dc6448d9338c15b0a00000000",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xc2b08f5257988a7b16d13eb48e20b0aaeee2a4d3b44e186d851073a15221ee7b",
>                         "transactionIndex": "0x6",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x3",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000008000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000002000000000000000200000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000002000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010010000000000000000000000008000100000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x76b6452d64a040e61541fc780980e0c226e4b900",
>                 "transactionHash": "0xc2b08f5257988a7b16d13eb48e20b0aaeee2a4d3b44e186d851073a15221ee7b",
>                 "transactionIndex": "0x6",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x5c262",
>                 "from": "0x5b5b70169cf5e1d2c794af49ef94e28e4f8024de",
>                 "gasUsed": "0x8db2",
>                 "logs": [
>                     {
>                         "address": "0x22b422cadfeb7edaadc7410645a2554ff8d46ae4",
>                         "topics": [
>                             "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
>                             "0x0000000000000000000000005b5b70169cf5e1d2c794af49ef94e28e4f8024de",
>                             "0x000000000000000000000000467cba222f543189b754905c4a9a9793cd530819"
>                         ],
>                         "data": "0x000000000000000000000000000000000000000000000004563918244f400000",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xb05c9adade59c6399685e82124cb0461a04bf5f3b823b962bf990bf06b1ec271",
>                         "transactionIndex": "0x7",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x4",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000082000000000010000000000000000000000000000400000000000400000000000008080000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000800000000000000",
>                 "status": "0x1",
>                 "to": "0x22b422cadfeb7edaadc7410645a2554ff8d46ae4",
>                 "transactionHash": "0xb05c9adade59c6399685e82124cb0461a04bf5f3b823b962bf990bf06b1ec271",
>                 "transactionIndex": "0x7",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x6b047",
>                 "from": "0xd4c7eb9000f934d7e26ab7452fed988540531cdb",
>                 "gasUsed": "0xede5",
>                 "logs": [],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
>                 "status": "0x0",
>                 "to": "0x469d5fd89c048a050c8ac0b611639b116f04b8f3",
>                 "transactionHash": "0x0fab79f027d4079ddaaf2df4aa0e55e737ae47b943eb37040dac3c200c88a903",
>                 "transactionIndex": "0x8",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x8a4b0",
>                 "from": "0xaa28d9f66b53201c53d22f8d22203c35d0442acc",
>                 "gasUsed": "0x1f469",
>                 "logs": [
>                     {
>                         "address": "0x3740e0a3109f6bf1041704fa21ad3d1eefec7832",
>                         "topics": [
>                             "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
>                             "0x000000000000000000000000aa28d9f66b53201c53d22f8d22203c35d0442acc",
>                             "0x0000000000000000000000003ffd46a970f073a926aa265db47af839ef716595"
>                         ],
>                         "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>                         "transactionIndex": "0x9",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x5",
>                         "removed": false
>                     },
>                     {
>                         "address": "0x3740e0a3109f6bf1041704fa21ad3d1eefec7832",
>                         "topics": [
>                             "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
>                             "0x000000000000000000000000aa28d9f66b53201c53d22f8d22203c35d0442acc",
>                             "0x0000000000000000000000003740e0a3109f6bf1041704fa21ad3d1eefec7832"
>                         ],
>                         "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>                         "transactionIndex": "0x9",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x6",
>                         "removed": false
>                     },
>                     {
>                         "address": "0x3740e0a3109f6bf1041704fa21ad3d1eefec7832",
>                         "topics": [
>                             "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
>                             "0x000000000000000000000000aa28d9f66b53201c53d22f8d22203c35d0442acc",
>                             "0x0000000000000000000000000025dfb59b480b542a245057ecad4e79349190c0"
>                         ],
>                         "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>                         "transactionIndex": "0x9",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x7",
>                         "removed": false
>                     },
>                     {
>                         "address": "0x0025dfb59b480b542a245057ecad4e79349190c0",
>                         "topics": [
>                             "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
>                             "0x0000000000000000000000000000000000000000000000000000000000000000",
>                             "0x000000000000000000000000aa28d9f66b53201c53d22f8d22203c35d0442acc",
>                             "0x000000000000000000000000000000000000000000000000000000000000074e"
>                         ],
>                         "data": "0x",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>                         "transactionIndex": "0x9",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x8",
>                         "removed": false
>                     },
>                     {
>                         "address": "0x0025dfb59b480b542a245057ecad4e79349190c0",
>                         "topics": [
>                             "0xf00d28232b285f24f2e38415deb2ceb31069e70d4505838b3911b4f02058502e"
>                         ],
>                         "data": "0x000000000000000000000000000000000000000000000000000000000000074f",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>                         "transactionIndex": "0x9",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0x9",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000000000801000000000000000000008000000000000000000000000000010000000000000000000000021000000000000000000000000000000200000000000100000000000000008000000000000000000000000000000000000000000100000020000200010000040000820000100000000000000000010000000000000020000000000000002000000000000000000000000200000000000000000000001000000000000000000000000000000000200000000000000000000000000000002000000000000080000002000001100000000000000000001000020000000000000000000000000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x0025dfb59b480b542a245057ecad4e79349190c0",
>                 "transactionHash": "0xd43d122385905bc342398822273bb95370b2383ca1daa405e8fecb44f4af5f70",
>                 "transactionIndex": "0x9",
>                 "type": "0x0"
>             },
>             {
>                 "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                 "blockNumber": "0x14914c4",
>                 "contractAddress": null,
>                 "cumulativeGasUsed": "0x911f5",
>                 "from": "0x96c5d20b2a975c050e4220be276ace4892f4b41a",
>                 "gasUsed": "0x6d45",
>                 "logs": [
>                     {
>                         "address": "0x0000000000000000000000000000000000001000",
>                         "topics": [
>                             "0x627059660ea01c4733a328effb2294d2f86905bf806da763a89cee254de8bee5"
>                         ],
>                         "data": "0x0000000000000000000000000000000000000000000000000002145ef39c2800",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800",
>                         "transactionIndex": "0xa",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0xa",
>                         "removed": false
>                     },
>                     {
>                         "address": "0x0000000000000000000000000000000000001000",
>                         "topics": [
>                             "0x93a090ecc682c002995fad3c85b30c5651d7fd29b0be5da9d784a3302aedc055",
>                             "0x00000000000000000000000096c5d20b2a975c050e4220be276ace4892f4b41a"
>                         ],
>                         "data": "0x0000000000000000000000000000000000000000000000000012b756907d6800",
>                         "blockNumber": "0x14914c4",
>                         "transactionHash": "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800",
>                         "transactionIndex": "0xa",
>                         "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
>                         "logIndex": "0xb",
>                         "removed": false
>                     }
>                 ],
>                 "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000002010000000000000000000000000000000000020000200000000000000020000080000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000002000000000000000000000000010000000000000000000000000000000000000000000000",
>                 "status": "0x1",
>                 "to": "0x0000000000000000000000000000000000001000",
>                 "transactionHash": "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800",
>                 "transactionIndex": "0xa",
>                 "type": "0x0"
>             }
>         ]
>     },
>     "chainId": 97
> }
> ```

---
description: Simple And Powerful
---

# Enhanced RPC

### &#x20;🔥 particle\_getPrice

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

* no parameters ****&#x20;

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

**Results:**

* `<[object]>` - a JSON object array containing:
  * `native: <string>`, native balance
  * `tokens: <[object]>`,  an object array containing token detail
  * `nfts: <[object]>`,  an object array containing NFT detail

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

```typescript
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

### 🔥 particle\_getTransactionsByAddress

> Get parsed transaction history by giving an address

**Parameters:**

* `<string>` - account address

**Results:**

* `<[object]>` - a JSON object containing:
  * `hash: <string>`, transaction hash
  * `from: <string>`, transaction send from address&#x20;
  * `to: <string>`, transaction send to address
  * `value: <string>`, transaction send value
  * `data: <string>`, transaction send data
  * `gasLimit: <int64>`, transaction gas limit
  * `timestamp: <int64>`, transaction time
  * `status: <int64>`, transaction status:, the value is as below:&#x20;
    * `0`, pending transaction in `TX-QUEUE`, can cancel
    * `1`, pending transaction in `TX-POOL`, can speed up or cancel
    * `3`, confirmed transaction which status is `Success`
    * `4`, confirmed transaction which status is `Fail`

when status is `0|1`, the object containing:

* `type: <int64>`, transaction type, the value is as below:&#x20;
  * `0:` legacy transaction
  * `1:` EIP-2930 access list transaction
  * `2:` EIP-1559 gas fee market transaction&#x20;
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

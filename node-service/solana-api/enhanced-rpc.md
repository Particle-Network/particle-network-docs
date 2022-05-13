---
description: >-
  Enhanced APIs complement the standard APIs to give its users superpowers in
  order to create the Web 3's killer apps.
---

# Enhanced RPC

### 🔥 enhancedGetPrice

> Get real time exchange rate of solana token

**Parameters:**

* `<[string]>` - token mint address array

{% hint style="info" %}
The native token has no mint address, so it's value is **`native`**
{% endhint %}

* `<[string]>` - Array of supported currencies, the value of currency is as below:
  * `usd`
  * `cny`

**Results:**

* `<[object]>` - a JSON object array containing:
  * `address: <string>`, token mint address
  * `currencies: <[object]>`, currency info object

Request Example

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'enhancedGetPrice',
        params: [
            [
                'native', 
                '2Dzzc14S1D7cEFGJyMZMACuoQRHVUYFhVE74C5o8Fwau',
            ],
            [['usd', 'cny']],
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
curl 'https://api.particle.network/solana/rpc' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"enhancedGetPrice","params":[["native","2Dzzc14S1D7cEFGJyMZMACuoQRHVUYFhVE74C5o8Fwau"],[["usd","cny"]]]}
'
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "chainId": 103,
    "jsonrpc": "2.0",
    "id": 0,
    "result": [
        {
            "address": "native",
            "currencies": [
                {
                    "type": "usd",
                    "price": 52.09,
                    "marketCap": 17527528410.265934,
                    "24hChange": 17.807880245620492,
                    "24hVol": 2822619824.236063,
                    "lastUpdatedAt": 1652433876
                },
                {
                    "type": "cny",
                    "price": 353.66,
                    "marketCap": 118957582567.63393,
                    "24hChange": 17.750642277273172,
                    "24hVol": 19162201462.773823,
                    "lastUpdatedAt": 1652433876
                }
            ]
        },
        {
            "address": "2Dzzc14S1D7cEFGJyMZMACuoQRHVUYFhVE74C5o8Fwau",
            "currencies": [
                {
                    "type": "usd",
                    "price": 0.00000559,
                    "marketCap": 0,
                    "24hChange": 5.572351955926479,
                    "24hVol": 1.0696550251883796,
                    "lastUpdatedAt": 1652415761
                },
                {
                    "type": "cny",
                    "price": 0.00003795,
                    "marketCap": 0,
                    "24hChange": 5.659460423725415,
                    "24hVol": 7.265738724094583,
                    "lastUpdatedAt": 1652415761
                }
            ]
        }
    ]
}
```

### 🔥 enhancedGetTokensAndNfts

> Get Token list and NFT list by giving an address

**Parameters:**

* `<string>` - Pubkey of account to query, as base-58 encoded string

**Results:**

* `<object>` - a JSON object containing:
  * `lamports: <u64>`, lamports balance of the address
  * `nfts: <[object]>`, nft list of the address
  * `tokens: <[object]>`, token list of the address

Request Example

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'enhancedGetTokensAndNfts',
        params: ['6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe'],
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
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"enhancedGetTokensAndNfts","params":["6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe"]}
'
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "chainId": 103,
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "lamports": 7395293870,
        "nfts": [
            {
                "name": "MPD #2855",
                "symbol": "MPD",
                "image": "https://www.arweave.net/dfF-HHAE_G2qQN4N7gN2gLvXah_7xOQzUysI68rLqPA?ext=jpg",
                "mint": "5bxPBSWibpkYifCzaKhPuix63pqhehZCf5DU4LWwh5ot",
                "sellerFeeBasisPoints": 0,
                "data": {
                    "name": "MPD #2855",
                    "symbol": "MPD",
                    "image": "https://www.arweave.net/dfF-HHAE_G2qQN4N7gN2gLvXah_7xOQzUysI68rLqPA?ext=jpg",
                    "properties": {
                        "files": [
                            {
                                "uri": "https://www.arweave.net/dfF-HHAE_G2qQN4N7gN2gLvXah_7xOQzUysI68rLqPA?ext=jpg",
                                "type": "image/jpg"
                            }
                        ],
                        "creators": [
                            {
                                "address": "MPaXksCWktUySTTQ91h5Bq6CAeTWb7Myc9jhEiQQwKb",
                                "share": 100
                            }
                        ]
                    }
                }
            }
        ],
        "tokens": [
            {
                "name": "XYZ Test",
                "symbol": "XYZ",
                "image": "https://static.particle.network/token-list/solana/DEhAasscXF4kEGxFgJ3bq4PpVGp5wyUxMRvn6TzGVHaw.png",
                "mint": "5yAymuqKorGrXka7SnEQaHdeXxzKrDMXzDkQUGgT9gGy",
                "decimals": 6,
                "amount": 51000000
            }
        ]
    }
}
```

### 🔥 enhancedGetTransactionsByAddress

> Get parsed transaction history by giving an address

**Parameters:**

* `<string>` - account address as base-58 encoded string
* `<object>` - (optional) Configuration object containing the following fields:
  * `limit: <number>` - (optional) maximum transactions to return (between 1 and 1,000, default: 1,000).
  * `before: <string>` - (optional) start searching backwards from this transaction signature. If not provided the search starts from the top of the highest max confirmed block.
  * `until: <string>` - (optional) search until this transaction signature, if found before limit reached.

**Results:**

* `<[object]>` - a JSON object array containing:
  * `type: <string>`, parsed type for a single transaction, the type value is as below
    * `unknown`, can not parse this transaction in server
    * `transfer-token`, the transaction contains transfer token action
  * `lamportsChange: <int64>`, the account by given address lamports change in this transaction
  * `lamportsFee: <int64>`, the transaction lamports fee
  * `signature: <string>`, the transaction signature
  * `blockTime: <int64>`, the transaction block time
  * `status: <"success" | "failed">`，the status of the transaction
  * data: \<object>, extended data, when the type value is not `unknown`, data is object containing transaction detail

Request Example

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'enhanceGetTransactionsByAddress',
        params: ['6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe'],
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
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"enhanceGetTransactionsByAddress","params":["6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe"]}
'
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "chainId": 103,
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "type": "transfer-token",
            "lamportsChange": -5000,
            "lamportsFee": 5000,
            "signature": "56VW9ki27EQzVQ1MqWFiZDfm2JgnCRW13MYJdmMQ8C4xeZLj8AbcQFKBfU4GMQQJT1MKyacpAiuMzMnDGcMmddNG",
            "blockTime": 1651214222,
            "status": "success",
            "data": {
                "name": "XYZ Test", // token name
                "symbol": "XYZ", // token symbol
                "image": "https://static.particle.network/token-list/solana/DEhAasscXF4kEGxFgJ3bq4PpVGp5wyUxMRvn6TzGVHaw.png", // token logo url
                "mint": "51qnHqrmVZfThAu9upFuKjsfLMLkZ8oezBz5gPxdFtJ3",
                "decimals": 9,
                "amountTransfered": 100, // transfer amount in this transaction
                "sender": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
                "senderAssociatedTokenAddress": "DdEq5UPCrg7WMqXqSXk2bUh16GMwLf5eXE1Ywj5CDTQN",
                "receiverAssociatedTokenAddress": "J4g3on2T2zPFCcjMJhPtGBXyLYFFYoFoxR69gVsTaqAi"
            }
        },
        {
            "type": "unknown",
            "lamportsChange": -1471600,
            "lamportsFee": 10000,
            "signature": "3XxaximWV9SnTVTeZtLyzTrJG7XHbByeYAKfxB1NS4BvhEP3Jb6hdExtvBMhp4e7y76XeCCDwxszTXYxP4yUFa2",
            "blockTime": 1650518427,
            "status": "success",
            "data": {}
        }
    ]
}
```

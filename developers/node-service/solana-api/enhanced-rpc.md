---
description: Simple And Powerful
---

# Enhanced RPC

### 🔥 enhancedGetPrice

> Get the real-time exchange rate of the Solana token ($SOL)

**Parameters:**

* `<[string]>` - token mint address array

{% hint style="info" %}
The native token has no mint address, so it's value is **`native`**
{% endhint %}

* `<[string]>` - Array of supported currencies, the values of currencies is as follows:
  * `usd`
  * `cny`

**Results:**

* `<[object]>` - a JSON object array containing:
  * `address: <string>`, token mint address
  * `currencies: <[object]>`, currency info object

Request example:

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/solana', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'enhancedGetPrice',
        params: [
            [
                'native', 
                '2Dzzc14S1D7cEFGJyMZMACuoQRHVUYFhVE74C5o8Fwau',
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
curl 'https://rpc.particle.network/solana' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"enhancedGetPrice","params":[["native","2Dzzc14S1D7cEFGJyMZMACuoQRHVUYFhVE74C5o8Fwau"],["usd","cny"]]}
'
```
{% endtab %}
{% endtabs %}

Response example:

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

### 🔥 enhancedGetTokensAndNFTs

> Get token list and NFT list by giving an address

**Parameters:**

* `<string>` - Pubkey of account to query, as base-58 encoded string
* `<object>: optional`&#x20;
  * `parseMetadataUri: <boolean, default: false>` If true, the API will parse the metadata's uri that can get the image url and other info.

**Results:**

* `<object>` - a JSON object containing:
  * `lamports: <u64>`, lamports balance of the address
  * `nfts: <[object]>`, NFT list of the address
  * `tokens: <[object]>`, token list of the address

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/solana', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'enhancedGetTokensAndNFTs',
        params: ['6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe', {
            parseMetadataUri: true,
        }],
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
curl 'https://rpc.particle.network/solana' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"enhancedGetTokensAndNFTs","params":["6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe"]}
'
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "lamports": 18394293870,
        "nfts": [
            {
                "mint": "583HuvyUfULu7uULrmQrTJ1v9PQh28esyFYNXLwfqC5P",
                "address": "583HuvyUfULu7uULrmQrTJ1v9PQh28esyFYNXLwfqC5P",
                "isSemiFungible": false,
                "name": "MPD #4834",
                "symbol": "MPD",
                "image": "",
                "sellerFeeBasisPoints": 0,
                "metadata": {
                    "key": 4,
                    "updateAuthority": "MPaXksCWktUySTTQ91h5Bq6CAeTWb7Myc9jhEiQQwKb",
                    "mint": "583HuvyUfULu7uULrmQrTJ1v9PQh28esyFYNXLwfqC5P",
                    "data": {
                        "name": "MPD #4834",
                        "symbol": "MPD",
                        "uri": "https://arweave.net/QDGUTZvyQmsvIzE8pYgW9ZTPCs0ELXyg2vcJd2g-3A8",
                        "sellerFeeBasisPoints": 0,
                        "creators": [
                            {
                                "address": "FbWan7RPJEqAgErbp1szUBjW3tQHSX5frDZXko4B5yEg",
                                "verified": true,
                                "share": 0
                            },
                            {
                                "address": "MPaXksCWktUySTTQ91h5Bq6CAeTWb7Myc9jhEiQQwKb",
                                "verified": false,
                                "share": 100
                            }
                        ],
                        "uriData": { // not exists if parseMetadataUri is false
                            "name": "MPD #4834",
                            "symbol": "MPD",
                            "image": "https://www.arweave.net/9D5eu2Hc5yVLwmrN4DGXLXkOhg9AE2SqLwZlkzN7drs?ext=jpg",
                            "properties": {
                                "files": [
                                    {
                                        "uri": "https://www.arweave.net/9D5eu2Hc5yVLwmrN4DGXLXkOhg9AE2SqLwZlkzN7drs?ext=jpg",
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
                    },
                    "primarySaleHappened": true,
                    "isMutable": true,
                    "editionNonce": 255
                }
            }
        ],
        "tokens": [
            {
                "decimals": 6,
                "amount": 500000,
                "address": "AEaYYeBrU5bWpf9CyaeJN8DV6cQLP9Rx6xhUvnuUTfwQ",
                "mint": "AEaYYeBrU5bWpf9CyaeJN8DV6cQLP9Rx6xhUvnuUTfwQ",
                "name": "",
                "symbol": "",
                "image": ""
            }
        ]
    }
}
```

### 🔥 enhancedGetTransactionsByAddress

> Get parsed transaction history by giving an address

**Parameters:**

* `<string>` - account address as base-58 encoded string
* `<object>` - (optional) configuration object containing the following fields:
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
  * `status: <"success" | "failed">`, the status of the transaction
  * `data: <object>`, extended data, when the type value is not `unknown`, the data is an object containing transaction detail

Request example:

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/solana', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
        method: 'enhancedGetTransactionsByAddress',
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
curl 'https://rpc.particle.network/solana' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":103,"jsonrpc":"2.0","id":0,"method":"enhancedGetTransactionsByAddress","params":["6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe"]}
'
```
{% endtab %}
{% endtabs %}

Response example:

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
                "receiver": "5vYsVaJufzUg1ZaW498VZCGbhNTTGDezHbydyJ6invAD",
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



### 🔥 enhancedSendAndConfirmRawTransaction

> Same as \`sendAndConfirmRawTransaction\` in @solana/web3.js

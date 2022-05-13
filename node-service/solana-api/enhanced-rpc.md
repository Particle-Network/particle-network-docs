---
description: >-
  Enhanced APIs complement the standard APIs to give its users superpowers in
  order to create the Web 3's killer apps.
---

# Enhanced RPC

### 🔥 enhancedGetTokensAndNfts

> Get Token list and NFT list by giving a address

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
            password: 'Your Project Serve Key',
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

### 🔥 enhanceGetTransactionsByAddress

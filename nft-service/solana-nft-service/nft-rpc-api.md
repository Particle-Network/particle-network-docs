---
description: >-
  Mint, Update, Sell, Buy, Settle, Withdraw..., Easily build a fully functional.
  Market!!!
---

# NFT RPC API

### 🔥 NFT_mint

> mint NFT by metadata

**Parameters:**

* `<object>` - a JSON object containing:
  * `owner: <string>` - the account to mint NFT
  * `metadata: <object>` - the metadata of the NFT. Follow [The Non-Fungible Standard](https://docs.metaplex.com/programs/token-metadata/token-standard#the-non-fungible-standard)
  * `updateAuthority: <string>` - (optional, default **owner**) An account with metadata update authority

**Results:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of the NFT
  * `mintAssociatedTokenAccount: <string>` - The token account associated with the **owner**
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the **owner**

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_mint',
            params: [
                {
                    owner: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    metadata: {
                        name: 'Test: e17Ij',
                        symbol: 'TEST',
                        description: 'Test description',
                        seller_fee_basis_points: 100,
                        image: 'https://gateway.pinata.cloud/ipfs/QmY21xP5qjz866TBovLH2Px5fLEyfU1uBg69BfDpdQu4KJ',
                        animation_url: '',
                        external_url: '',
                        attributes: [
                            {
                                trait_type: 'Type',
                                value: 'Skeleton',
                            },
                        ],
                        properties: {
                            creators: [
                                {
                                    address: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                                    share: 50,
                                },
                                {
                                    address: '7quN8ZdQYkXyWzSpytv9xK5ngzq32UapNbFSz91tpmvA',
                                    share: 30,
                                },
                                {
                                    address: 'Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V',
                                    share: 20,
                                },
                            ],
                        },
                    },
                },
            ],
        },
        { auth },
    );

    console.log(response.data);
})();

```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "mint": "29nPkoZPcSCnGhfKKvcPB29joUa7iRfcUSyWk3qoZB37",
        "mintAssociatedTokenAccount": "87sjCifEip3hNPhLJQ3rNayWvP5zgjL6bjGnsgPpN2iR",
        "transaction": {
            "isPartialSigned": true,
            "serialized": "3upXthSvHwVVCdrM9euWVNhZRZUekYgt1BxoHaEGWRwyZkVk5Ro3BbVNEJjDMMCthVYZc2YCFeScwL3cAHtftWw7Z9SCRue6xrU7xLMTYvY6h9Ed6w99Yujxtt7xH6mMyc1RHVfj19PMhTM9vgHgjYv8xV3tXCexsvuB2pTP6cVnF79mTLmgk8puRHYD2fzR8JaYr4EgpxyJEddUYFeVUiiq8aLob3oLBZAsGD9V9CY8pgeBvqyFhvWz8qXa3rbueRvUFid8rguaG7mjcmSYSQTgX9C8R3fN9q3Hp3CpF4rD1BE41S5QGuPvmJ5oCFiziciNvHRGfYMM6ogdtG47xZ7BKPBxbZieo1u3Moam2VFLNJJauphev48D92tr77uC9rmCzpZ4YZzVLAnkbJXMbr5TY3A4p3USp9BJDnkanESY2jvW9WMu7wHzyLC8MvTfqmiNzppoQwWm7vrSDNkFtJoauEtC7ZXugZhM2hsBLFBaEiPH221h2hyuEBnXSSjULdakJVGbDU2NkeCcKsv234mSr15NtUHDX43ZPKSfjR1meoyoZD15ArkNd7TKPq2qxBmd62oPEqMTxaafsWZUyhYjHyUJG2h4x3KfxQdVYhBD5cnNNcVavgqHiebJ8BLwTcagW6QF7yJbAYdb4en6Pp83cnpQA4o8uwYW3pe7fJM9RP4Ue7WP6aY9BotX5amKUreSGRzQrbrngiJ8kZmi5BTHu4b3C5GU5x1F7GUgJ2TdUWSWGL7HTYg1T2R7F4rfkcJNwKcwprzqaY6GDYqwiSzvEPU5GSguw44tRDSQrcmqsVFweU1MBU6YXTVgdUWT5cytjhmECrBoyR5WGHJjYgQ5gWb85iVrh6w5upiKpmaxaqfGBaJ6eXxkPj2qZSLwBbmtW5GdL8RdbeYn1cor7opNNRyV13UfVwXb42RteVMAMaHVXybRJSb8tNAnHP4H9wFrKamSuE7SRS5gLxQvMKzuvrXT9ZrzSyAWHCubk1LMPe1BG1vJSauhzehrKB6c2UEzLuEC8GCH753ubnKXWzTa9KyGvw7h1QvfNzdkeDhSMg7wW83LRgHFi4MZqaczEwNMkomBx9mh1uThQopqb8HEraayxpf8WVJG5H4Eg2vzFLzKL8BomNoUVbYFHG8ELXK1mJbC3WEeafxYUb8Qc8b9wic3AjkwPCHbL7hqgm3qsKs8Ej8DPkqi3U426bM5jJVGXZyTD2Gksy",
            "signers": [
                "7NtyMZAYfmJzFVAXRrHptm7AXJwBmFErQGRphQWtU3vsKU9eipyyWfAiLfK6gbhMBcbDTsNYTgd4moUf2hoCYGh"
            ]
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_mintByUrl

> mint NFT by metadata uri

**Parameters:**

* `<object>` - a JSON object containing:
  * `owner: <string>` - the account to mint NFT.
  * `metadataUri: <object>` - the uri associated with [metadata](https://docs.metaplex.com/programs/token-metadata/token-standard#the-non-fungible-standard).
  * `updateAuthority: <string>` - (optional, default **owner**) An account with metadata update authority.

**Results:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of the NFT.
  * `mintAssociatedTokenAccount: <string>` - The token account associated with the **owner**.
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the **owner**.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_mintByUrl',
            params: [
                {
                    owner: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    metadataUri: 'https://bafybeichqewhbuj6zbbil2adnvdj4wvk62n2czgmqidpjxp7lxyuoh374a.ipfs.nftstorage.link',
                },
            ],
        },
        { auth },
    );

    console.log(response.data);
})();

```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "mint": "9nqTYkboxC8X6LT5pxKDApZXFSHiHsJVfHLeBdbasqF9",
        "mintAssociatedTokenAccount": "F1TpWa88c3G9goyCPeiPQkUjCKa7pBtRqYwJYWwqx3GP",
        "transaction": {
            "isPartialSigned": true,
            "serialized": "3upXthSvHwVVCdrM9euWVNhZRZUekYgt1BxoHaEGWRwyZkVk5Ro3BbVNEJjDMMCthVYZc2YCFeScwL3cAHtftWw7iiiAAuHyeRxYuRJonRxq3ZE66CpdmiCsQSzGdz8JhPxV4bbeD31JiuSoEUv1umNjGFog7VLcN2o9hfgvCs4SCNFixPCEzLdUPPxY2SPM3iBd1F7kVXW9ERU8E2UX5LkY8qzddBz1ZddTLvjGmTw2bGvLB1LbxUkkVFbZehJVYEqQ1xehJQDAXkvWcf1zG5zPHzYVKhpGuagWPrxp8hipwdooYuiwE6ovnbJGdJGNXsUjFGKPzaimnkFtHhJ37Ydcit1NCERT1nWQ6nKERAY9AABAb8X14CKC5vquEdyGC72kggPQdCBdUwCDEowzpL2QshV4UtfhfXGzkiNUag26ydvS51kNFYQPDyqR8c17qqqr2C5SA1Tkog5xM5zcuTxjm6kcu1cYLe8QAzGfgpQeSQzLsvYcjBZ95XbBekZUAecaWU3pfZY9whVD6hwgVnGiTWtVakSPZETZEkX9bCXfqMTfpsiHz1U6EyrKGK2KxpHxNE3WTNKBYGrBk6S44bYyg7cP68MxGh5wvJh6YfEbNwo2zPkWTTTMsjUaKAEcv6J6jVSzQ2rGmLas5Ju49dvL2NDZAziTsny4xuB2Vrmb4geAh7fsLr2BjHJsgZkWcu3ypvfvVFyWb4VdMeuQVSq79jxdQHv4LUYGEDERvdjJ941cjnjSojGEHxa288EubqUevNmmUE8oeNizTgHvCVDAE3ZG5pqNxq8Bfpr7PyH9xYxfTsvEwBgysRDw91Y2oCyTCjmuSUWvypoeNFRgoRi1s6DW7utdYnXqaRo5hAGywT3DM24pNpDSCLBS8yDf22Ft5xoiRcLBJD9as1fRENUGaWuPWKC1agnXKeYGZtLgKrn55X3S1R2u637zdpzhdwREdenJN7rDSGdhvXQQmTBbZjdec4npqgKTe7aEWL1EfxcyCxU1mYYiGeYRvmecJG8M29XJELu4B6YBp3AkqPBfNQMJbHjCSnK6u1ZZLiQCURss5tgrA63JuaXW6VUZxrGMte7MDZTH1j4Sj98vhEAovLktGknHsUuyH7HctNWLw1zM4KmjWXz6jrcUV2DwffApEaZJTCiaKCwwnaPUQydRbqqBsAR6XX9Hfn8uUUbq6zhCr22nrizr1ip7FLqB6o5EvqtR1N38kf",
            "signers": [
                "2eH4ZEBTaNwU3NVZEiv1rQTTurUf6fwFF7t4mCndVTBzRVopyK44PD3tgX57ucD3sUFKRpqLBnd8QUNXjsbyQnbu"
            ]
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_info

> get NFT info

**Parameters:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of the NFT.
  * `parseMetadataUri: <boolean>` - (optional, default **false**) If true, the API will parse the metadata's uri that can get the image url and other info.

**Results:**

* `<object>` - a JSON object about metadata

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_info',
            params: [
                {
                    mint: '2aqWDYDeZaMdqkYQcd79tXiWrpSAzwj2J8Uf4GqoB7dg',
                    parseMetadataUri: true,
                },
            ],
        },
        { auth },
    );

    console.log(response.data);
})();

```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "key": 4,
        "updateAuthority": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
        "mint": "2aqWDYDeZaMdqkYQcd79tXiWrpSAzwj2J8Uf4GqoB7dg",
        "data": {
            "name": "Test: ZlNae",
            "symbol": "TEST",
            "uri": "https://gateway.pinata.cloud/ipfs/Qmd8U94CJjaihLYo2TrDfgcLRJAbGEBfD1gYWfSSXWjb9J",
            "sellerFeeBasisPoints": 100,
            "creators": [
                {
                    "address": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
                    "verified": true,
                    "share": 50
                },
                {
                    "address": "Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V",
                    "verified": false,
                    "share": 50
                }
            ],
            "uriData": null
        },
        "primarySaleHappened": true,
        "isMutable": true,
        "editionNonce": 255,
        "tokenStandard": 0
    },
    "chainId": 103
}
```

### 🔥 NFT_update

> update [the data of NFT metadata](https://docs.metaplex.com/programs/token-metadata/accounts#metadata)

**Parameters:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of the NFT.
  * `metadata: <object>` - fields from [the data of NFT metadata](https://docs.metaplex.com/programs/token-metadata/accounts#metadata).
    * `name: <string>`
    * `symbol: <string>`
    * `uri: <string>`
    * `sellerFeeBasisPoints: <int>`
    * `newUpdateAuthority: <string>`
    * `primarySaleHappened: <boolean>`
    * `isMutable: <string>`

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the NFT owner.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_update',
            params: [
                {
                    mint: 'EkGWiw9zrqbGj8iFueGBnDE7g3yQZCKuDneHSvLmR6wY',
                    metadata: {
                        name: 'Test NFT name',
                    },
                },
            ],
        },
        { auth },
    );

    console.log(response.data);
})();

```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "transaction": {
            "isPartialSigned": false,
            "serialized": "6fPvCMgpMTwLsAX7W2WEz9hwXV32Cnk3LbfBFZLEsDA2jZgC9QxiSHWfoXXMfUYXqZHBGhtu4YvPuhfXVtZoBwKdT37C7f6D9dv4wEmSdRjHiHAt8Cr2sZZAoiGvqJkVzEqSjnNh5rTXQG7Z43oDPtskheZo2hVWiqirgJpogbQ36Sko5J53dNtagR4fUJTnNDgq3m6yRqCT5Vq9wN8r131Yz9C9GtKKkpk2EBBPDEuqdxFcwcN1QRs8gNhmkm4McKaXkmVp6s9oS9Gphu2j441Dv2rcnt9VbZrjv9GBgEkMSZLKueMCxH1FvpBKLBGLfwPyJHxsC3YzdWxjiyTe9uykRuE3Y8ku59j95VSXVDbsUm23J4jsmak6LnN8x7o4WBL2S3NYdEuxKaSSfU6y48NgAYrCCTCvYXYrPT832Yyg7pcub48L4TL9Dj4DVp9Sh9r85zvqKGmuV7NtQ68VAzHhKuxzvHktKRC8AESYjapgYv1JPd8jFEQn5J29XAb3zaPqddBE8Ere5aKC8MAWo8KwVygGgieSvnQpXQRbWLGeqNKDXJiCAmfW1GXgyGEYGr8WieRPNdCX2GieMoAo",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_setCreatorVerified

> Set a creator is verified for the NFT

{% hint style="info" %}
It is important to check this field to ensure the authenticity of the creator.

Without certification, creators can't share royalties.
{% endhint %}

**Parameters:**

* `<object>` - a JSON object containing:
  * `creator: <string>` - the creator account from the NFT metadata
  * `mint: <string>` - the mint address of the NFT
  * `verified: <boolean>` - (optional, default true) verified or not

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the creator

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_setCreatorVerified',
            params: [
                {
                    creator: 'Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V',
                    mint: '7LvE3zUYSmowFGEW6pxCUjwtnMgaGqGpxB1XXvVfn7wq',
                    verified: false,
                },
            ],
        },
        { auth },
    );

    console.log(response.data);
})();

```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "transaction": {
            "isPartialSigned": false,
            "serialized": "3XkfoeU1FeyQ6spTnBHnipD3eiPgcqvBecXPST439B43gtvxJA6UuTp4YEWtDhSNcgt1b5CkjuWkKn47F5rNcuKT7uZ3cfxmMBtg2P5XfTqftSYub94K5AJQmzLnoNSn2bPYvrgxFLiDZRaKLQHDm4d7PD3bQCK1JqQ1745vJxP6M3sPKfimXm98ZJzaNn61fPQ5SopNQjG6CwkVthnn1EYNqezJBtv2ADpBZdcK9rLjN27p1ufpjsrbrGesqggEYmU6qHLavHe9WFNtH3gjSK",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_isCreatorVerified

> check whether the creator is verified or not for the NFT

**Parameters:**

* `<object>` - a JSON object containing:
  * `creator: <string>` - the creator account from the NFT metadata
  * `mint: <string>` - the mint address of the NFT

**Results:**

* `<boolean>` - is verified or not for the NFT

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_isCreatorVerified',
            params: [
                {
                    creator: 'Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V',
                    mint: '7LvE3zUYSmowFGEW6pxCUjwtnMgaGqGpxB1XXvVfn7wq',
                },
            ],
        },
        { auth },
    );

    console.log(response.data);
})();

```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": false,
    "chainId": 103
}
```

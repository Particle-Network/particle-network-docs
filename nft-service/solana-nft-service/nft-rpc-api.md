---
description: >-
  Mint, Update, Sell, Buy, Settle, Withdraw..., Easily build a fully functional
  Market!!!
---

# NFT RPC API

### 🔥 NFT\_mint

> mint NFT by metadata or offering a metadata uri

**Parameters:**

* `<string>` - the public key of account to mint, as base-58 encoded string
* `<object>` - metadata. Follow [The Non-Fungible Standard](https://docs.metaplex.com/programs/token-metadata/token-standard#the-non-fungible-standard)
  * you can also offer a `metadata_uri` which fetch and parse as a metadata JSON instead of a metadata object.

**Results:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of the NFT
  * `mintAssociatedTokenAccount: <string>` - The address of the account where you place the NFT
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the NFT owner

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const metadata = {
    name: `Test NFT Name`, // max length up to 32
    symbol: 'Test Symbol', // max length up to 10
    description: 'Test description',
    seller_fee_basis_points: 100, // 0 ~ 10000
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
                share: 100,
            },
        ],
    },
};

// same as metadata, fetch and parse as a metadata JSON
const metadataUri = 'https://gateway.pinata.cloud/ipfs/QmVJ8tG8nuqCk8QeZmBWkQxJKhSx55kprw8i18LiWXU9f8';

(async () => {
    // use metadata
    const response1 = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        method: 'NFT_mint',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', metadata],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response1.data);

    // use url
    const response2 = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        method: 'NFT_mint',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', {
            metadata_uri: metadataUri,
        }],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response2.data);
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
        "mint": "5DTYtoLtdWnUjtkf9abuyPDKA1K3uizjNZBj2saWdBHv",
        "mintAssociatedTokenAccount": "Hmi2gfLhzzveeT58yWUXDVcfLGcrgGzMfmQ3LPFoe3J3",
        "transaction": {
            "hasPartialSign": true,
            "serialized": "PFPXCr7kuoiiw3y55ACWKYeYdCUsXw5fnQ384LuX4c8djRtgVKmT3hdNtEhMuw5YQooVLSDA33ri1exvX2mQ7GEQ2LUkHkaY7QeYL9UAcFXyAYqPL88xeFrADWzZ4H4XuAemCYwoTRNwogRNfonuZSeq8CVSzymYHZ36qVtuT28oiohoEFSQGDt9q8NzoT9QebFWojJ3Zfp4Twef1h5QeK3Y9ffamZLzqWoGHmZsJHDRhikA7HkMZXNiW5qG87xpg9mCwLQZFWBLXwhGDbiyi2YC8DvoGniieem9ftPS9m7e4ufZfZktn5wdTUnGSXrRU7jpbmRqR8Eutszig8Y1yewGYESjCWKjWFCk3fD1uzbGTEJyZjQkoUNqizREEBBUq11FeoSFreKWX3xUiEvUe7BNYBAUJKP9CvLJe7ZiD4f3voWMEjorkB4qKWXXFct7o7Tm3fJH5LAucvwpimnFkbkKA6Nou9WBUV6upUsM67u1hgKiwZj2wMxhd9t9nu3by2rHhhScjrGg3EnhyH8vpq6mf645bqubjfEDodknvy1zaswVuJQFUh8ZaCLyB3gmRB8c9xwwZHwcgfV4bs5qVMKxK4PQxR3ArYVrLLq2yXRvVexF1YuF8GQSuZ6LyNggwVBK2t8trxjtepAWNpug8B3zmwxquGPfunKtwRtNvJXkrp23TkDrCHtZSnQBGo6d5zXD7bG4sDq85H7Y1FwtEeDVQTxhcvLG46ZVBkkYKxegGBTcjLmLjk1LyqqoF6QSVmZcGHD56FPTjHZaLnzXzzNozqx9ApytuZkQcwDWV9UWsWAwWbvGtGoUWkDcDfVEKCv1BDGYcqGuSp27Fmz1Y1pVdQ4BV3sxdy3Bp4yzNRaXbJpSnASxLmn3ixSs4g1Q6c6SHoYqWnXgEmgM8VL4JpCehqmdGDCvwzi7bE9dHYZ8DLL5aCA2nJmC4L89PMBVRxxLAc3Acvn9jRhBieRoCfxMrDPjjbmx47tUN8s93e9xvkR3ssX4Pb1SyN6aKqfGH1vZz2azqdSUTGaVT5ZC7Zh7jnXSzWXQpjkZbGeKarxB6HjCJR4DmfZD7q74K7ZmbsNS9TKqMmjiffExvCRJKzmG2afjSTTnET5c2NWezZby4WbnCo",
            "signers": [
                "5p64ZhpEQYGmzspkV6Lc1AMhdvemmPuHKZMnEc4knhxbUyUPqWvQvwfqcsuB2zNQbBFAvqCzbANuwtNnhKN8CC5J"
            ]
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_update

> update [the data of NFT metadata](https://docs.metaplex.com/programs/token-metadata/accounts#metadata)

**Parameters:**

* `<string>` - the mint address of NFT you owned
* `<object>` - fields from [the data of NFT metadata](https://docs.metaplex.com/programs/token-metadata/accounts#metadata)
  * `name: <string>`
  * `symbol: <string>`
  * `uri: <string>`
  * `sellerFeeBasisPoints: <int>`
  * `newUpdateAuthority: <string>`
  * `primarySaleHappened: <boolean>`
  * `isMutable: <string>`

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the NFT owner

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const mint = '77jsJBjKMi5MsKYoSBS2a2HVQ9nd7ULTZUVqKZyhRFzj';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

// update nft's name and symbol
(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_update',
        params: [mint, {
            name: 'New Name',
            symbol: 'New Symbol'
        }],
    }, { auth });

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
            "hasPartialSign": false,
            "serialized": "PFPXCr7kuoiiw3y55ACWKYeYdCUsXw5fnQ384LuX4c8djRtgVKmT3hdNtEhMuw5YQooVLSDA33ri1exvX2mQ7GEQ2LUkHkaY7QeYL9UAcFXyAYqPL88xeFrADWzZ4H4XuAemCYwoTRNwogRNfonuZSeq8CVSzymYHZ36qVtuT28oiohoEFSQGDt9q8NzoT9QebFWojJ3Zfp4Twef1h5QeK3Y9ffamZLzqWoGHmZsJHDRhikA7HkMZXNiW5qG87xpg9mCwLQZFWBLXwhGDbiyi2YC8DvoGniieem9ftPS9m7e4ufZfZktn5wdTUnGSXrRU7jpbmRqR8Eutszig8Y1yewGYESjCWKjWFCk3fD1uzbGTEJyZjQkoUNqizREEBBUq11FeoSFreKWX3xUiEvUe7BNYBAUJKP9CvLJe7ZiD4f3voWMEjorkB4qKWXXFct7o7Tm3fJH5LAucvwpimnFkbkKA6Nou9WBUV6upUsM67u1hgKiwZj2wMxhd9t9nu3by2rHhhScjrGg3EnhyH8vpq6mf645bqubjfEDodknvy1zaswVuJQFUh8ZaCLyB3gmRB8c9xwwZHwcgfV4bs5qVMKxK4PQxR3ArYVrLLq2yXRvVexF1YuF8GQSuZ6LyNggwVBK2t8trxjtepAWNpug8B3zmwxquGPfunKtwRtNvJXkrp23TkDrCHtZSnQBGo6d5zXD7bG4sDq85H7Y1FwtEeDVQTxhcvLG46ZVBkkYKxegGBTcjLmLjk1LyqqoF6QSVmZcGHD56FPTjHZaLnzXzzNozqx9ApytuZkQcwDWV9UWsWAwWbvGtGoUWkDcDfVEKCv1BDGYcqGuSp27Fmz1Y1pVdQ4BV3sxdy3Bp4yzNRaXbJpSnASxLmn3ixSs4g1Q6c6SHoYqWnXgEmgM8VL4JpCehqmdGDCvwzi7bE9dHYZ8DLL5aCA2nJmC4L89PMBVRxxLAc3Acvn9jRhBieRoCfxMrDPjjbmx47tUN8s93e9xvkR3ssX4Pb1SyN6aKqfGH1vZz2azqdSUTGaVT5ZC7Zh7jnXSzWXQpjkZbGeKarxB6HjCJR4DmfZD7q74K7ZmbsNS9TKqMmjiffExvCRJKzmG2afjSTTnET5c2NWezZby4WbnCo",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_info

> Get NFT Info

**Parameters:**

* `<string>` - the mint address of NFT you owned
* `<object>: optional`&#x20;
  * `parseMetadataUri: <boolean, default: false>` If true, the API will parse the metadata's uri that can get the image url and other info.

**Results:**

* `<object>` - a JSON object about metadata

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const mint = '77jsJBjKMi5MsKYoSBS2a2HVQ9nd7ULTZUVqKZyhRFzj';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

// Get NFT Info
(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_info',
        params: [mint, {
            parseMetadataUri: true,
        }],
    }, { auth });

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
        "mint": "77jsJBjKMi5MsKYoSBS2a2HVQ9nd7ULTZUVqKZyhRFzj",
        "data": {
            "name": "Test NFTFFFF",
            "symbol": "TEST",
            "uri": "https://gateway.pinata.cloud/ipfs/QmSbQerqyuAVJzD5zX26wAiEihmyWcU9htDchwuVhejghZ",
            "sellerFeeBasisPoints": 100,
            "creators": [
                {
                    "address": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
                    "verified": true,
                    "share": 50
                },
                {
                    "address": "7quN8ZdQYkXyWzSpytv9xK5ngzq32UapNbFSz91tpmvA",
                    "verified": false,
                    "share": 50
                }
            ],
            "uriData": { // not exist if parseMetadataUri is false
                "name": "Test: LBFUF",
                "symbol": "TEST",
                "description": "Test description",
                "seller_fee_basis_points": 100,
                "image": "https://gateway.pinata.cloud/ipfs/QmY21xP5qjz866TBovLH2Px5fLEyfU1uBg69BfDpdQu4KJ",
                "animation_url": "",
                "external_url": "",
                "attributes": [
                    {
                        "trait_type": "Type",
                        "value": "Skeleton"
                    }
                ],
                "properties": {
                    "creators": [
                        {
                            "address": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
                            "share": 50
                        },
                        {
                            "address": "7quN8ZdQYkXyWzSpytv9xK5ngzq32UapNbFSz91tpmvA",
                            "share": 50
                        }
                    ]
                }
            }
        },
        "primarySaleHappened": false,
        "isMutable": true,
        "editionNonce": 254,
        "tokenStandard": 0
    },
    "chainId": 103
}
```

### 🔥 NFT\_setCreatorVerified

> Set the creator is verified for the NFT

{% hint style="info" %}
It is important to check this field to ensure the authenticity of the creator.&#x20;

Without certification, creators can't share royalties.
{% endhint %}

**Parameters:**

* `<string>` - the public key of the creator account
* `<object>`&#x20;
  * `mint: <string>` - the NFT mint address associated with the creator
  * `verified: <boolean>` - if verified or not (default true)

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the creator

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const mint = '77jsJBjKMi5MsKYoSBS2a2HVQ9nd7ULTZUVqKZyhRFzj';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_setCreatorVerified',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', {
            mint,
            verified: true,
        }],
    }, { auth });

    console.log(JSON.stringify(response.data));
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
            "hasPartialSign": false,
            "serialized": "PFPXCr7kuoiiw3y55ACWKYeYdCUsXw5fnQ384LuX4c8djRtgVKmT3hdNtEhMuw5YQooVLSDA33ri1exvX2mQ7GEQ2LUkHkaY7QeYL9UAcFXyAYqPL88xeFrADWzZ4H4XuAemCYwoTRNwogRNfonuZSeq8CVSzymYHZ36qVtuT28oiohoEFSQGDt9q8NzoT9QebFWojJ3Zfp4Twef1h5QeK3Y9ffamZLzqWoGHmZsJHDRhikA7HkMZXNiW5qG87xpg9mCwLQZFWBLXwhGDbiyi2YC8DvoGniieem9ftPS9m7e4ufZfZktn5wdTUnGSXrRU7jpbmRqR8Eutszig8Y1yewGYESjCWKjWFCk3fD1uzbGTEJyZjQkoUNqizREEBBUq11FeoSFreKWX3xUiEvUe7BNYBAUJKP9CvLJe7ZiD4f3voWMEjorkB4qKWXXFct7o7Tm3fJH5LAucvwpimnFkbkKA6Nou9WBUV6upUsM67u1hgKiwZj2wMxhd9t9nu3by2rHhhScjrGg3EnhyH8vpq6mf645bqubjfEDodknvy1zaswVuJQFUh8ZaCLyB3gmRB8c9xwwZHwcgfV4bs5qVMKxK4PQxR3ArYVrLLq2yXRvVexF1YuF8GQSuZ6LyNggwVBK2t8trxjtepAWNpug8B3zmwxquGPfunKtwRtNvJXkrp23TkDrCHtZSnQBGo6d5zXD7bG4sDq85H7Y1FwtEeDVQTxhcvLG46ZVBkkYKxegGBTcjLmLjk1LyqqoF6QSVmZcGHD56FPTjHZaLnzXzzNozqx9ApytuZkQcwDWV9UWsWAwWbvGtGoUWkDcDfVEKCv1BDGYcqGuSp27Fmz1Y1pVdQ4BV3sxdy3Bp4yzNRaXbJpSnASxLmn3ixSs4g1Q6c6SHoYqWnXgEmgM8VL4JpCehqmdGDCvwzi7bE9dHYZ8DLL5aCA2nJmC4L89PMBVRxxLAc3Acvn9jRhBieRoCfxMrDPjjbmx47tUN8s93e9xvkR3ssX4Pb1SyN6aKqfGH1vZz2azqdSUTGaVT5ZC7Zh7jnXSzWXQpjkZbGeKarxB6HjCJR4DmfZD7q74K7ZmbsNS9TKqMmjiffExvCRJKzmG2afjSTTnET5c2NWezZby4WbnCo",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_initializeStore

> Initialize a market that can bought and sold NFT from

**Parameters:**

* `<string>` - the public key of the account to become the market manager

**Results:**

* `<object>` - a JSON object containing:
  * `store: <string>` - the address of the market
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the market manager

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_initializeStore',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
    }, { auth });

    console.log(JSON.stringify(response.data));
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
        "store": "DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy",
        "transaction": {
            "hasPartialSign": true,
            "serialized": "RzU9o8Jk1KtrgR1uh7e4jFRhfYjvK2tyKVyWFFhvCceresDzC8kutbsxGMizCpaB7KUQLKW4nftg7cV3cWbksGmDsvJfuixghdkrfymSmjqHUVsfdqMbX9ri5GBGTGdtGeBJFCtTVQ5dTx1qevinXz2U2yndyqDHpZtRNGMj37h1xNPMbjwEoBMYciFPa7gfvJs3bogEDrJekUGExY6ksefrG71aPhWvaye1wv4K7vL5NrqnYt97RJLyVyb4fR3VAeiRjVLpJYzB3bn5G4rXJgaZQwo4L48cJLCtuYUmMRGFWKf3sWXajvsdPGG4nsVFPpAG6Q45Ujufc9JZuAWdFw4y9BEJeoustWkt1wSsrAbdjrtKWheZMLtyjq3TDycXijgPo1ZM61coxUCvMraFKH5HpA8UnrejJG2e3MCee96TV54uJbKjZY34Fn6X1uPpp9KQqg6bzScpmpKwKwBkbFXTVGybNWBQ1Htf9dCrfTSfn1JRUB1hfLUFaUNKHtjnU8DidyowmiV15Rp2C5SgSPbpgSw48uvCxZp6FMojt7YoWCeVppVruLDt5YEfGjQJBjLjFgxBpTg54DoyZ4C39",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_checkStoreHasInitialized

> Check the market has initialized

**Parameters:**

* `<string>` - the public key of the market manager account

**Results:**

* `<boolean>` - whether the market has initialized

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';

const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_checkStoreHasInitialized',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
    }, { auth });

    console.log(JSON.stringify(response.data));
})();
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": true,
    "chainId": 103
}
```

### 🔥 NFT\_setWhitelistedCreator

> Whitelist a creator in the market

{% hint style="info" %}
Only whitelisted creators can **sell** NFT in the market
{% endhint %}

**Parameters:**

* `<string>` - the public key of the market manager account
* `<object>` - config
  * `creator: <string>` - the public key of the creator account&#x20;
  * `activated: <boolean>` - whether the creator is activated

**Results:**

* `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the market manager

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';

const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_setWhitelistedCreator',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', {
            creator: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
            activated: true,
        }],
    }, { auth });

    console.log(JSON.stringify(response.data));
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
            "hasPartialSign": true,
            "serialized": "9TRcmPvqAeBVC1n1CAe64Dae6c1McF228Dhw3QQkMABYs3WMMYASYpAtJ1287QMYPsvW1ti5wGy4qJSxGkN5y6aRgT5hECfcDJ7CbfW6eqbc9AT6ckStVU5EpLLXRDi4kVfwwTHyzWaHGbQQoyJDvFfVF2jBYegE53UuRKjZGzyHBuj5wnVQ5y4jY1CUe2cjBfUZMWB7axomaAu5ASXiJLnAGnSYYbKBgoFWfXBLqjwgGqHoqQL3GQkBwoJeYmz7Cx2Gh5y3t8VSeXNiRn83Bb1VzuW89VqKe4ZW4NyHDrCRVaYWFKU6i3w48hL5bwhSNPV6jhxDZ7C7b5r15TmcANzCkKtBzPd9EkQmQUh4TEHNtp4tGMMB7CSSq5hkCGXXBXFZupM5pvEkFRLxA2jjEGf25Z1JoCXep",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_checkStoreCreatorIsActivated

> Check the creator in the market is activated

**Parameters:**

* `<string>` - the public key of the market manager account
* `<string>` - the public key of the creator account

**Results:**

* `<boolean>` - whether the creator is activated

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';

const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_checkStoreCreatorIsActivated',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
    }, { auth });

    console.log(JSON.stringify(response.data));
})();
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": true,
    "chainId": 103
}
```

### 🔥 NFT\_list

> List NFT in the market

{% hint style="info" %}
Please make sure the seller is a whitelisted creator in the market before selling
{% endhint %}

{% hint style="info" %}
Only fixed-price sales are supported so far
{% endhint %}

**Parameters:**

* `<string>` - the public key of the seller account
* `<object>`&#x20;
  * `store: <string>` - the address of the market, see [NFT\_initializeStore](nft-rpc-api.md#nft\_initializestore)
  * `mint: <string>` - the mint address of the NFT
  * `instantSalePrice: <number>` - the price you want to sell (**lamports**)

**Results:**

* `<object>` - a JSON object containing:
  * `auctionManager: string>` - the address of the auction manager to manager this sale
  * `transactions: <[object]>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) array is to be signed with the seller

{% hint style="danger" %}
It is important to make sure that all transactions are executed in array order.
{% endhint %}

👉 [How to handle the transactions from NFT\_list API](https://github.com/Particle-Network/particle-solana-nft-web-demo/blob/main/src/apis/list-nft.ts#L66)

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');
const { LAMPORTS_PER_SOL } = require('@solana/web3.js');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const store = 'DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy';
const mint = '77jsJBjKMi5MsKYoSBS2a2HVQ9nd7ULTZUVqKZyhRFzj';
const instantSalePrice = 0.1 * LAMPORTS_PER_SOL;
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_list',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', {
            store,
            mint,
            instantSalePrice,
        }],
    }, { auth });

    console.log(JSON.stringify(response.data));
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
        "auctionManager": "6tbm9pBrjzeu8P9beH5EbP1ZnsjwKWwMJ7tc5m7do25k",
        "transactions": [
            {
                "hasPartialSigned": true,
                "serialized": "4vuRdbkK2txbJVMxYSmksWfiqXNVD5oZ3SvKJszkQ2aSeXcL5Y8c8isyyPdbEwNGDQZCp3F53bPUFurJpg1AzngP6J9PrzqAZFuY1VTprqN1QESAinZDFLjc99wrx9UsKUa75FQuJM66STze2nFe7YLTLCdTBwekuTc63kKxzaQH6sEgt2VBcWxDeX46vciRcNRPAQjctQVA2QyF9H6jjtRPzENiF1nkgcM5evSocrKhN1aUAHnD7x97dRbA4f59hgF2hQ2qdUYzJHGLG8tL72Ni8pJbEzBFHacapCyfu5sJKiNTgEKtW2LVmbEbhWhEACQcCiQqy7yyHw1cJA8zN8A2M38byQeTWaFenjZDws1MwBTQwL81tybxGuAYEwuzJEcykXDUobcUN6WBQp3XGPB7SuPxFPQH2xUch4FURmRW3JooQ1fJVRXGDrKLM1HSwRjwiMPTaqzceKadcsDhhc4JLMqteanV3494MUZ1gn8gfGqe6ELJVomadkxWs4wmFB52ZLVi2EtqKosdygiVnrVPVv4gH6x",
                "signers": [
                    "22SBcejHRK4yUb97zp7W2K51qKPkso7yManu64Adyw5u4CKEJGfnCniNJqWdDbeyFHXK9k92s1jtp9P7QnfnEz98"
                ]
            },
            {
                "hasPartialSigned": true,
                "serialized": "F3SsHpaGuY6KifCoDhoaf3dUMNGKbdRjBcoNSmRDCQNv7FaPRWMFDCyoFHdRweBXV4DAp8YBh8B7i9JTqvxzxMjnymy3cErsejRajLD2BYaW3Qgt3ubfL3kdJaJCgaed2Yi2iAmrQG8ovFuRdZ687sKr1Xt5vSrszRfFsXXNTzvWPyqMEHtTB7hCqyzvJF7nMs2GDkoiWEYCGwsPB7tM6iJY6uU8B9n495VUBPjezM3cWMoFUBXe3t5T7ygnxkbF7pbwLB4MWavFSuXUXqtNiJkj56MTqJXj1Zni4puQTfxRsrNqErSWHUsTRurLGKzB4Zy2VD3Sna5s6BEANCMEnFfDnEQj7xyB8gfKHMCcC9GauKQtz6a2brMBWg7JS74zahjcEZ3SfMyV8aWeVBVYytTmxVdUn3jd3wEUSW2oDvPnd1CypbRrNSpryWjCVLboDHtRd19wGk3jJCpRN3iXykM6rQ5VRJt6QRscCfvx4PEZ667RpGLr6y2p2e8jsNU6qypY38RcfiT8oSgf4XtNPjyW2K47r3RbRY6ey47rvhWd4CRsVREoCz2eHDqL8CA6k7gi2hzjvq5TDtPagx3NzpLadiXgDjrdFF2SuPVtFNCkw1kLEgeUd1Z2FeoR8RZWYEbZ511RP6b25Pr9YCGskY2XXHLtnGVkofJKFp37DJvwSRBtaS2hqrjeFTHLwiq5V2N4TYcr4LpxNngvsoBWcC4uvkjAEFFCGJxscWZ6irCS6FjkLq5BfWavEegyZCPFdfMF4X7P2tNtd4bcnXYfWCNKWjVvrRzaZXf6P2EsU7GExqrsBZQWa9awrXfHiV8H6a2NPVnTwq2asi8aaGfxqVDwyqBSXeDdrvxzhLp8ZW4TrhRjNv4GP5dK2ouHw1n7Wba1ZY2zK7vsXjbkmF9QCzdXBAEfWqoHRB9zPSkp6Xbg9ZJuKQVQ4ySiSuTupPgZkoekvoLaxUK6rn4ayu8r8DB9wJFENJmBQ6ETk8Fee5UZMH2pVYBibYWTz9gxVregVqVerqxt3qbjBkAqALAADQKnF4q5peyLFzqVx7ZQBiQ3HiAW53hoZiECer81U8MYDirnKbrjEgzbn3kscsxGJq3TANdQK9aGRnxEdTd5ZGTQLhQBS9dW4Sbi3RgGcCg1entSNVFndqwpS5NGSQbwW6iBxSewhqEXGvbiHF9sAsiu3FXeqGWTCBLAGs6Lb4S7yDdyREsAXqrLJ4WadYtyTdm9uPgTCYNfvUfNNAauzBiBQdYmb81pjpREmw9kCwZBrHLsybGLPLzn4vjSicUHrMX7ow1dRwpt3CfcT2LkHuTriB1ieWvHcFS85WrCjeCUPtXV3BzheRorKsWAkh3inK8WWfZRHVovqmHy8wq2saKu1rQ2LenFHczfUQowSaCD2Pfb8xRnzm5LUYMzhAWfxw5sTiDBLXcvXbozyBEt4gGMaTsEGojuCb",
                "signers": [
                    "5EyDBhqzaHsLypmNFXxjjkLjR6FtuX9oYyDzAN691ZrHp7xffTitTBp4mbwcjBbWpV3GB42KeBKyXVCiLWJdCLo2",
                    "422iFqfHJk8xLiZK9EPgpvEYLPJBUovxAxEksSi4dDtNNRf8LywuFesMw8oG3VQVdwBfYu6KfWJM444ot9WSpUAM",
                    "5zG3rQPaPoWmTnmVzwgTg2pUbVswWXrrawmDAN6p6CcLCuvgdhftmxPWqjd2b5RET8BEpFmcavBsHFrhtJDHw7Jm",
                    "rsrrqrC8NJ8wsxijiUeUvL6DuybBXQJL4AXiX8wVBQDRjcAZ3feRzqzvyrq4NwCj34WHoWpG7iCWAFK1sdeF9Qp"
                ]
            },
            {
                "hasPartialSigned": true,
                "serialized": "KKgUM2SeJ6WXC4eYD3hHy23X5bueXZX9GJhawprtj9ZVwQhrv3Q75NEwGDc77zX83g9c3cWJMs91cMWDmxM3YNaSr6kpejrLjzvv6NEGXA8m863wjAMJCBaZ6xY9jBMVXNyhpeGbiyti2knLL2Ne56hXLJpW5cyvcAhbCLwBpLq12Yvgijkexiy9Rv5vgTTQR4H1pKc68PXQn1CdG29RKyUqb1CzwNQB5imBcJEhrB3prW4JnCpPTarKXKPWGgXEVAgmAVQStPy9zyqykrCa5iXinaeQA4n4ibQ4jVM362dysCNz5cJkPgrc8a7BSjK17K9RcKkh6pyZ9ydVmusvRtZPznmU8VdiiywrJVkFSFD3L73iSuGFwwF5eFZbuzCh5pEsAjAebqXc1ohJpMbWXdVBeGWTqPyJXiU7XAiPwhMPN4tpwpuM1fBJCgzKgJ5yNyQXi1dPMvUFeaf9yUeA3PjbJEBQ6ouHosgPDkXLkKP89LdTwqTD2BjWr36ejdfXpNz7SQuYUw9EnAnVavNBNpPc27q9kjv289MpFvQVHumDSDVmMCjcb6E9uRb7yX2We6UvsJs7KbZNXYBXMLiSecXBmkmTRKQhTqTcM7tWVUNRKeG2yZoFcj3ourQkVE5jotybYgM6fpHT7t7qwnjSHwUyGEzgaxkrh3bTbjFVbdcZdYmCvEDjtP3T3QxGRpCq3xWTEE69yHKnPeLxYaTe8bWwNPfwaczC5CAgb1VNUS8ZrSAT9a9bs5RpnfNuhRiYqQPkdbWVNQ3cYKFVRitAeksa6EmUtyxSMqDiZ4iJBcpLCikKLPUXVbhLDA3SXXtYhhGmkAF2xSNk9fZcjY6rJKiGu6ivzven15S2ucC4o3LZnXY1BgtxuY5aNpmVE66vm6UfcesMsDQKXZJJmHEeap8qjXE3GGoVedqhkFAEQReRqmfbv6dWzq1EZonSx6M4NgQNgVtLaEpuiWaC7",
                "signers": [
                    "YtHuhEc38c4CDDDLkhVi9FVNhEER6z5mqwHeYMmtcQRNsphrm6Bi7v8nv6HLUZ5dMb1BBcbBU3zrRtU8xc4161U",
                    "3NNGBnd6y9xj9kfwkRwbuhHEvFb6syPBvn9FdDmcjZehF58EpcyWZeW9KxzRqUchWBMZbJuGGjA9UoT9YSjYDTqJ"
                ]
            },
            {
                "hasPartialSigned": true,
                "serialized": "mB2JfmyPoX2nwKPJVz1HZVRQmzU11uzhdNAYoCYQGHf2nbkZKcdhH9xTnZbjUZ3y3FJ25uzyYqCkZnQYRXvdqEsHQUroVBXdjCADg92rzFGvnVMoW2LLAzJrt2qHUCFzeRCeJiiTjqdLVPC3LDJou3R6ZRGiTXJYRuLtQdiTgxtvcqJv4yfULvhhyzPWfWdsmwQfTC1djwaZVKHY3dFkZ1v2QgtNNRCifsQhiGsCKgjQKHRCvDMUoHp67Gu3jQszX9yJd7s4uwL6sUJwZVwkHbd8pE6JCR8udYxCPqtUgJmt4kXra8QiiiZ6Qn3ud1v7ichaaCUg9vqb44r9cbdT37dRTdytB1J47zEu9EEjaCM8AwGz3UMAFTY94JFYosJzZQv7PgiU5ZtgTFEPy7YkaCGYDABVFf1onBvc8AJ3Ex9UNYJJkrbit36Lo4em4uW1GzZzytvC2sw1qbxCTTpCDSLmCwWi2kvB1v1qLh6qBpRy3bJ2p8mZWymBmZV7XbHpCRY1B4wSK8cHfKfEKgTjnrJo9nTnm9sGBjtQqocDEHaCBQ65471afp5XDpKkTxcfKJ1JtGVvKV27AcVDAU99rwo9uc3EhZg5rNkCcP8nXeowRYBoQ2m8JDaRW43611hZ8eWkAkSps1Z2MKmYfwtRykSFyN4SrzqkoHsQ72AJWkGEdZc63JpYQpAgt2zzrNfnZ7bgQ8mebHRJLzLxdZiLEFNWpLhkExNYaHKnoSPeYB8xWXm2FJF23Y8tbroy6pzAXhhm8j7KZWMgCMmXViLX7FbgSg7vCDh2F5Qdsp4bZyCcKfJehnTCpwREbSrGT8MnHW7Q7GcX9F6G8oQwLGHBLGjd4ThCHW25LYwe393NYBeqdhraDKHctFDJcvJnrHpqeXFCjY8AU9QeQvYmgxBahEEmpboYxV4YzPia9Lr25TS4fhyvCTz7TMqJ9mjkcQUxQsg3vhWgUfGso8BnwCcuEkCFUoMtCmJkdfUbj96s4oTstuA7sRRrbKXH8pBKuHqq2dbtxvdSC8fRmhHkkXxaGuWRiCxXPombvaDBVsn2Ejx2HS25zVEzpDWcewB5Ua8BrBDQPkGLwPbaLTDaJLE2M6JR4BDdFSm4W8h2kfG63KwqeNh7ktXdnCEeDfp1VViiSZML5Cr4GJCVKQ34fXEQFuVLgCiKVcgCHygbGMxUEHMfSiLyESCYZ6dRDQvtJa3cTBJCMz3BvcsnLrqJMBFMm96e91LMM9m7aXjPGt3z7HDaEThBoYEy6PuYVbVeCPcpgfkVcf4BVD9duWJhJJioMBi1Y2SogsBdsBogMuQaE",
                "signers": [
                    "5SRPfKRTFQveZCWqkzone8f6WpCM12yrx6H5eKwrLaEqNaoJeTBuUGWiwaHpuJfEyswx41XqYHey89a3EniBk17t",
                    "ypRu15q3a4qmEE1vBFyVb86iJNqX2egYarJEoV3Mp41dFQ6W43GpRchVDrpd6Ppxw2MDZNVCfvQVxhCBH2EBVcE",
                    "673KJHBRhNEw8A9L7CFiyZnX8mD67uZvqd15ksEeL7tRoMrNkxRA4xYPyeuUX52aSqLa1BmDGrJqFTgkwGaWmBvC"
                ]
            },
            {
                "hasPartialSigned": true,
                "serialized": "2Vr8qpbX8Wafp5sKe2FvcrdkjupUF31wKRLpDoQH1rQHZGu5pdkJd3fGufZSWq3KVMS18iY7yC8EAmYuUsL3Cm6sm9tC1evUN6GDe1AeRmVv8KD51McEt3vPBUmKBxWkpUKpgEwuCBSLP3ZHueJvcTsANv6Pu4afTeTryU8FXnfQbMyD5TH1o137eVqkcp5uaoDrX2V5NUoUTG6PH2oWiCXSHtauikkDeAAR5BwivsFh85KTwAj4FqT2eViXRMk3zG9Kmo6342gtn5gUFyu174ZppEzbxsC4ydaXTaVw3kMzAyEwMMQo3vKRZ4M5wQ93dvSSufJkUfQgiSAjYJWo8sfhXaBbmmAxtYDytjG5tue1w1DFzjTB6zHymFmX7mZm9YsKp4EtWhxSEj1bCgCyTPJH9sjwUiuJeYWE7fvMh4PWWF6hF9hd2vsGiLusPzm2GVLQd2scbLE1shtCYyDYNHqFHC3238zis2tcufwHw8qV1ZQ9N74i6mNpvmpk2e5JzxPLg7YdNX6eFozZJsX2u7rTSBsFyofSL74ieEAsxibFKHYU1fgVYTVCaa1M95wjUqBmPktrNR28bTPugYpu9kHgEn1T3yHUm9YA23EBL3dbiT1FYfzq8FkZ8EsFTZpggor1B25DnxyymZUghEz2SahFURRDc71pfGymuYXJYLwKDHUQVtdvLLXy8XQ4fwqFHZvxuPy4eRxmsFsywi1Nt2rR59tbXbFysNJ6m4njPhtc1Ev9ZWYy8iDvf8ZvPTC49wQeYmnWriEPRGzdou6EwyEZ4R1idTfaGYxiUbip4E4hVgvzQFQLgC8xdkmrtXNbMNruM9H5qjBrWGEBLJBjMD7bgpkhe7iYMrFE9vuoLLX1UFbZPfpNaHyu8Xu6a9H5oopu2AnxUp1Hdou5CAXVaDYUv6tHQZ2VdmQH8LKwHuJRKRp63pEcxu9stGi4q2THc5xbLwnc3UX7sd68p62hBNqE3kACndX5FncjvPFwyeZwMi3vUxfKQrXuJoGuUJ4pcYMCx2zG3GJqwT91TtpgwHWh51yV2cYCrWSWFgF6Q2jug8KngN6ftnqxoc742XosBAivnHFvXGmqghmxprkKAiamJBmeqn5aj6y5WxpktEzYgR41jqdBwqo4W4PVo6ftEBeX4d52eLht5eFCPcxUTwqknHYij",
                "signers": [
                    "2yGDnjyhxQ6ke3qwiijjHdsxKZQzjMKTwQLgbLuqoQSJKjGMifyksxKRXEkaLChR8FZRwuQYxMfvpWbGKnrj3SN6"
                ]
            },
            {
                "hasPartialSigned": false,
                "serialized": "CnZtHzPTaZUa75Jd96FzQqnyvjWLZVau6ikBB7Koc6ubULQY1htngEFfxBU8xxknyMZPb7HGCHzmPe81Qk6758ks93wsrm3sdsnjpdHacQ7qFrhnxREzm33Zg2QRx2yXXjAiKDTvbCKujJvDQ3BUx9Yp5LHtADjiitDPiURBa5PvmZZ2DGMbJn6w5WoGakt2Ge5fHZ7QtpwaGkRFYZ4jXRTBFJMetcEc2pP76iSwtcUpUdEymRcqxbppgmJEm3zxf4aK9G5eNpxmjBvC74xS6TSxML3BST9osrnWUguKDbbtbctFezdCJMV9bAx1Fq53wgj6vykG9LrTFoJ2iU41dNCoZcBcB2r5DLxY8zZM4deh7RrUDjp2BiXYrZ1wq1AwLTuaATwKFHYPQPVriar3D6N2Hg37FaHDsrTrFTa4N6YxExWv5cVDNGnMxzdedt9cM6vJMMZbYtN45qUyYCrXdZA6PgfyTVYHGxHHfSQoctRHghPuDwpam5qGSNQKs7ZfLWtTp8k8oXhitHdVoqSUjouBYc5D5YnsDy51b1ofGKH3edVr27cik9JUUjwZKi7NUteBS6XbtUPNpiPRFJuA3VKg7HVK5gEFy55E9d2UhRu7taLkujgoJaxryRhJwnB5DSSkwqaNsSpQqdukeHYQQUJxMKvRsMZb9aouoSF24UQxFVf5rNc3pE6tdy7McKmRCN6DLMKn3m3SEC2qujEWoKcF5RC5NtTgDk1XcdCWfqzpDMGVT4qh2BnXkuqcEHgMHSDvT4qxxwmmNMhRRz8Ru92X9eXr7awNt8wY4JVhnRSP2hEmjM3uzs59AeW7mnJZY32puYLy8RsxbPVXe25B8kh1jMuAQhVoxw8uCLyYQwzUvjmrrBspZaNnLRLhPd1Rq5nxFgSa4UU76McJfc6wHKxKWuktvAD259QCoqgCn4xVRDEbzQykgA6Z1tyH6RMyhVUJoTdLgj4aaLRdbL5JW1xxpM93F4JYKNzGAEZHZyJqdQATEJLBFFLyjKzKW4Y4rgn6YUXZvdqw15DerB5y2fApNbFf18aG3513UkUQh4mcAeTnN94UFc3eJH72AHE5q3BxKGzuPmb7t5t5YABRtg5AZTQh9k7KbxU1yHqpTsFqfUmeG7KYeKVYnrteNdTyCw2p6Me5Q9gH5FkLZ9Jbw2qXF1ML6YkSyPcvigfrSSv4KDsNVddCbQCuS",
                "signers": []
            }
        ]
    },
    "chainId": 103
}
```

### 🔥 NFT\_unlist

> Unlist NFT which is listed in the market

**Parameters:**

* `<object>`&#x20;
  * `mint: <string>` - the mint address of the NFT
  * `auctionManager: <string>` - the address of the auction manager

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the seller

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const auctionManager = 'AaQEaBX67R4eJLmvN96izgmwYPNebzKKbt746tfNXQJT';
const mint = '6HeKjfSt9rYWatGmL4g8sqQd2hoU8mPXkGFhFkEzzka7';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_unlist',
        params: [
            {
                auctionManager,
                mint,
            }
        ],
    }, { auth });

    console.log(JSON.stringify(response.data));
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
            "hasPartialSigned": false,
            "serialized": "3kzXxRTzH5SR7Y82QoNf1q4ccLD1Bjkx3BLsvMDhj1zbsr5x9gumbxYcyHPeWeSHQdW4TacHkLBBSFtS8wi2dY6xwJYcYkXk5ejA8PbXZzZ7sJ8wTNowTMEVKrWUSSkd6dRFKeoNLYChbmNtzTvuCnDEfJ1ENgktTLuLpVRugUJfXzcc8Lx336Mco1z5wFpGvXAZaQokeofsmxX8yA3C2e3oiNv3CSFm9c5f6d9WKEDK542hZwu72tF9zxKmdWTMNnfaoaEoX7pfA8moZ3SCdpFRAj1czdBZEEKEVLGd6W8DAVMoEs11EWZQEt3e1T5PcC4pULTBKW18LsCJDjCJ8VBTQhvUMro5noVqFw5x4pGqZT56A17KEL1iXs2PJPjYDPGY7A81ouVWXaass584Rh5qHcrGCpZWjwYvaJEcTZ31c4vVGVcsfZPUMUVqa2EXTPcVsRvRtmWoKb4Eea6hcNQvvHWq2D8X43q9hfb8HY3qW6oUmURikjuu6HVT3QZFN1FJPAWPxQ2sh1WDZxnqBJkgvTAmVWUbdixcCoxra5MsnnRXWwfrjMaGpWKCrw4mkGbhytyqqpMyPP6QR4cE1Fmm5LaycoHmY4ZK5VG2KLQ9d2S5j4vRrKPTV7Ucwm4zmKiXaN4hQyyL2CcFUAQqbR89qHb8yN1kDgGaTeag1bc9WaeD5WshCXPsH5uvuJMte4gW3phCNwg5dCbyZm1ePm2vYXy9g6uNcNQCFMCHrqJPUm46q5BDPzQrEqboPnc5VjfN87bW8KgAUbTBMMHUkaqkyEhg7McSgerrbg5ohaaukMhFABaquVL3kedCzWyzSwnNTmxKHiuuYrnctHNc1yDBe2woRZEjEySvAhvyWLjikDfs1FZEg42anPHV4zDviBQmsmJqESbfb32Ys7xkwC5C6xSJELGQnccTufErUYSY6dTrWPKD1xkQWpBP2sP2gVqd3sZEaV6Ukc1aWxLuXSUxS8Ss7EzH2XG2fQ8hARphB6dmk3a5ukMXYs1b3g7KurZmJYkHHjEsLFtkE9RwUPZL7MokzfGqMfL1CAT8LWJq8UKmRmbUhStWm1N7u5KVvxannDaZbQYiUjDxL68of8hzDm8T1mxswwYvDRmdCR9AtNbLvuWerHunagR3BNmd",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_buy

> Buy NFT which is listed in the market

{% hint style="info" %}
When the buyer buys the NFT, the seller needs to settle first to get the money.

See [NFT\_settle](nft-rpc-api.md#nft\_unlist-1)
{% endhint %}

**Parameters:**

* `<string>` - the public key of the buyer (can be anyone)
* `<object>`&#x20;
  * `mint: <string>` - the mint address of the NFT
  * `auctionManager: <string>` - the address of the auction manager

**Results:**

* `<object>` - a JSON object containing:
  * `mintAssociatedTokenAccount: <string>` - The address of the account where the buyer places the NFT
  * `transactions: <[object]>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) array is to be signed with the buyer

{% hint style="danger" %}
It is important to make sure that all transactions are executed in array order.
{% endhint %}

👉 [How to handle the transactions from NFT\_buy API](https://github.com/Particle-Network/particle-solana-nft-web-demo/blob/main/src/apis/buy-nft.ts#L76)

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const mint = '5NwzTC2zNdDzvKSc1CaKKFm1HNZ79KNop4yuJajRBPQg';
const auctionManager = '5QzJprZ2o5M2kbmBoZX2mCuNGWzMkMpjKMxWUHv3WzGf';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_buy',
        params: [
            '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
            {
                mint,
                auctionManager,
            }
        ],
    }, { auth });

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
        "mintAssociatedTokenAccount": "742fsoZhH6bs4DNArVafiEfmLbmj8fhrrqpuHPgYMGtD",
        "transactions": [
            {
                "hasPartialSigned": true,
                "serialized": "2BjJ642HjF4RpzaB5DqYZg5vVL4cdRjmjuErHfKARPD48SGR9WzxCorRBXmQTq3eWLusmwEuuJKHjXFo7dGMfAJMXWysAoTqyiRpKF8uCFu2Uy5H8KM8g8m92FwpwvM2MMthDiGLZt8Z5PpPfRMc5uDuAN8RQn25eznH79BAHaPd2oQCtrKWogjLrAbfbFdKoAsc1uH1bRSSvisENTQ7WDZRTmzvF421qeeRLPQR9mtfYJDuzQT7G3KRFEkDKyqA2vUFyBSnXfy6ZENf6Lw9cD5JS77fr962f58eGwiGZZ2VF8txiDTgH9eMDeuULYS3PjuqXjGaJrczyHGbUhg7hLJ8KkqetMpSBu7h8vpi8nKZ66pydn8WXLkygTDPduGUhxGvgyL5prJP7TTMpDVWkeJcNgE2xvLpZ7CWqn8jngFGVRmwbqQbbPT3ZFFesho7MFi5TEBrxWSK4fHRioFPF8XACmrVEteSo7XpKMoLNwhCGtoE5pxHvQKnQJcfMvr5R7GKB4XugPjBz4MKU6dEDEepoZ9R2AUYdafXX8JgRBNxLAw2c1N32nunJZeR3WgRdUkrCt9frs3ayN1rLUwfeVFa6Pyjb6eiVQ7AxT7RqMDh8s7MWVHA4wKz7iXJo56xVb4qUvSoxhGPntD8fHL3UtmrFdeFWtUjGuyvQr8vtwVdAY7YgYQi7FmrKKvRMubVvYFVRuJfsjBW1KX7oJQnKN4FfgTs8ogb6K2R8kmrYmsM9pEoBtXqRdxstTPMh87dFhiM4oUxy2t75hKjBKp4L4WxvAmb1ivr9JiNbUbkNXPnHboQBv6hRLN7f5VjaajrLUjsZSEEeZqUjgmZnpMf9hrLyrjsQWgX5F3fdWedhJxCN2VVivvFYuaL3GDXoortHPKNQKzX1W9gYv3GCK6kuT3o84GiEUomqq8z6LQuvhdSiVqj6TeYYeT7N9rSjTofMbbuMLe56uXSggDbDLBXCcXy9ZR3enDZwp91wabB6NVoGp6h5budCz7saU9zfxysP6SwP3AQSg1rZ5egHQvEGicY1hHhM12zDXQCgAMqDkfrL2mvz8EwYhzT4dDz8dWJPTjr3Hr1NEaSVpQNKJQCb9nspNyuDU8cwDcLjDajMVYg8qgAd9vqGjMeotczm1yUNRKr2cmQ6xt1jAWhRFLWwD3yxV2jBApCNH8XRaYQjr3Zfbb99kkvGXdBSaiBEnqj4Ptn9gVPcau6PBNsH3R9iikLVSPCJoRafRwZxKpgfH1",
                "signers": [
                    "3NAmHNTq4cdfRZJiUZusGfyZcKmKNNCYP8umgzDwyXAGEzZjp3iZepUf99uVvjEsc6q7ZHd99zCZTmeZh1v21aB8",
                    "t2z1U2Hd6JnDvBxeffJwEdFCjVeSnHN9kFN1GanyQtSPvqEeTbDBSTvPCKMQaFfaYrGjgMaZ3q6cBRKeKpnFxqj"
                ]
            },
            {
                "hasPartialSigned": true,
                "serialized": "6Kk7yb9v4bTprXd4JYJKddFhcXXy3aYfPpY6QktGp6KyyiVtR1FdkU1vDSPfjmGfMXQVnpBBKdcvDNnYDVyMKRxYZa9NrKnNn4susr6zoShDoVJVJ1mQsPzLyxZ4MK2D39PRfCDJXNfPMCqDVdLJ9dS22ptEz2rKk6vZes7QidWR7rV7K7k9sPmwks9zzcLdXfZ7nXhCPdGYcayS9AD4pxy8T8Kxq78NGU5Um4epwp8F3Wb2vVKRNJDbrR2iEvmhTkPM4AHH7YhT6Mti8qwKSd6uamG3vnPCD9LZDguALvVL6h8Rd9rSfnNvZw6Y3i4StyHAK61LHZYKPH8vRBhiAby4ouxv2mKpmiJrA98Hf8rC7RHa8LciMpN3RKq5PN3wgx4D1uifvHpBWwgSLd3FmW1JScF1weg9vsP4CAbFDi1Z5QdVmjVwQH2VEtFUZdeQAhibkbkUdkA6n45BHhrMx96kduYKNTomfhyhPLm5aCYcm2wgHBp7B6Uhmm8cg3dE4yAodeY6XebDkxNV7afScbLAAMvXtcfMdVcK7jnWnDJ2TS9wcDGBQidRhrnSTmrMgwmMD8rT3NVrgfSkxQk6xaGs7So645GusoeEoP2KFWmrnVKdVfSMUXcwtCGRGPXsC58qB45oFRrtu1oNWHd3HVcVqL9ZDPpSRvddHsAzALMD1JFRTcQe9e7crKB28H3kJvx8u7pcasCLsRJkqjVqkQWSSe2fNBLTmHE2Esc6gmHTjqsUuAQy5qAAUtjsp3SJocpu2GQSstqTCnujuTH75XcT6xMdDHLaCSpjp5gYrWnCpaExWtS3rvDGmLfHEJwKGkHEKyhNgb2CRiZGJPr7eVxXDknoCAC9iH4ZsQnLq56YeZQ6AKcxJtmumYmYm1NdhESwCe26B1v57uZ5mK65kNJKcPdcMzz8Vi4qiquW3tkZTPtt5AjMipkbEV4vh9YBChgGdmXmnTaNE4MYPJhiA11gwojMibCeL9DAoWqJ6stEErdUmsm2YPRq2Ewt1XYCRfBUr1GSUinz7TVJbS2hNWydsw9JHRBanniZBEWMRAkKkaS45wmr3Y9WPWtiqRZE3taewQqs52TK7tRDPrFJXuaPVTbjAXTL6mmCcPPUf8yuMwiMtAojuc3SapS75muDHJnRApmtLqRSqr5k4mzjYCd8jWxdb5i2aC1aEZkbhTMpojugJQDwZhu4m7DgSko1gvEANLRQ6ZrkfssiVGbctbdoKBLmEFQJo2aWN4jsi3Y6d8Ag9UG7avHsVLWYV7HnMwmmd72AtpmzNprmtXk8uz4SngZEufiQ7iRF2e8Ny1hvk1XZg1Aaa6C4Tx7d71ZSy4kdtdUv3oHC78668rmEzr67z6Nki76csYqkyLrF",
                "signers": [
                    "3NAmHNTq4cdfRZJiUZusGfyZcKmKNNCYP8umgzDwyXAGEzZjp3iZepUf99uVvjEsc6q7ZHd99zCZTmeZh1v21aB8",
                    "t2z1U2Hd6JnDvBxeffJwEdFCjVeSnHN9kFN1GanyQtSPvqEeTbDBSTvPCKMQaFfaYrGjgMaZ3q6cBRKeKpnFxqj"
                ]
            }
        ]
    },
    "chainId": 103
}
```

### 🔥 NFT\_settle

> Settle your NFT auction and transfer sol to your **sol token** **account**

{% hint style="info" %}
Sol token account is not your native account.

It is a specially generated account for the auction, and all the sol obtained from the auction will be transferred to this account first.

If you want to withdraw from sol token account to your native account, see [NFT\_withdraw](nft-rpc-api.md#nft\_settle-1).
{% endhint %}

**Parameters:**

* `<string>` - Stakeholders in NFTs can be creators or sellers
* `<object>`&#x20;
  * `mint: <string>` - the mint address of the NFT
  * `auctionManager: <string>` - the address of the auction manager

**Results:**

* `<object>` - a JSON object containing:
  * `nativeMintAssociatedTokenAccount: <string>` - the address of your sol token account
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the stakeholder

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const auctionManager = '6YKFom9PerD8pcWY7KMd1uX5MkYDFTcQ9H4vjN5jLW51';
const mint = '2J48iL7bW1d7WR7meBsRkhwmhP5jHHVuwo8A3nEcxrAR';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_settle',
        params: [
            '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
            {
                auctionManager,
                mint,
            }
        ],
    }, { auth });

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
        "nativeMintAssociatedTokenAccount": "BLXQ15eimFb7mqRmZ7t2PpDmfN8uAK5PZFX3aPnMEp7t",
        "transaction": {
            "hasPartialSigned": false,
            "serialized": "3FE6hRzWgvbA7dAs3tz9shFgUAaVPnnoXym55eLahDUdFw1rg3NfxRqg3wZw4pmy4oqboX6vZHMJKoy5yqJuoffohxEuuzFxjFdKg4pUgVG8hjkkBS4FayFkohJhmNf1KM2TGXcegshGTXsLc9Npe5WsoW7ZpuRzDJ7NMFqJuq2AfwojXbDYb1Es8iS99tVW51HVV2EipkuHHDVrwhZrWWPZRAGYTYKHey9gP7M1fAvF25fP6EvRqLynVFwVSF77dB5rXq1An1yU637Qmepda8T4xApTPX6tSWZAxNV8DmF5H9HNbNvrkommUXmv8jXxxC97ijcQykEzpQNDF9DTvc67paDjgoXTu1939fJt6YuLexvoXe3f7fyBd4Xf6WyQuUN8u3uwq3iGcTpwwdL4iQTBur2XVPcJ3mUQ89Pn5awuRzVewHxQzKAkJ9s3KHrdzZNxtTE4X4UYy21Y7f3dyd2qKgReDMg85NULCE5ge1AnUMXNqnTUfv1tMYM8VQKKyVtBVuUg4tWNybta4M4QXS66vJ9TiSX9nLNdhBd3fWk3DYmKdQg8xNfJqbdJRPYbQUCRjSKKHE9byxjuFRLUiAekdDPVAxGUqy9kwdRjc5tTxzUrp6e9yh4B18ktMMmPaVrK5rZZ8GDxkUk15gvm3hSUDFfjCXstPbdxwJ3L5YQ2oYMZzU6eH91rEm1bWirr1bYdAkfcGA2mTTQaD7CxFdxmjG7g4DigmykTBtGGPJxpBiRmSyQVQcRKVe1dyRAdZHJnWTzvzMhejyQ6ne1wHY6T9NRpQeF5zckRvKSVh1guXvkrvXmSnnqrkgUZxVp1ooNdqPkaDBds4UHyQ9Q3U2UDFY96BAvCJXz1sJVLi6tq6kYSSnfDkSZny6n1yUJ7Jy63fGBdPjnXUCv9ni4kffJuircyzfAFDrknKzLa7rWNpia8V1GFu6Bt1tgDwzn2okJVNSS4UERCjpsKQZGTQzyFETkeFaBkFcb",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_withdraw

> Withdraw from sol token account to native account

**Parameters:**

* `<string>` - the public key of the account to withdraw

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the withdrawn account

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_withdraw',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
    }, { auth });

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
            "hasPartialSigned": false,
            "serialized": "3FE6hRzWgvbA7dAs3tz9shFgUAaVPnnoXym55eLahDUdFw1rg3NfxRqg3wZw4pmy4oqboX6vZHMJKoy5yqJuoffohxEuuzFxjFdKg4pUgVG8hjkkBS4FayFkohJhmNf1KM2TGXcegshGTXsLc9Npe5WsoW7ZpuRzDJ7NMFqJuq2AfwojXbDYb1Es8iS99tVW51HVV2EipkuHHDVrwhZrWWPZRAGYTYKHey9gP7M1fAvF25fP6EvRqLynVFwVSF77dB5rXq1An1yU637Qmepda8T4xApTPX6tSWZAxNV8DmF5H9HNbNvrkommUXmv8jXxxC97ijcQykEzpQNDF9DTvc67paDjgoXTu1939fJt6YuLexvoXe3f7fyBd4Xf6WyQuUN8u3uwq3iGcTpwwdL4iQTBur2XVPcJ3mUQ89Pn5awuRzVewHxQzKAkJ9s3KHrdzZNxtTE4X4UYy21Y7f3dyd2qKgReDMg85NULCE5ge1AnUMXNqnTUfv1tMYM8VQKKyVtBVuUg4tWNybta4M4QXS66vJ9TiSX9nLNdhBd3fWk3DYmKdQg8xNfJqbdJRPYbQUCRjSKKHE9byxjuFRLUiAekdDPVAxGUqy9kwdRjc5tTxzUrp6e9yh4B18ktMMmPaVrK5rZZ8GDxkUk15gvm3hSUDFfjCXstPbdxwJ3L5YQ2oYMZzU6eH91rEm1bWirr1bYdAkfcGA2mTTQaD7CxFdxmjG7g4DigmykTBtGGPJxpBiRmSyQVQcRKVe1dyRAdZHJnWTzvzMhejyQ6ne1wHY6T9NRpQeF5zckRvKSVh1guXvkrvXmSnnqrkgUZxVp1ooNdqPkaDBds4UHyQ9Q3U2UDFY96BAvCJXz1sJVLi6tq6kYSSnfDkSZny6n1yUJ7Jy63fGBdPjnXUCv9ni4kffJuircyzfAFDrknKzLa7rWNpia8V1GFu6Bt1tgDwzn2okJVNSS4UERCjpsKQZGTQzyFETkeFaBkFcb",
            "signers": []
        }
    },
    "chainId": 103
}
```


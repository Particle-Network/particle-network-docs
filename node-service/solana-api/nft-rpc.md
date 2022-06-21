---
description: >-
  Mint, Update, Sell, Buy, Settle, Withdraw..., Easily build a fully functional
  Market!!!
---

# 💥 NFT RPC

We implemented a series of APIs, from mint NFT to building the NFT market. You can find all the features you want here.

{% hint style="info" %}
The NFT Market-related API is based on [the metaplex protocol](https://docs.metaplex.com/). Metaplex protocol is one of the more popular standards in the Solana Network.
{% endhint %}

### 🔥 NFT\_mint

> mint NFT by metadata or offering a metadata uri

**Parameters:**

* `<string>` - the public key of account to mint, as base-58 encoded string
* `<object>` - metadata. Follow [The Non-Fungible Standard](https://docs.metaplex.com/programs/token-metadata/token-standard#the-non-fungible-standard)
  * you can also offer a `metadata_uri` which fetch and parse as a metadata JSON instead of a metadata object.

**Results:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of NFT
  * `mintAssociatedTokenAccount: <string>` - The address of the account where you place the NFT
  * `transaction: <object>` - [the transaction struct](transaction-struct.md) (to be signed)

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
        jsonrpc: '2.0',
        id: 0,
        method: 'NFT_mint',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', metadata],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(JSON.stringify(response1.data));

    // use url
    const response2 = await axios.post('https://api.particle.network/solana/rpc', {
        chainId: 103,
        jsonrpc: '2.0',
        id: 0,
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

    console.log(JSON.stringify(response2.data));
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


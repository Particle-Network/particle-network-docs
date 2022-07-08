---
description: >-
  Mint, Update, Sell, Buy, Settle, Withdraw..., Easily build a fully functional
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
                        name: 'Test: 6VF5h',
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
        "mint": "6Gttjpk1LxMKwdiLt5q9TSW9hfMuJmdeuVXDrcPyB7YY",
        "mintAssociatedTokenAccount": "E88EfbbPMKoJny8ABdoeti4A1S91TupwP44dwLy7grnT",
        "transaction": {
            "isPartialSigned": true,
            "serialized": "3upXthSvHwVVCdrM9euWVNhZRZUekYgt1BxoHaEGWRwyZkVk5Ro3BbVNEJjDMMCthVYZc2YCFeScwL3cAHtftWw7pJQV2HXzBBZtTEdsFZ1FD9HnSnDfQWjDavSMPpbe4XXe251LP2TBvqTZFYejpVXCipey3dPER27Q6fEYrdNyCFhPpxREvxL64pwiNry4QHxD74LLeoceGmTsfRjK3Bm6u32yd1uf22do1hW6QRMdHQgWAJPnWRj4BAgn76U4gTv55GwqnXiyfQ26ESSrAXkibEsLwBoEt1LQuU8EtjxUD8oyt2WXHhfRjzMaCUHJnCEcgSPr6zzwQLwW6KVQb2V84Sis7ZiyFFgFtwLqcJ1jui1cxtDdq6b3grSgBHZhv3yEMbe3jnJZUcz9iJ3gcQqtqJSBDibVBURf9NSn48BGLyLaRN8bF4sn4nfCTjwboK7mRYKmsUK2Evgj14a8b22rpckuRHRtqyFdrRyHmaGwePTpZGrZ5Ad3hnCRzksqJNsD87czjy3himvtv86jforU4WHkXzFfnBpKwxFs57oXzR2mViXog1r2hCWcschV8AMGdwPsFbfUYWaD5Aqp8nXapFfakF1aBYfH69BPBAuodhxgTQHa1nv2MaBqArDnDQT7eg1bx3ybsHVZpv5jaHPvpFSDryZ7N7pemjSvPhhn4QpQzgC648L1eYKvec3yuyxYhDFNre5XFCk14F3HhTrfkW6CnALFkenXtQMejMDVuzKUNxvXD6SV89AUtXabscSoWjUiggrhPQuD61BfWY9fgME7T13ibd4bWasAL9scPR2DTQMjv2gpzBg6pvtVVzfZ6QZWkWRGBzXB874wKPxn6tdP3QbbTj91ozHySSFfCmi9K9pR58Rpaztfdz8N85r3eHTp8NxKwTc2AvwzUtDGoTc68iqENj6MQ5hmFpALBWdXAMApofjtF6b6J9hwGWEiSTJn8XsZEiaSd4sgwk7KTLirWoPz8kmrCMgvTV7BvRLueG5JxM3YvhBkD9JXDKnhRT7yiVGcwWdgZiw3zAq24BM3Rs1gyu41f2DZCizjDFNWeR4DaR89Q6KiW3WGdqB7qPgXE8tRn2E8xF77LoRXcb4UszLnx3dYj44Rz357DGPWcxrk8uhGPX94txCqorocCFAt7MWVdpozYRi4gqVHadXf9SbsayEy7xAJrYWds238kxX11cKdrxr6g8vXCm8Tdq3sBgjfXV",
            "signers": [
                "4EA9y2qbvCn9oGDB6GVmGbU8LGDkoy47643Q3dGbS2jmZWxMGCbVF3BX65oBwi5ReBcurcSVczWGPDWacf6zKfhe"
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
        "mint": "HMN4C1gGfvgLn2Yb564yRKrUJVwprZbyq5CZLBUQL7ud",
        "mintAssociatedTokenAccount": "91m3dtmB9ij1tc4chkvE6GCqLK2wcUZdUqaD9NwbdT3r",
        "transaction": {
            "isPartialSigned": true,
            "serialized": "3upXthSvHwVVCdrM9euWVNhZRZUekYgt1BxoHaEGWRwyZkVk5Ro3BbVNEJjDMMCthVYZc2YCFeScwL3cAHtftWw7ZgRMCM4nHakFXjNN7ziTB8BLx67xBrzXtrfSPou46rDpfbjLdKr3Zo9Udv3HpsTcTU3AkcB6u1XFaTXze3AMAW2jSy5CffeDhxr6cEmvPZvP4tWwznBzsVvsHmpP8NNTdLBr9Eu1LzjpmpLxH5tki5msc6PsX3oJiz8kAGrwhFv7WMheDtjok5oRwdVnmtqbMvBzTJg3mrh8LjpJSS5XoTRPwsArf6xc6xn9nkyCUuDTTjzPigz93iDstWur5H99oW9QjYfSaCPLhhLPaJq8GJCVxDPUZBEZe3g1Ako5wStKWrP2smfzriatNjaaxZyf7QYVviVayuSh5fRVn4gZ7NTTBijWosXnC3AygKXPSqgttgqXHmTfM441C5Bsg53uwpXtcddVdSxUkhpCv5ynphcKhyx6Rx1rnNuUF5ekDFwZpAAp14JJetHD4eMCKrJmgTEQgVJhxSGzxj9TizSsn9pi3x7U8D7gyW21YCTg3M1Ef5VPr3m4UX9g5YC5m7xfkt2fb22qasHdHxWQL13QsRA4Uaz58jMXjvgJJucgiaST5gwSUzgczLpKJfLhcYKJXXCuyPiJQhqR6nfrNpCP3Ebn2GeB4J4PdgM8n7XSD86ec91ecGTheLqYJu5h5JEvdkVqfPKbk2UWUPGmcY8rZJNqoNZ6K2QY9rZcnNE4fW6vSoQ92UaYgs7kCiaLS53xMnmr7v921zAqjitaRKLrh5u5Lnx5FKyk54cCbPac4BX1h41UCCvJgbY2vvRQnseC65bp7eYUn3zMVksG7CgvGPxcvACWwVmB2BzzFv5atcfynQUrEpBteRk97deNMsxFN1yTb3hg9oFqxq76pYQjX1fPTU4mTf14h9s5MYEzwXPQNtSMQ1Sq9m3orgt2i6QGxt4cjMEWFaDYnCrZcvFpHYfpcxv2PgGSxwG3ctgJEsX18fYJnsurE7x5TJRB1mhSKKzxn5pg2VQqB8hFjbgMH2kJ7R2rk73g1m437haHXWpZgnKzZwAdpLyA24Rm8UVTcuXRxb2GaxGizNKU7PbTY1f5TAaAFaoy8kTFZ5RGesTB8P7K8kG9WrgonM6kabhsUFxqvGFYVehvQcy5a7vCWT7QMoyMhyPzYxgc6x3uZTtFSHJ1ox3Tdq",
            "signers": [
                "34rBHbbqJcf2kX9eVFK4jfVqN7qau2uV4tW6a5wH6rcMmQjyyfJt3uLjCwandDtaLpyJMQVtmRE3xprqSByRzDum"
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
            "uriData": {
                "name": "Test: ZlNae",
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
                            "address": "Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V",
                            "share": 50
                        }
                    ]
                }
            }
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
                    mint: 'GftsSpwsLG8VRLX35YfV7EtQS5ZEh6veEZuSMJfS4Txc',
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
            "serialized": "6fPvCMgpMTwLsAX7W2WEz9hwXV32Cnk3LbfBFZLEsDA2jZgC9QxiSHWfoXXMfUYXqZHBGhtu4YvPuhfXVtZoBwKdT37C7f6D9dv4wEmSdRjHiHAt8Cr2sZZAoiGvqJkVzEqSjnNh5qCMEY3qrVT5AypK1My8sN7m8ThMVkE1mK3sGmYzxcDaQLRydsUdKcspEmpUfEYdXHDL2Vd2RqmPLDEnm6zGCbCkvZEZyFwPxCB1z9KL1eS5Xtn1e65zdhiDLnVSYdfxmUordJdzEjnEb32pmfJRuqhQjVGUzk7eDF9Nxoq1KwEv9jxWWpBjyJUbTkyrW6GrtNN2pV93a9pm3HXa2YbE6oy6EpGSospLDtKcynSpBoL28oVr2Q1TArBoSGYYDa76t97gophhRA2j6CtHKxJzmQ7rxG7GRkA6mbKLdSvmJaKYMRhn6LCrkPPEWzEZhNHhHFkne88hoFTWvNHbXFqDVS8CNwyooidcsTWTSsksKgQWGF4rEdAexfp4xjyDyp38hUgm55B3PMpnD7ABepYYF7EQMLJtpc9UazBvPdXzbBUpFq21HhiTSmbRB4J3BZ8BTgK9RYGDnqiF",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_setCreatorVerified

> Set the creator is verified for the NFT

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
                    mint: '6Gttjpk1LxMKwdiLt5q9TSW9hfMuJmdeuVXDrcPyB7YY',
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
            "serialized": "3XkfoeU1FeyQ6spTnBHnipD3eiPgcqvBecXPST439B43gtvxJA6UuTp4YEWtDhSNcgt1b5CkjuWkKn47F5rNcuKT7uZ3cfxmMBtg2P5XfTqftSYub94K5AJQmzLnoNSn2bPYvrgxFFRp9LiJYLDv7qN5ADA19FG1k3PbL1bS1kNoBdZ8nUEE6pU1D5e6gVqcW7mrnc1z86rKjTMXx41SaLsmCVPkjpBLxWTxJiUTadzmzeU6Rh2ZYzHAZy1zuG3XntX9g1iJhLXxDaLYmwjD8f",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_isCreatorVerified

> check the creator is verified or not for the NFT

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
                    mint: '6Gttjpk1LxMKwdiLt5q9TSW9hfMuJmdeuVXDrcPyB7YY',
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

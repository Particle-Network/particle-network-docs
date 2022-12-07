---
description: API for the marketplace.
---

# Metaplex

{% hint style="info" %}
Based on [the metaplex protocol](https://docs.metaplex.com/architecture/overview).
{% endhint %}

### 🔥 NFT\_MP\_initializeMarket

> Initialize a market that can buy and sell NFT from

**Parameters:**

* `<string>` - the account address to become the market manager

**Results:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the market manager.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_initializeMarket',
            params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
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
        "market": "DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy",
        "transaction": {
            "isPartialSigned": false,
            "serialized": "RzU9o8Jk1KtrgR1uh7e4jFRhfYjvK2tyKVyWFFhvCceresDzC8kutbsxGMizCpaB7KUQLKW4nftg7cV3cWbksGmDsvJfuixghdkrfymSmjqHUVsfdqMbX9ri5GBGTGdtGeBJFCtTVQ5dTx1qevinXz2U2yndyqDHpZtRNGMj37h1xNPMbjwEoBMYciFPa7gfvJs3bogEDrJekUGExY6ksefrG71aPhWvaye1wv4K7vL5NrqnYt97RJLyVyb4fR3VAeiRjVLpJYzB3bn5G4rXJgaZQwo4L48cJLCtuYUmMRGFWKf3sWXajvsdPGG4nsVFPpAG6Q45Ujufc9JZuAWdFw4y9BEJeoustWkt1wSsrAbdjrtKWheZMLtyjq3TDycXijgPo1ZM61coxUCvMraFKH5HpA8UnrejJG2e3MCee96TV54uJbKjZY34Fn6X1uPpp9KQqg6bzScpmpKwKwBkbFXTVGybNWBQ1Htf9dCrfTSfn1JRUB1hfLUFaUNKHtjnU8DidyowmiV15Rp2C5rjqQAtjiwApYrWeHtP758PkJsgVYujGtiWR5ovTuEykgFVCa6L8QMhRAPc5zg3c8CfH",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_isMarketInitialized

> Check the market is initialized

**Parameters:**

* `<string>` - the account address to become the market manager.

**Results:**

* `<boolean>` - the market is initialized or not.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_initializeMarket',
            params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
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
        "market": "DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy",
        "transaction": {
            "isPartialSigned": false,
            "serialized": "RzU9o8Jk1KtrgR1uh7e4jFRhfYjvK2tyKVyWFFhvCceresDzC8kutbsxGMizCpaB7KUQLKW4nftg7cV3cWbksGmDsvJfuixghdkrfymSmjqHUVsfdqMbX9ri5GBGTGdtGeBJFCtTVQ5dTx1qevinXz2U2yndyqDHpZtRNGMj37h1xNPMbjwEoBMYciFPa7gfvJs3bogEDrJekUGExY6ksefrG71aPhWvaye1wv4K7vL5NrqnYt97RJLyVyb4fR3VAeiRjVLpJYzB3bn5G4rXJgaZQwo4L48cJLCtuYUmMRGFWKf3sWXajvsdPGG4nsVFPpAG6Q45Ujufc9JZuAWdFw4y9BEJeoustWkt1wSsrAbdjrtKWheZMLtyjq3TDycXijgPo1ZM61coxUCvMraFKH5HpA8UnrejJG2e3MCee96TV54uJbKjZY34Fn6X1uPpp9KQqg6bzScpmpKwKwBkbFXTVGybNWBQ1Htf9dCrfTSfn1JRUB1hfLUFaUNKHtjnU8DidyowmiV15Rp2C5rjqQAtjiwApYrWeHtP758PkJsgVYujGtiWR5ovTuEykgFVCa6L8QMhRAPc5zg3c8CfH",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_setMarketCreator

> Set an account become the creator of the market

{% hint style="info" %}
Only verified creators can **list** NFT in the market.
{% endhint %}

**Parameters:**

* `<object>` - a JSON object containing:
  * `marketManager: <string>` - the address of the market manager.
  * `creator: <string>` - the address of the creator.
  * `activated: <boolean>` - (optional, default true) set the creator is activated or not.

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the market manager.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_initializeMarket',
            params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
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
        "market": "DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy",
        "transaction": {
            "isPartialSigned": false,
            "serialized": "RzU9o8Jk1KtrgR1uh7e4jFRhfYjvK2tyKVyWFFhvCceresDzC8kutbsxGMizCpaB7KUQLKW4nftg7cV3cWbksGmDsvJfuixghdkrfymSmjqHUVsfdqMbX9ri5GBGTGdtGeBJFCtTVQ5dTx1qevinXz2U2yndyqDHpZtRNGMj37h1xNPMbjwEoBMYciFPa7gfvJs3bogEDrJekUGExY6ksefrG71aPhWvaye1wv4K7vL5NrqnYt97RJLyVyb4fR3VAeiRjVLpJYzB3bn5G4rXJgaZQwo4L48cJLCtuYUmMRGFWKf3sWXajvsdPGG4nsVFPpAG6Q45Ujufc9JZuAWdFw4y9BEJeoustWkt1wSsrAbdjrtKWheZMLtyjq3TDycXijgPo1ZM61coxUCvMraFKH5HpA8UnrejJG2e3MCee96TV54uJbKjZY34Fn6X1uPpp9KQqg6bzScpmpKwKwBkbFXTVGybNWBQ1Htf9dCrfTSfn1JRUB1hfLUFaUNKHtjnU8DidyowmiV15Rp2C5rjqQAtjiwApYrWeHtP758PkJsgVYujGtiWR5ovTuEykgFVCa6L8QMhRAPc5zg3c8CfH",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_isMarketCreatorActivated

> Check the creator is activated in the market

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `creator: <string>` - the address of the creator.

**Results:**

* `<boolean>` - the creator is activated in the market or not.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_isMarketCreatorActivated',
            params: [
                {
                    market: 'DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy',
                    creator: 'HXYC1Rwj8RYEis2mLiw2zoTUFv8JRAgz6JSnnme8sNA5',
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

### 🔥 NFT\_MP\_list

> List an NFT to the market

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `seller: <string>` - the address of the seller to list the NFT.
  * `mint: <string>` - the mint address of the NFT.
  * `price: <int>` - the price of NFT, note that this price unit is **lamports**.

**Results:**

* `<object>` - a JSON object containing:
  * `auctionManager: <string>` - the address of the auction manager.
  * `transactions: <[object]>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) array is to be signed with the seller

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');
const { LAMPORTS_PER_SOL } = require('@solana/web3.js');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_list',
            params: [
                {
                    market: 'DvrXAsPw8X8vqFpgfVhWypDeM3sboiBs5iVn3FKNmLWy',
                    seller: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    mint: '29nPkoZPcSCnGhfKKvcPB29joUa7iRfcUSyWk3qoZB37',
                    instantSalePrice: 0.1 * LAMPORTS_PER_SOL,
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
        "auctionManager": "5FCbwcy55sgGSVNZGmP7nemGfMtZ7ZtzgMvQH8h5YKQe",
        "transactions": [
            {
                "isPartialSigned": true,
                "serialized": "4vuRdbkK2txbJVMxYSmksWfiqXNVD5oZ3SvKJszkQ2aSeXcL5Y8c8isyyPdbEwNGDQZCp3F53bPUFurJpg1AzngP8aZFGF5BFDDQTH3MEmVgvXvUCJykuPvGxcwx3e1XLyymhpHZCBE8oZxCRZbiYwR9rLvvWgAjotJd7sv9xRKpRALESayrRtMUyU3RC5yYiy8eHTD18eFvEhNbFWRaS1E8wDQTqPhDPMPrFow8hsQASFbXgLq9si7SEirnMuSLDhABaph5iGWTYVM2K2RKk3TNZPKJA9MgtdRxDa1aD1SLhDjW45UJPubk5ezWSME2AUvy8REizYHAh9P4cPPZqwRJkiZrUY4izDw24Nd7iUzpcxx2H5ztXL4RvHXqECYusfGvAQN4CcZWNJu4sCvtkE5LwctcYMbe3UuLNXonPQkKJYZVSyYEjA1afMkdtTz4g8QKjbkAWqV7puQV1PBKauJAbf6GvVpoEsYLm1Z1V98kJ7guC8YaoPZWWcEznQX22dtgNPiv1dqzjRMhMj89NHts3MGF6vk",
                "signers": [
                    "FkxE3Zk1eJLfbNFLmZ2uXBqeUZRSDC8SQY3ZuWcTqRh9ErgExxhRaZDFQhLvsbfG8P4ZpUc3vErbdXtG56kwpyA"
                ]
            },
            {
                "isPartialSigned": true,
                "serialized": "F3SsHpaGuY6KifCoDhoaf3dUMNGKbdRjBcoNSmRDCQNv7FaPRWMFDCyoFHdRweBXV4DAp8YBh8B7i9JTqvxzxMjnbXdXhe43r8EaBr821jg9gPCESvkditsFm6fA6MXkWNa6S6mVppjbQ7bNXftF7ZBKMPRordmYpLDzGeSZxUNYD51XjpkSHzrLN6t6AmR8PwDz5G45vAAYPCgdGVUbrfdM9r5bncfdvKp7YAaTcz76b2mtJfu85UGkwdApeQBumbF2scsR12NsMrVLENJB6T92MXyCPDrW8UP9SmEhSt67BrrrRnEaW37WRMaYFY2M6fY5DQFuG6Jbc6o7sr13bZxvwhRu3z3JAuuXhmtBwtRowmEHzAPpA8SBjLGH1QGHUKSf7FBPoFFfVhVLy4fYVY3BiV4UAHNMU8fcfjhJi1ev4K4zEKqg1WjAEw2Ba66cMJ4eKiCQaUNjggJWRPqdyEP5ydJ931wTYMfvJkTrpgeA4JBAEYAQ4iXfhEVxzHMvH84kDHPB8sJG1s6ZzgQ9G5hUTZdqLZQCyAxP1wVwcdpU4hvQHKejvt1VQGhycSVbFhUubXMRA3bx1HgdqXnKFxABKyMd4WfBk9UxS8kFwL4gw4SWy2e6PV3hYdkVjGoh7bPCNGsWBxiXtNexC2PsTnyn9N9qpLQ1oHxcEGAhrnTxDiEH6Zw4tsemBMTLNT12NkEDbQQ5RxibEnowxuXVw6GFTgf75JdzfwnAr3a4zgXCwSgm5tnyw2G7FCy8GirF6pd3y2nDYUMkXYqPAPL1iUUqTciCaW6jtTe7PdQMYMrYxAU3nu8q7xX1D3aeTc5YzW5r82iUBT1wqpWdBR9kNrzWVnQAyFmXJYcAUM5qUWULbUvZCTbsccER4zhHG9bdyEFqfZVpHEewjdDhkVub2ptmBGJFxe1ztEv6XPQtLrtVLraXQV4eXzqKgcjZTA8qzr4u59NouoWX7MJeCmqFpqvdjPQZpUxgWXXLVR9gMhBmhM3P7tLvX3bTZYDeNWrjWnEiDauUJntov4Dj3VeyNXAabfS6bPBPASyRitM3pxJNkHr1rnRwP6AnpuhQjg9gfSHQFiRA5b22w4utv7eXJxP5tfs25cUyXfZTSKkgqNSmd6FNhJ1fVJPP6ovSUeTmFeCbKagNxGboSrJ96crCYpvoQvgvTixCF44x2wYPN6iaXtXq66RJLLKWm1fhSNpV72dkxeVUDQxCDneskoXDbgnpQTwGDZkChqjtYJFwjpmxssiZ98aQtH92pSZmddTQi1KAcGyFi7Lm5dmWoFbgxaBtFM5dkwPwJ99zW6zbEg6W6saC2FEU2jawPxr66FbVaYzvSqftproMCycqK1uuoUttrSrQBKvxR9XaTJp74U3WVDVZRRTWBmGaFusU6yRioQMPm5ecfWV6EzyNvv8NL6WduXdS9QQpZLfGxvSkRuGVW5cCbNLMLT",
                "signers": [
                    "GoJ5xVGy98pn6gUBzQJs1HtyxBohwipTjNyJmQnvCLni2mHA7kowaB8HU8KYeGK4r1i1nNWJh6SzboN2ZCWeULr",
                    "2qAU3qa1Qs7ZkdY3WpWYRKhnpuWA1h11LqAUo1WE9rp1QL44UX6FZEudUPsZ8dRzcrzPrLGs7ehRTDFB8RSM8uyy",
                    "2JoQNLsHmeRCFa6T9KcscUsRMPbACDmbu7eA4UmsKp77ynVUd2pAifEJ4VxX2GhCa18Z46MjjUJz1uCFwV7Xutzv",
                    "5LbdQwxvcbmaqGsmGtVTqrGaxS9TMsFpTm6XrmhH4dqXF7wqF9Qqhxo1abr49dvgtXxgo2HLUqeJ5Bgpiv8jmyue"
                ]
            },
            {
                "isPartialSigned": true,
                "serialized": "KKgUM2SeJ6WXC4eYD3hHy23X5bueXZX9GJhawprtj9ZVwQhrv3Q75NEwGDc77zX83g9c3cWJMs91cMWDmxM3YNaSoRvtTZ2cNiKYmrS1GLR5eBMib8dTmVEdQKmhdGtpnobkRHKdBp8pyUAVQNZQnV5M27WvjbmYUrjp4fBfMs6JyLvM6pp1YP89A9N7nTqZoSkRFZFPkMHGXJHVQdqC4Z8NquxXz5dEiJubdFG33rLSYvpvdxhWb2LpXMWtqpwe4HPpkefJdE2CawGrGEu4L9uvn55upxbWaUDGiT8wC5jNaDUX5H7GQ8Cy5i5mRtG6wKZn4tKUGqNHaG5zxbAAFurk2TRGKmHjtWC7bd8L9n35yLwaD7WJy5fmDaANVkxqyotgu6GAoGh8ZMJV5atfpmLwe9quJAVzUM1XvoQbabsYhCLHSyV4oZuLoc8hisr4W2aymSCpUHvo6mftH82cJPnPPwzemNkVm45QyhkVKg1tNaWiTubqBw9upyh4mca4WTYhL93R37tCB91KMUs7YW826B6e3mPsA82K3z5hye4GriXEogBaweEzxLsTvNxirb8H4Aa6mFHLqwdKS7jRQ5QBiuymgeEHPtkkQKJCfGayB9oCvTQaWn169TdZTsUMeaTYsGmAEmB2pHiS476tVn7aXHNRjgPJQe8ye2hP1uq9dsbnVz757AqdJDgmhbEPctbgMLevACWvoYHS9VJ4qyeqrFQT71ESaEX9r2Gw11GWowrshfvimAeHiw5HJa64jYGuzNBMYAXH5juZXMvwsjmRuyCM2jzYEBppE2HirUgA3x2mWCWQTD5mzzLx3nbrrcgDuRvDae55WNwmVFswaLGZ9kpfi2V2UHYvjk4jDv1cFgT3V9riR5WNrdAjgZ2HZocu2QWBmCpqndZPHWzBtAqhQ3gdjRS91cyvKfE5FVZSv72QFyNFjYY5eRqaC9okNMAbDJzj6KbRMrytj",
                "signers": [
                    "3rY7sowGcTvfGqhnmY84FsS7e7kGyTfVdLFAcKtwrBFYR1EibjEjDv2hSz3FKLv7ayAZmkBMSy3Z439DUrcdzt5C",
                    "2uNVZHLXjGkYb8GCAoCJYyoBYGMqx6T2TQmw9uSKoGpx7DNSC14RUWsosv7RJ1Y4GWeqCrvr1gue5WqoLmumGcuq"
                ]
            },
            {
                "isPartialSigned": true,
                "serialized": "mB2JfmyPoX2nwKPJVz1HZVRQmzU11uzhdNAYoCYQGHf2nbkZKcdhH9xTnZbjUZ3y3FJ25uzyYqCkZnQYRXvdqEsGFK2VNn5gsPs5tY4tVfttCEvNpZXePz7xZuw3hWdSwVYV3coNoYRvxkHQqeeLnSZf8K3PDbyHMSzXd6QenNGrcXoiwtrpLe69viR9MybU4As5H9tsBCDJ5XzbnDcvFvfT7d27Q3ZH9LijaY5z2u49eC2C24mTFYD4Jhz5SAGgMW8rMUizjVZcVWfd2PUKwTp4bCY9m5Zh9APykEujMXMRAwgkCRujivAmJXzMffzAhifJT6FPHUxBSBrNBWGxYddJiASiZRvExZchzwVYygUZZHKSsXcvAjLAE45RTZrF3SjdKKJchAdAeMk8dUBpTQfwqRccuJPLCfVuGwffpDmCDb9QvxKvBSjiMmFpQvw8bQE19QR2YSvDiErC5gfftQJzVEPWTsH5BT2XXGrw71PjrGWpDpL8iTUcJjsR4gmXmfWTQ4PwDBJ4HQmxurSVXvjzbFDmFknPA5SnhFViQ7KJMEXtt6459DTxCxR86iztbLBmsxson5FykN4kpopPgJpTHCYsqUKvQVM1sNcgJcAgA7M3q4geCHLJZMTFYqrNUdBdCnkigSZ3tVgDzAEhhRKFGws6vt4heaYLw5cRkqb9VA7DutT9tP58X83mfqv2xo7eHt3M9aNjz7M9XBeWY3wJsaG3toVK3TPx9bU3QwJfeg7KaRVmoDitQJm3Y6RM7HEUiYnfeUP2bxs9tvwGhQwuRFyP497nodjHVsdccNXBkoc1E5JMjR151SeFNA5At5JmNVSCweNaBLKvBLC4r67eqVsrWevMS7sSwGYs9nKN7qMcGidgU2N3zJc2jeWkmtdZJkMLpLnzmpSNdp4gGTJqwtWa8ceczKYufK4YkShuBRQAiSdEuN1hSKZHtGb8VuP9BfQa3XTGqAJAvfzLbmpezWUbNN7v6Ee1MbbS76pQPQD2uhznEQKfrE1j4b9mUCbWriBWm2SP9hrQ8bv7trEy2wxsmpVBNvkNGtdERUws2UF5uEo1z1CYZadGXDxthyxhsZnjJNsTwMgojfNUdAeTfjD3fuL9TkYN4WakNPFecreBHYb94WTuCf5imPJ1Xrcmw34FsxkP6gabkTAXaWYcqfHhw9LMD8RzLUcsTBFR5yPkjVe5p68kU3NYKA3zrdyaFCyQcK2V5kaXYg1PNMBQP74oKbVyDZFSZPCBG6rd7Xqd549TbFA6taabkHLxkqKkm1xyf2LeXdeUkpDBwoBLXAatdohh6kMKu9avW",
                "signers": [
                    "59s6evePkad3BH9J6g4FW5tFqRNKVk4WXByZjAFVMJF7mrPrshCBqCUZT6BxEeFGekHQA5KAYpsweW2tRSPKevBm",
                    "3KxcoLEsx6ZZgLGcNew9uxfy8Ed9fhwtoCJ5znFhjKpQ51jr8cAMLihpuZH7ni8vQYpk649zFRX8rUhwwMdRJLSJ",
                    "3yW7gKGpsS1TufgtZutrrV1BwUN2KG2rJhopuwunCCWAveQqCYa2TBfTL81N1kTUR6jq2YVh1tQrkpd9NqQzK9mD"
                ]
            },
            {
                "isPartialSigned": true,
                "serialized": "2Vr8qpbX8Wafp5sKe2FvcrdkjupUF31wKRLpDoQH1rQHZGu5pdkJd3fGufZSWq3KVMS18iY7yC8EAmYuUsL3Cm6sn5EhMi4JvA4eQifZyhUjkLg6pz6F4z6HkSLwWf94scPEB1RnNNakgwrqM5jE2dofNNTJFkf46BgFGL6gpU6h2j1uQYDhdWvq3QjYUc6Nyb1mUocz9jusmPQTbGDockVy17NeCesjBnTpD3YuHW34RAJb9wXBCmV2jhHPyA7jnup15q8AR6VSsULP8YFyVaSYrKXAkuoe83R3BjnQtpwnNGuqoVc9WCVdEVvPGT21h6K2k4dXVd24k8VCrhBbq21eyzPb8vUH5T7kqxNqvfkLRET7ekcL63aoMqFPtZ1WwiMmMJgGjUZB77ECnHhm9iGmNpyNeyQ7e1c5Z59Z7oHz2r9Gsi5yTzdot6NE89QPbXSVqs4zeMftfCcK99Sj8EXFmhhFMM9Fw9V3T9TQeZsLTLysGDh5aqWy7Qb5T2PX9u7MAyGWZDKAiNo5AUnFiJAerVsmshKgFxvHtUzjFDqDtEjXH4wHzYhS4wf1B32GKSEqCTP6BNaJ2SxwCqAwTmrrez8XdxoLBLDgG5tFKhut21vDnMpchHSPQ5gXUUSXbjsQn2W4zqEw5erMdyUCp1SyoPoLKMuMonj96MpRKZczrbv3t8R8qdw9YM6PsH5waXxKBFDaPgXkPSCCJQYdiYY4dQ6D15pjfMhdAfr8NgfftGu8zKLcExHyViuTMBdUqK3PzwFLVeGirNj1KJkwN2uhB75EpeSDmay767xRT2JWPU2N4CbiQJW8gitZTD2eUHJxqVPQKRaYBsC9fgF5mCRnp5wm4pUTM8hzAwtwuBFW9cEEte5qHqfAmtJ4Qnytk94NkTdw56rUkT62zC1pxRhbfC1czxdum2VyBfCoLHF47K2oBycdp5VFUAoGb5PToRudqoizimN6Ed73XPXzVwLthL5gyYHQGtB9bRqyEtT6HoHzbkAePAHXsH6n5TJVKoqCCLEvdeS89ozUo2eQdxoeFpdjHHDkEQx32GsBiQzWMAsQ9AVfZkSukvs4vBZ7ciFvNac2uoMd1rmAciFDtqfXYdiyYRZkK2LdSugcGDPLcqMQVsCwvxXYGHbhohdkhA1UfLXronKzXkWGBQraoprG2jiRD",
                "signers": [
                    "5znxZhDLsadGwDPR2keY44SFxyaSjkZhP8CxBgndSQVo9yF4iooGkGz4fx7xATasQARRt1YvPz4K1m2ddHEEjHpq"
                ]
            },
            {
                "isPartialSigned": false,
                "serialized": "CnZtHzPTaZUa75Jd96FzQqnyvjWLZVau6ikBB7Koc6ubULQY1htngEFfxBU8xxknyMZPb7HGCHzmPe81Qk6758ks93wsrm3sdsnjpdHacQ7qFrhnxREzm33Zg2QRx2yXXjAiKDTvayTirSYB63G14ViSSW53rPDs5zTYU85qsV1YJXvafSikedHUF2qurJtvFyCqgafv1BnDgDV2eU3ppgctbha7Bq9cS5bpn5VVwsEWXgDJnvukNEWV2D1UF82brEKouPrhtpKNq5MEqBE7PyRWLwhM8DubqQvm81ay7X9u7o3WtcoQCjtLcUTTMeboPzbsdmLbubnKi72evDov1ZT6DcahMniqUkTWWbRvdaoaRVQv3JM9koSPuYguJrEX7Jxm6Kn5xqw93HuXsvjEqKq5tozE2bZSpEUZPauuNxpTXegkiEq36qamjvZnT7CC9UPx67rLLDR2KfWFKJe2LvCQtWZjbqJegsVrXiPBMGvp5YHD3XSdq6G7Toh2FFEWDi7ewSYi4gAz9HQoQGCbivxE4hyuocmBYjbTXBbaHY6bMWKyv7aLoiuM7w9e3dHuViVd45v42nw3UMhht7PegH9e48NUysosiPLQ54dNXguY13zTQT9DMWZrRaw4EQPRcoFdubD4xE77zkZqZyMhUyy74gRtpADzsd4Lgws8kXstN9hcqj1L9vQqCU5KKkJPzfgQLSWZGtj85jv37JS3KDV1miZDLrkjmU3vLsUKYsTGUUFhKhYRXzLdyxwJ7zdppMacyGDTqL1PkCp5yVE2rrY89HJ4GGvyNngP7CmW1kGcwQBL2pt8sHCQvAiUJzAHzB7GLzVyrmyUJK8mAwExgKFHKUZmhvTF9LuadFifpm8ttgEtF4P5snjc64TomcRLufBwrbezQy8pv43GgDFNndtru9vVuvFmvq97eDjbc8DtxmhSTL4GZ815zpymiTy8A5uGHHKtS7S7u2vw4YKQX2VqELRYovKhLPEpwJueyLYofdFjT9kMyX6y5aruXN64H3XiASJdzN4HJzNb8dHm2PoJXyQMeDfCK9sqJ6tvC7QE3RbbdYYBH3VVrGR81g1kHSvARy5qwZPKgywPD4rMsVQRgoXqBLSg78War2TT1uUcC6tAkr1ycbHRLeZyLD1jjGx49oNgjzVXH31nxfMcLjMnRK9veZcgCH4ZvzsZDeyWHD28LsGqr3Ja8",
                "signers": []
            }
        ]
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_unlist

> Unlist an NFT from the market

**Parameters:**

* `<object>` - a JSON object containing:
  * `mint: <string>` - the mint address of the NFT.
  * `auctionManager: <string>` - the address of the auction manager.

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the seller.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_unlist',
            params: [
                {
                    mint: 'ACzVqR5UQ2ZW267fPyDYWVrPBuw3JPpQQF6wFZFWCNPk',
                    auctionManager: 'F3xUt58CSnST6iLEV5HSSZFTuGjuiuzejXTGX7D4Q9Q5',
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
            "serialized": "3kzXxRTzH5SR7Y82QoNf1q4ccLD1Bjkx3BLsvMDhj1zbsr5x9gumbxYcyHPeWeSHQdW4TacHkLBBSFtS8wi2dY6xwJYcYkXk5ejA8PbXZzZ7sJ8wTNowTMEVKrWUSSkd6dRFKeoNLdnix7Voqjh5ARWgcBMrMP73zFpEvVhhz5WHNX8nhJYke3C9r1juELVa2fmc39591J8pJg2x2wxzjVDuewCwAcBehojeRJSxmisnqhTffF5iTADW8VTbiUCSBi8HBDCksH3ua1EcTRGy913tYRSM74AUMns6PAaiMzMAPXwToYVtzdEQiqR58xcDxCWJdjNC5WPTSAhkoRNpnnMmepkV7TRB4yEabmkYKW5pdPN641ZabF4fAgPyqZmrJpDUB3X12BRhB1kTiWLrWkmmYsBiR3bLUCvfzBEBLiFjYXaWkQnFcg2E5LuEEL6NofA7D2nqk6Ageh7vWJbx5tJ58pfzRbJdWQFmkPxWRBRoxNjbXG2eKzFWQJ7qC9kjdE8YJmkeZifnP3CeSX2YNb44JSVVvi2VL6TpQ7gncvpvUsTgKDpAuhRc32Nwu5C3pE7Ny7rS5UwCc2XKADvQqTPG9uvihQMTgwbsBcGYZzEVMLhLiieR5Dewu92sWiBVfy6RdFPmf7mKDrPyXyJeT77KE5ZQCohnBXcfebCCvQ7gsJDVga73qhntnx9rvmueg1fsGoQ7dEiaUFY3uvJu8h8PhcefaCh4wBh2ZGGQ8J5tjTdjdkzhwtm3hXFtXQMczJxnDVDMrgWEXQm82is7PwkdBD39zZsV17592PSEywnWEFeGqLtoDXTasDE3GfJdKe1digoqC6NWdYpBf55coANHrieZWYcjzz4FYRDbkrcQ6Y8zzHyvandMSGki72g9AWDycGShNkPEmjensrtb3YAwrRn5w97jEEQZ9o5EDJJaiRLojPp7rF471ZFZYRHjwKorwwM34K52XdBW3KmvusMoa7hQPdy1sXQm2FaexkKDvDcHod9U1xoh4uz6GGqyfpNdJLJUDJiAoFdEcPFteZRD4Qyqg6uYMpjpiD58aji9xzj4FftSTiCjmynwWXksH7LUzaK2KwfvT9fvztjr3iFSmnQ8hn4KgADJnVMjmapy5ZcyNvG9d3pobiwbPCDm",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_buy

> Buy an NFT from the market

{% hint style="info" %}
When the buyer buys the NFT, the seller needs to settle first to get the money.

See [NFT\_MP\_settle](metaplex.md#nft\_mp\_settle)
{% endhint %}

**Parameters:**

* `<object>` - a JSON object containing:
  * `buyer: <string>` - the address of the buyer.
  * `mint: <string>` - the mint address of the NFT.
  * `auctionManager: <string>` - the address of the auction manager.

**Results:**

* `<object>` - a JSON object containing:
  * `mintAssociatedTokenAccount: <string>` - The address of the account where the buyer places the NFT
  * `transactions: <[object]>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) array is to be signed with the buyer

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_buy',
            params: [
                {
                    buyer: 'Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V',
                    mint: '29nPkoZPcSCnGhfKKvcPB29joUa7iRfcUSyWk3qoZB37',
                    auctionManager: '5FCbwcy55sgGSVNZGmP7nemGfMtZ7ZtzgMvQH8h5YKQe',
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
        "mintAssociatedTokenAccount": "H17mdhJP9XkRh9rULt4x4fXTd71LZnnk7Uui5jjjF1Nf",
        "transactions": [
            {
                "isPartialSigned": true,
                "serialized": "2BjJ642HjF4RpzaB5DqYZg5vVL4cdRjmjuErHfKARPD48SGR9WzxCorRBXmQTq3eWLusmwEuuJKHjXFo7dGMfAJMVu51MnF2Gqp9ZHQJWJFdUV1Ym22txMPJKA8hrb14yHGtXyWPtMHUzqLjNVWvSHE2HjG1DpXtnpVvEs6zyJeBCsEahpbAgDF7n842peqU1nFzgDWohUmo89fYVFvPVd5yWzRaKTTYzmb9APBhs9VeyTvyBSHFdEMdY7j51cu7SQ8f9Up2iuBSMZzmszZWwcMXUdn3Hx5T4F4Ludafra6G7uL1Jof2cKXHokLtRT39WooxK5MJGDZuGWoYAwxHWH3QvgmgrJ7LUbLD6wjkfiW6DCUK127dD6BQmEdJ519ZPDdHLSLLWc7RJsmQmKVngm9nMEvWPGPFC44CL3cyVJEDE9BMxXY3NgkGMAXMJXLHFnaSURhPktaJsPJxc2gY7E3wPFkLg4EfLs8FDchvdYM6zD1nhU7vEmmLVuDkfLgSzJv4RKvpVcy2CvRwKwxcP2zkj9rvM9SSxUcWQSewKxR8RVfTzMMbf9CQDSw6nfsu9gq1tTw9Sw9zyhy61fTW1G3Z3JeWkzptgUk2Ywvj9m6Pgq21TWGh3BoPtSKxQvvzhmy3WN4rUo6Uu3hsQWdYJP5mf28W6nb5dGHdUMEAzfKjYiMjxHZ4dkWej9rSEgSp21RkJ2d3hdvY6Er7AnUEzwgYqszAYEb5GEy4EjgpSesTz9mNE2KyrmggQvvKcBWDk1Pbo6YE3xUE9UasrmikBwjjnrwbfwVcKstkEVGdSxfzQtHUcvQPb2N7ffU4w2pNYyp2HVGEPGrCxLwUc4S5AEK7gWjZkTCzRMptRmGJLHTu5pQtrENHnSm1aJ9Luhvfj53B5HVRkKBXYfR7iiCbNyZLuaEYowFEj6ZqzRGrL57CDdKoEuYJu5umTW1KYTTLGU5A92Pq5kQji66bTaC7E5S74BkBUGvSLZTUoEED6AxChbZgW6LvHv2iUkAr1s7YCwUjuVZUZj8NsDrjReapoFJZFfmHq17xFqjpCVTSLZ2kC6QeHhtwcunQ5ePCN6hoWLCFVqBhZPqARdmF9nPx7Sb3pyiX71rdLy7A64jWmAGueCPvAJUkfXLra27wKu7r2DYqGUCbvW6cwiNQ1ggCj1R6pX82xgioeeZwVhK9esfKXgWCmGRT5ZUcEj9g3mjCoBzomUdJDABxctVbz92nYgbR4z9ycoa5SLmcQREP271",
                "signers": [
                    "5huUaHpEmEK5MHSaTDovpeuyTfiWgwB97MxSA83paruJEYM3SpKWjq2QoxwkqDNpTyxDvxQS2X2zXXx4t3E85bsf",
                    "64XmQwvtNQ9mjLKcgKQEALKZrnP1kVhQrNvipxdB5CBURPcSRNx6bFST3EBJzTvt6CxnSsNKRan1SUYQ3NmRSo7"
                ]
            },
            {
                "isPartialSigned": true,
                "serialized": "6Kk7yb9v4bTprXd4JYJKddFhcXXy3aYfPpY6QktGp6KyyiVtR1FdkU1vDSPfjmGfMXQVnpBBKdcvDNnYDVyMKRxYZa9NrKnNn4susr6zoShDoVJVJ1mQsPzLyxZ4MK2D39PRfCDJXNXhBmo9XaDKyKC4DEB9am3xmbp2RpYR7J3PD7typK35cVE82NwWu2cGoVfFq9Kxkx4NEbJw1rvPXFWHBxXhnHipaJPmbR6D3pq5KxSj9V3b6mQcmT5WGxvidC9bXr525xkHpzNtPs38uT2zVmdkMWXSyb2kMq8ej7WPTboKRmHQaD3k4pFDHagjxwWk5FXbateYtVRCKXbCFZbDdyRXvib35amPqtfyDwN1rKoLVm7uy5FdoufvWapj3imj6WepT6oKqEesGetcZ6yfvpGjMQTYfsRGwzR9yPMKTuv8hZRz5q5GkbKodbNAa2NnfScSHXQYpJ99rmiXwffYh8mFJt4BXK7CpjfknUGoPodaPY2TxJhmdnAeS4YpBCWPfvgiqtsBbRQ81dH3rSMLsQkrdcExyEGCDRBPhQQFfPbHb8bqM1VDKSqCXXmLj2esCAvuEyc6ahHDDPTaWJMUZrT13qwLzUz5WQbLWcaSiztPu4iN5ME85bGr4ZdRcmjsimb3qy6EjchfuYqctnA4nP9v4H5qCoJy8X34wiy9wrZ7Vq3uyTmyzPuMQLpR8rB4pbG1Z7YtMxXxPVvoDponQ4vdefMWyGBsTrk7Bcen8Wj2vo1yKa3N7NZ9UzVu1t8oAKLTDFQviDBtRUBKEAVyRgNkWyB4E7pTa8gmSNta1dRLb97qWGwHwB42jeLf874NEcBQ1ibCXtnj6hKHS1bYWkusJBjCCzUMVxYAaTSNag7H7pDnPa36JZVANPTmFFM3PAFjYJupihg3givoZk7eTMDyGad4cAShfKM1UBjAkgrXNXBFAu8bLCsgvd79qgPA9SYXvx3XRYimKEsG1dEVrVpHiR37pqjDoJD4hvEDcUccCv4EHoEG3n1NA6cKtfWSm7DhvE9c4xuSFaNqSEVSdaxNvBLJqswFsDGf775d2CKguQbM8FCK93H7kAx6MkYauWE5uw9SA12WNUnayGGjRs2wD1Dm3F7XcuPXiEV8Z5GxTBXLPLsuUHsRSHTuC7seSpkTMHjbM5a4WWMpGgPED2Vq8szyXhdAx3VRvdF35KzwqmRHoPhGX3Yi7P6rUK2FYvBSuPd4zAXn867B1VGN32rz9f91sb3gGsdNCdCN8jVd2jciisGEQXE1gVYe4wMS2cp8amtkFc7nM5U6VjSQY61Szr4axJdAefMwDHxBM3ptNEfgXYAKYEUYF3fv5CLd7Ad29DN7pM1gVMqUV7cWgn3WevdQNPjZZHoR",
                "signers": [
                    "5huUaHpEmEK5MHSaTDovpeuyTfiWgwB97MxSA83paruJEYM3SpKWjq2QoxwkqDNpTyxDvxQS2X2zXXx4t3E85bsf",
                    "64XmQwvtNQ9mjLKcgKQEALKZrnP1kVhQrNvipxdB5CBURPcSRNx6bFST3EBJzTvt6CxnSsNKRan1SUYQ3NmRSo7"
                ]
            }
        ]
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_settle

> Settle your NFT auction and transfer sol to your **sol token account**

{% hint style="info" %}
Sol token account is not your native account.

It is a specially generated account for the auction, and all the sol obtained from the auction will be transferred to this account first.

If you want to withdraw from the sol token account to your native account, see [NFT\_MP\_withdraw](metaplex.md#nft\_mp\_withdraw).
{% endhint %}

**Parameters:**

* `<object>`
  * `address: <string>` - the account to settle.
  * `mint: <string>` - the mint address of the NFT.
  * `auctionManager: <string>` - the address of the auction manager.

**Results:**

* `<object>` - a JSON object containing:
  * `nativeMintAssociatedTokenAccount: <string>` - the address of your sol token account.
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the account to settle.

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_settle',
            params: [
                {
                    address: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    mint: '3iVDgUGC26AGLsigTT51pWzVZ3Cyo2wm7Snby3TEFdPx',
                    auctionManager: 'GCGah5TvzSgNDASdeaWTpBMxW8hsJkSp2T4ch6kv7F7k',
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
        "nativeMintAssociatedTokenAccount": "BLXQ15eimFb7mqRmZ7t2PpDmfN8uAK5PZFX3aPnMEp7t",
        "transaction": {
            "isPartialSigned": false,
            "serialized": "3rHPn4RF8fedoJPbkV1MMYdB8zEQRsGcXsHeq2EBxroyymhKKDXqxz7hjvfSfivhue42LgngLTbgRNwMqG2ZtJ7AhSzsWVBuve7f6hj34cxd89LNNuC16poUQNmh1xzdv7SnYhvAh9Te6WszmbkerWocRi84Um8EE73HRc4BSAew7jn9hGiTxE5b9EiGtxPaF2cy1fufurxuNMMpMXTGCSdP46avhc1gDqyk2X9BSqPzk748yrhMdXnjbXpcQ7h8bXPFRU6pKoYyRrbj6yCgk8ugFU4GahPfD3nm88SSDWF5BqgDqzZUoXm3qBN3dFV9ZHMNcHjfK3B9VP41nTKWRTXRaxmHrocwFw3qFo4zMgJWxJuQukLuUgpvCTyrWKbJfiMUxYF9Dm3Lk5YAALp4BDCtcGqVM3VUsvkcNzs7qxtwY8DUAcaYG5MDn3BPpmvCvV9rYZ9h9Dt3fjeG1C9pa37d13uw9pXymbmoXBoRUobbY7QZvtwayZipHcMYcT9XHauoLUrTrZZuYoZawruftLrmM3ShTriucJuCuCrb9WiLfK9L8WMFu1UYU6E95tf1ai64sByqAjyDW4P8RLZ5kUiTy9KUXWtUc5dnoWPWVjCjWqtaLn4kqWAhmD7Dz9tDtVo4cXMDzQJZxeKsBfVmQMSKQwigsdv7dL2JynWUQrbAwiPTPqEzMgJBpvL5VE4VwhrZM4QxFqjQvjVwj8ajj1cKR7A9ZV5Zmuw7fuMMBSgfReZZLfCRK7XMm8BwXJewvAZk9HqL8z61DuAze9oo21EfKeZiX8Gu5xqGmpRafukCtGj8WnmEfjQxdRi79V1dmNAjUgFFMQJwVriAG8dPCkJsVUf8wceZ82m9kb7AhxAhjqgzoj7AWiQ4uHa3xkeFFccCQPuyLsQbiY8SG61iv23NLsKQqeaFDDrsAKJgZd6osbGLhJcQqXCUHKim9b7yZVAyF3utL7s5nVrnkz6nmovCd7vkvuy3eLs2dikAd1h2GKaSnQoHcriitQWZdj4Da44gTnb74djbMP8UkEgDjcSKRThTLrwbdNDKjeLfNdwh31aejWxMBo2zjiMaEniefB7akP4s",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT\_MP\_withdraw

> Withdraw from sol token account to native account

**Parameters:**

* `<string>` - the address of the account to withdraw

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the withdrawn account

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

// chainId: 101 => mainnet; 102 => testnet; 103 => devnet;
const RPC_URL = 'https://rpc.particle.network/solana?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(
        RPC_URL,
        {
            method: 'NFT_MP_withdraw',
            params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ'],
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
            "serialized": "CAjkGtNC7gbvxwJVCQR6oZNAhTh7sCuk11o83KzQT5NSs6D94j9CAZLicreu53UzQX29bPYt4NnJDsriZAQvRTvSHUxSYNoyA1c9xmAJcWp6TrGHEzoLYcP4d1A4qwNWrv8WBAoMFLVRznthGLGw4tyz8yY6dawmgTUgaMdepz7zBcvstrzWGWW5NLzKtwuJx31Vzp9MztL8iNQit4mccPjBBGLq7Lj4wBfza79o5XZRyYAkcLoK4QtLZggMu71yggn8QCfqbq3Ew8ssvtT8ZCt",
            "signers": []
        }
    },
    "chainId": 103
}
```

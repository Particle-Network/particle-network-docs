---
description: API for the marketplace, which is escrowless.
---

# Auction House

{% hint style="info" %}
Based on [the auction house protocol](https://docs.metaplex.com/auction-house/definition). It is simple, fast and very cheap.
{% endhint %}

### Overview

![Architecture](../../.gitbook/assets/API\_Flow.png)

{% hint style="info" %}
If you want to build your market, you need to implement some centralized functions yourself, such as the list of NFTs being sold, the history activities of NFTs, etc.
{% endhint %}

### 🔥 NFT_AH_initializeMarket

> Initialize a market that can buy and sell NFT from

**Parameters:**

* `<object>` - a JSON object containing:
  * `marketManager: <string>` - the account to create and manager the market.
  * `sellerFeeBasisPoints: <int>` - cut of each transaction in the market. The value range is 0 to 10000 (10000 = 100%).
  * `treasuryWithdrawalDestination: <string>` - (optional, default **marketManager**) the account treasury withdrawal to.
  * `feeWithdrawalDestination: <string>` - (optional, default **marketManager**) the account fee account withdrawal to.

**Results:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `makretTreasury: <string>` - the cut from each transaction will be transferred to this account.
  * `marketFeeAccount: <string>` - the fee account of the market. Usually, the network fees for transactions need to be paid by the user himself, and if the market wants to help the user pay these fees, you can make the market manager sign the relevant transaction, and the fee will be deducted from the fee account. Developers need to recharge this account manually.
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
            method: 'NFT_AH_initializeMarket',
            params: [
                {
                    marketManager: '8kM5iZdcYTQAtTRKvGe5oPLxNkowTrLtprbkdoJrcWqx',
                    sellerFeeBasisPoints: 1000,
                    treasuryWithdrawalDestination: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    feeWithdrawalDestination: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
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
        "market": "FscuZGfM7m3Bobee1dtkgXJRUxkKC9PmkdKuNPmkuqw2",
        "marketFeeAccount": "J1eqsRsHfaHDx8o2tqLos26qtS2jrRKJ36XrenmTBay8",
        "makretTreasury": "9Bn3yFtnziBwambR1TZwnqgmaiqHERQWF5SgrSA3dXZo",
        "transaction": {
            "isPartialSigned": false,
            "serialized": "vAsD8RH4xV8cK7RW5XYmE6pjszVd1PMGsUMCNqCsUSBdf5AzZuBU5Uvh86RejoWAPmaAtD5L5abbaxUJ4EUBXsvbVQBAWKHbwj6ekAHzQzJ66XeH8nMc9DSV6eT7QXPHNx1UB5N7rsXLCfK7zgxczM8N54ADQSjUbRwP4HHtdGhBY89uBWycSWRLnCtqY2H6zooAVuBJ5zA9vw19aQQENReYnVMci8WvdFLnz1bmMNP196h7kZRDEazhhnMDxr3VuycbFX6YP4cMfYqVZCeWK5pM5bB5U35xRCAenQVHL6TqoH1SMdese6RCX5gPPgLM4Tn3kvBJGXVVu1wriirK6U1J5HMZaUHnT2GVvVuhBTY5GbtY6YksoQXz6GXQdYYyeVrGqnDyvdigyygjwfjJPk211QTARGtN9yarGGiWLHN8UuwQiJcSmPAXUvoBTKB2v4kWZsAFHSpqDTewgUZPwgBiQnRYGyf55ghqUQKx7wxtzbV5g3scfictyTU1rHu6uUa9V6aceBqAAF1eN7u5R7x5xJSnWg7jAHxg1M1nFChrjEXk882tMcLhpvYhBdJFiAnsYbwVvcJAa3A31WK8mAia7iiCghq38rBAv7ZxMC8MS9YNH5y1pCpDputN79SgVHA9UMxswHD2CLgT6DLf9",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_updateMarket

> Update a market config

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `sellerFeeBasisPoints: <int>` - (optional) cut of each transaction in the market. The value range is 0 to 10000 (10000 = 100%).
  * `treasuryWithdrawalDestination: <string>` - (optional) the account treasury withdrawal to.
  * `feeWithdrawalDestination: <string>` - (optional) the account fee account withdrawal to.

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
            method: 'NFT_AH_updateMarket',
            params: [
                {
                    market: 'FscuZGfM7m3Bobee1dtkgXJRUxkKC9PmkdKuNPmkuqw2',
                    sellerFeeBasisPoints: 2000,
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
            "serialized": "3MWX8BEG3iNbFjBkLBnRtKVn77aCRpKf7pVWoHdsnvitymyU7FhMMof1Xtee2PzY46YDEUCSnRoD2xt47LTcEca915K8N9BN4qyH2qiWBSLUuxFaa4qH5XwMmDeL1U4T7QVaRZ1LGr2XPD8W3BD4o358ihUNG1jHYQxDGc5xCADGx34sAxyU2aJHQPxP8aifBP88hGVTM2w6nTWxFUf7DbZiKAzzYdtn947UP5DE5cn5eDCoUGfbs6RVbXiA1WSkQJh9nunDgedo7dJVHrfi6bktUVqDznJt1HyosAXmqpepDkU82rz76QWACbCXjktRjZHBoEA9DbBXh7siEWGUS5UmjXzw72qQzpziheaeJCR3r9VZV9Patpt48UP3XHigXjJB5iKrUJGtfNiHLJuvBmZFi2dawZEdjDTcZ2zeja2F2exciRbrMmVSfX9FqsxvzTCFeBNHaDGQSJdzL1Wf3vPQVisrKfJpZt368xQmWtzhciQXUXUEVUV3L6UjP2buYphLRJH1fHteZ4cvftGCShHF2KJuWYcMGsrRxHsGbPDXqTzUSTuJRKAHqqaGw",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_isMarketInitialized

> Check market is initialized

**Parameters:**

* `<string>` - the address of the market manager.

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
            method: 'NFT_AH_isMarketInitialized',
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
        "auctionHouse": "F3H6AfbRkYrySoG5PqD8VQbF1p12K7jHwSH7EQ8KZgTn",
        "isInitialized": true
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_marketInfo

> Get market info

**Parameters:**

* `<string>` - the address of the market.

**Results:**

* `<object>` - a JSON object containing:
  * marketFeeAccount
  * marketTreasury
  * treasuryWithdrawalDestination
  * feeWithdrawalDestination
  * treasuryMint
  * authority
  * creator
  * sellerFeeBasisPoints

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
            method: 'NFT_AH_marketInfo',
            params: ['FscuZGfM7m3Bobee1dtkgXJRUxkKC9PmkdKuNPmkuqw2'],
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
        "marketFeeAccount": "J1eqsRsHfaHDx8o2tqLos26qtS2jrRKJ36XrenmTBay8",
        "marketTreasury": "9Bn3yFtnziBwambR1TZwnqgmaiqHERQWF5SgrSA3dXZo",
        "treasuryWithdrawalDestination": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
        "feeWithdrawalDestination": "8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ",
        "treasuryMint": "So11111111111111111111111111111111111111112",
        "authority": "8kM5iZdcYTQAtTRKvGe5oPLxNkowTrLtprbkdoJrcWqx",
        "creator": "8kM5iZdcYTQAtTRKvGe5oPLxNkowTrLtprbkdoJrcWqx",
        "sellerFeeBasisPoints": 2000
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_list

> List an NFT to the market

{% hint style="info" %}
Listing is to approve your NFT to the market, after your NFT is listed, this NFT is still in your account. You can handle this NFT usually.
{% endhint %}

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `seller: <string>` - the seller address to list the NFT.
  * `mint: <string>` - the mint address of the NFT.
  * `price: <int>` - the price of NFT, note that this price unit is **lamports**.

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the seller.

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
            method: 'NFT_AH_list',
            params: [
                {
                    market: 'AoMBLTWSJKiKFG4n7EaWxGqSV3km1a3cCBHtBhycHGwQ',
                    seller: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    mint: 'DeoxVDYFRGfWaps4k91izyK3TM1RxHnHSr28Z6zjS3XU',
                    price: 0.1 * LAMPORTS_PER_SOL,
                    auctionHouse: 'AoMBLTWSJKiKFG4n7EaWxGqSV3km1a3cCBHtBhycHGwQ',
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
            "serialized": "68rmsvTNuhd4GTMnBXrHZLtn9PQgycJ77vrdeUZZiMnxdD6SdCJuFTByCUYz3bPbF4yjvHAis39U1c9snxfsBbVufTsvzHSp3zrWi6zaGZY66s9mx6nnqZsFQiNJspmSegAynterC6pswGkSthu4YFFwW5ykgoXedmAW97TDkMicdENveKcxbmpuoYNS2L26MtJg6NpVrgJvvi9bmXDavYAkP5k3L1KCvygPSGBUyYuXcpsRDsWD45LkYpKENvysSCpCUHgMUoK3S4xSvKtyQFwxhWqakKRXWDZAF8jBLCbjxeR4qzbKuYwq8TKHZ3XvxnhrYqHKe2rxAECWSn7FD8wyy8fgcr14SAen1bBijRuACEg8uNzxB7BCBHGEqDx5sQPG7NoFC4y6unztK5MTYPdi5BcuZcTgtMQBasr4XzEMuynZ3YCVkzPpBnSmX4ytTLjp4exvBA2hfXqTuDrxjFkbV23r5vsgNn656cXyR3vzUNyNgUm2mw1yWSRZVUhzTZrG9qu5Ez5xuDzFRLetFqNRb99K652kShep9mHbTXmtK1i2jvwXVSFYnwEe7CKRPEsSJZzNwvxmH2A6UoGBguYY4ryUHYramXKyWX6g7eJD4bdfp82XRV3aCBpJXsnBJGgcaPmy96M3SrNcdK2VttkKqVBkAbNCJdKHnNzWS5ACxKSu7TjKXqotXmbjZaMctpkDALcWazZxJV9zTa3ogPv1GFc1KN9p9Bmr3SXUaovp4HT95Y7pydkhxTsD",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_unlist

> Unlist an NFT from the market

**Parameters:**

* `<object>` - a JSON object containing:
  * `seller: <string>` - the owner address of the NFT to list.
  * `mint: <string>` - the mint address of the NFT.

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
            method: 'NFT_AH_unlist',
            params: [
                {
                    seller: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    mint: 'DeoxVDYFRGfWaps4k91izyK3TM1RxHnHSr28Z6zjS3XU',
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
            "serialized": "3XkfoeU1FeyQ6spTnBHnipD3eiPgcqvBecXPST439B43gtvxJA6UuTp4YEWtDhSNcgt1b5CkjuWkKn47F5rNcuKT7uZ3cf28HYQNWrRmKU9QSZurSpzAsD6TCL4sGAbnHJ2FKAUGTjKu5P33Ty6eMKgj68AgZVghGSmRdSqsf8nCYUvSNBucNe13tDuvnzP2cUWWfJVQzx1qbsBwq9woD5Mnmricx2hNREZqTgnLgMisjNHuZEpNVNiPXQmwMTkuKTUNGavptZJ66GeF2UZFMv",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_buy

> Buy an NFT from the market

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `seller: <string>` - the seller address that listed the NFT.
  * `buyer: <string>` - the buyer address.
  * `mint: <string>` - the mint address of the NFT.
  * `price: <int>` - the price of NFT, note that this price unit is **lamports**.

**Results:**

* `<object>` - a JSON object containing:
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the buyer.

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
            method: 'NFT_AH_buy',
            params: [
                {
                    market: '47XJUZgUFe5MqmHZVGvu2Dh9yJ1GHd9vQ8dD66S9hnwg',
                    seller: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    buyer: 'Bx5GU8y6oYtTE3ESL469YTjuto8zxPn2xTTvo6BBdt1V',
                    mint: '9N3rM5CfUwZZypqx4hVr19r9zANr1wjzKUCRKXB61rou',
                    price: 0.1 * LAMPORTS_PER_SOL,
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
            "serialized": "VMSDHYE6FBEcjNaSabxCTPQMT8XEnuBPU5nAxr86cA1g85aVdeNcg8c8ptEbro89CffUX5mpYHEHBzejRJDm3Vp6v3ER8KQwLSbEFZvGAcPVeTk6fKP68BanZ6GrpTFctXd1ZmgsKDLrpmAycmmyfSZwUd7ZFc1Hrc31J2hhZ45MrAn1aNotZrF2n3EAw3BgCDUPjRDzLu5KB1pTkt8tvJ5rRCu3j7DJ6DnU56te7roHt6FPQU9iF7xLPX7sN1Hbp8T1YTN9k327jMi8Bg89DxAsLqtNMm782wENELNj7A2YeVQ3bbUUdJd5eKTAGuNT8zautgKFW9xj3sPi7u69atNZ4SCCJDZSqQkhUEboFt8LxuLzjsjJtt8vCFYdWhpeoeFoUgLW6RQqHyoEx3nGBUYYdAWXgDeJ6GMwbsUyTtQQhhziN8KS69w47rKJfZmArw9PE7hFtKLZ9WCMz49nFSJXLxQo68AVjbW4KgZhVfkpsQ5F3dDrgsCcn2YwEQ7Cg9qXB7p64jZHQ9cs1fjThiA4ucsPmcMevpyd61X8MiQJAnWvRwrwJhEVmVnyYBcHtk4we2s8zcpwHbCMP5EwaAAAubwRmN9YV8KmmBbY8pPXjt8ZdbsES362N4zsCuZM4kSKCWcNsEyacSE4fsxcLRpauGTs4AhfUjzjuwDVKDTEGNjEMMW9VqsNLh3UFfYaYWFAC2e9epQ66K7ShNqumKoV7vDxvRTzh7WWu4BHufNeCj88WcQgCyM7Pi94BK6y4oFEketQc6CZDjyxM1p7ZorcFJbjGE75GK14epXdYGY9gTkDT4q2xffxJ6hmH1W7K7eGTij7w269aUzxG97H1w2FUMSXkc3mAXogGrxU1gX8bfS9agVNnEX9ajCF7ceSrqWwV7AbbgjFrddMNSZw3fP48u3DKCjDTGBEafZ3pMsDrHAkgxHURdANFnUbHjDyEWRmDiPjj5tN2ZNTTT5nBLR9cVCCFSvzqTXj69ZD73zWw5WniMabi9doSSLLyGngbpiAW8CmXZw6mvK7cgj46i6QAAdm94UVbdwtP6eovGBAAUzBs8HQzkxGDz6QWNXtHuQMKEcJ9kLe9qJYX55hJn6Qqc47P6FzXp3WZnNBLcVFtbgUDhStoLYJZJLEuMD7u8e9mfokKhiUuXorWnB5es3KAQhJrkiYet3NXfmm9ar3Cy9EBh5Jar13mUjfGkxfuSr7PPVprrAUqSgP",
            "signers": []
        }
    },
    "chainId": 103
}
```

### 🔥 NFT_AH_isSaleValid

> Check the sale in the market is valid

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `seller: <string>` - the seller address that listed the NFT.
  * `mint: <string>` - the mint address of the NFT.
  * `price: <int>` - the price of NFT, note that this price unit is **lamports**.

**Results:**

* `<boolean>` - the sale is valid or not

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
            method: 'NFT_AH_isSaleValid',
            params: [
                {
                    market: 'AoMBLTWSJKiKFG4n7EaWxGqSV3km1a3cCBHtBhycHGwQ',
                    seller: '8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ',
                    mint: 'DeoxVDYFRGfWaps4k91izyK3TM1RxHnHSr28Z6zjS3XU',
                    price: 100000000,
                    auctionHouse: 'AoMBLTWSJKiKFG4n7EaWxGqSV3km1a3cCBHtBhycHGwQ',
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

### 🔥 NFT_AH_marketWithdraw

> Withdraw money from the market treasury account to the market manager account

**Parameters:**

* `<object>` - a JSON object containing:
  * `market: <string>` - the address of the market.
  * `amount: <int>` - the withdraw amount, unit is **lamports**.

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
            method: 'NFT_AH_marketWithdraw',
            params: [
                {
                    market: 'AoMBLTWSJKiKFG4n7EaWxGqSV3km1a3cCBHtBhycHGwQ',
                    amount: 20000000,
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
            "serialized": "55QBZB7NF5gQy7LAFEDpJWQNuQjstocXramGMU5kG6fkYR3NXF6mAzqjH7a8X71hEHj5RjqL8MhquMs6usRp1GvHiJPcTLhBNni3XVrxWDrNye4nu2Yhz5BiBiKVyc4nHb285QJfRPR9USs9TTR5Z8ZKTWQL8giT2svcMZD5vDpqsZ7WGGq9mdxsEsGT7K84ENfKr2ksEA7mCoSNz1WPgAkdUGzsUy9dJM4NojcxdmGwnsKW7zxxXhCTvQdkCbRsEUSB6U4nCzhRq7u6FbrT1KqBHB26SM3JKWtR5uRoRV6FEzFmj2xHadwWrPz3a8KEXuMr9gEQH7NzzwUEHH2b6up4zw1VAqqsx91ZhQoK4hXgjQnm8ASF4bcBECF2jrvJWTYqWv7RWY2qAFHu7g6tXeMp5wvmkE1tTXBmmjiWM7o2bS1HQEHvk2vuAL6UsR86AyuwqEgb6ff6hN41CRBJBYFo3aaG91c3",
            "signers": []
        }
    },
    "chainId": 103
}
```



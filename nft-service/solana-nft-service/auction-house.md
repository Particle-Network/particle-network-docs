---
description: API for marketplace which is escrowless.
---

# Auction House

{% hint style="info" %}
Based on [the auction house protocol](https://docs.metaplex.com/auction-house/definition). It is simple, fast and very cheap.
{% endhint %}

### Overview

![Architecture](../../.gitbook/assets/API\_Flow.png)

### 🔥 NFT\_AH\_initializeMarket

> Initialize a market that can bought and sold NFT from

**Parameters:**

* `<string>` - the public key of the account to become the market manager
* `<object>` - a JSON object containing:
  * sellerFeeBasisPoints`: <int>` - cut of each transaction in the market. The value range is 0 to 10000 (10000 = 100%).

**Results:**

* `<object>` - a JSON object containing:
  * `auctionHouse: <string>` - the address of the Market
  * `auctionHouseFeeAccount: <string>`
  * `auctionHouseTreasury: <string>` - the cut from each transaction will be transferred to this account
  * `transaction: <object>` - [the transaction struct](../../node-service/solana-api/transaction-struct.md) is to be signed with the market manager

Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const DEV_RPC_URL = 'https://api.particle.network/solana/rpc?chainId=103';
const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const response = await axios.post(DEV_RPC_URL, {
        method: 'NFT_AH_initializeMarket',
        params: ['8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ', {
            sellerFeeBasisPoints: 1000,
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
        "auctionHouse": "22xrnJiMuq8ESmwx8VofK8AtUYZfG28tHMBx58oanW5Q",
        "auctionHouseFeeAccount": "EGD1kd4EHu1EF2xniKAAsM2gzkA4j6W8swCmVi3Vyh1X",
        "auctionHouseTreasury": "Ek2Jhmk1EQch6LYZ9R9sNY2H887KJwBRUK7RvhK6iB54",
        "transaction": {
            "isPartialSigned": false,
            "serialized": "45zDbqFbkcGewf1pGYqG4qjecPk8GQ24ip8Lqqs4oSHcK68KAAjhrSpdV2EKp6nrBn3DzDPKV4K42Nq9hcWSL7FK4XSRJSbFa9hvRc7SmFEMNW2oF2wLbTpwMhnxb3kJeJ6A87P8FsR9bFcX1EavpbA2ffX6TkvxY1QBzk3YERSsP3oCCT7ihxbzGr1JARJuNUM9nGxP1bU93EAV7Zt5u193bga1gTq5wx722zwprbVkZxCLAmdm3DeQJtpo2eped1SrCw6tjW8aUBcVPMMQTkmgv1auPN2fPVzZa1gKBnfAiGrkMZsnjwEuoD8wP8tsC8zQfW3nyR55MgVARLpS4phMV4Q3RffGKVcEoX9qxysvhHf74YnfUrZwMTLrRRwNcdQNBeK8Se6Yj8fx4KDs4jzoBLYjoTG1tBQLsstVN2GBTZvc2V3sZaSyNUkj7H9ecDR6AncRu2W3YUSDEubdcExHTWYV9LQWQbJpwDCwwzdPQjXKBxEqEfxjui84nzGTUnNqEN22KYRfdotdGQbxE6MhqnRzQSoPbfq6MaqcELP6p6gXgZsCWKLNLAkyd1NhD8ZT4rq7fUzZajNgYeUKxbrZqdGALrupZkPMn8sNxw",
            "signers": []
        }
    },
    "chainId": 103
}
```

###

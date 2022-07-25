# Server API

The Server API provides useful APIs to support application use. It follows a JSON-RPC 2.0 standard.

## Endpoint

The Server RPC URL is:

{% hint style="info" %}
**https://api.particle.network/server/rpc**
{% endhint %}

## Authentication

[👉 Check Authentication](../../node-service/authentication.md#authentication)

## Methods

### :fire: getUserInfo

> Obtain user info by uuid and token

Use this API to integrate Particle Auth into your user ID system :tada:

**Parameters:**

* `<string>` - user uuid
* `<string>` - user token

**Results:**

* `<object>` - a JSON object containing:
  * `uuid: <string>`, user uuid
  * `phone: <string>`, user phone number
  * `email: <string>`, user email
  * `name: <string>`, user name
  * `avatar: <string>`, user avatar url
  * `facebookId: <string>`, exist when user login with facebook
  * `facebookEmail: <string>`
  * `googleId: <string>`, exist when user login with google
  * `googleEmail: <string>`
  * `appleId: <string>`,  exist when user login with apple
  * `appleEmail: <string>`
  * `wallets: <[object]>`, user wallets with public address

#### Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require("axios");

(async () => {
  const response = await axios.post(
    "https://api.particle.network/server/rpc",
    {
      jsonrpc: "2.0",
      id: 0,
      method: "getUserInfo",
      params: ["Particle Auth User Uuid", "Particle Auth User Token"],
    },
    {
      auth: {
        username: "Your Project Id",
        password: "Your Project Server Key",
      },
    }
  );

  console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network/server/rpc' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"jsonrpc":"2.0","id":0,"method":"getUserInfo","params":["Particle Auth User Uuid", "Particle Auth User Token"]}
'
```
{% endtab %}
{% endtabs %}

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

Response example:

```typescript
{
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": {
        "uuid": "2d7b1ff2-0791-4fd2-a26e-16fbcaefdf8a", 
        "phone": null, 
        "email": "U1gphy1mnU@particle.network",
        "name": null,
        "avatar": null,
        "facebookId": null,
        "facebookEmail": null,
        "googleId": null,
        "googleEmail": null,
        "appleId": null,
        "appleEmail": null, 
        "createdAt": "2022-06-08T07:47:54.000Z", 
        "updatedAt": "2022-06-08T07:47:55.000Z", 
        "wallets": [
            {
                "chain": "evm_chain", 
                "publicAddress": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF"
            }, 
            {
                "chain": "solana", 
                "publicAddress": "3k61BRDo253SF1XuknbBSUh8FvFqJkcMYzmZuvMMhCC1"
            }
        ]
    }
}
```

### :fire: lockUserWallet

> Lock user wallet transactions for a contract/program up for a while

**Parameters:**

* `<string>` - user uuid
* `<string>` - user token
* `<object>` - a JSON object containing:
  * `chain: <string>`, wallets chain name, the type value is as below
    * `solana`
    * `evm_chain`
  * `chainId: <uint64>`, chain id, see [#chainid-configs](../../available-networks.md#chainid-configs "mention")
  * `publicAddress: <string>`, user wallet public address
  * `contractAddress: <string>`, your contract/program address
  * `time: <uint64>`, lock time(seconds), and max lock time is 86400(24hours)

{% hint style="info" %}
Only verified contract can be locked for user wallet, please add and verify your contract/program address first [👉 Dashboard](https://dashboard.particle.network/), and testnet will not check the contract is verified.
{% endhint %}

**Results:**

empty string

#### Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require("axios");

(async () => {
  const response = await axios.post(
    "https://api.particle.network/server/rpc",
    {
      jsonrpc: "2.0",
      id: 0,
      method: "lockUserWallet",
      params: ["Particle Auth User Uuid", "Particle Auth User Token", {
        "chain": "evm_chain",
        "chainId": 42,
        "publicAddress": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF",
        "contractAddress": "0x27833fBD4FA313A11653D1D2372cB89f5f2D2f62",
        "time": 3600
    }],
    },
    {
      auth: {
        username: "Your Project Id",
        password: "Your Project Server Key",
      },
    }
  );

  console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network/server/rpc' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"jsonrpc":"2.0","id":0,"method":"getUserInfo","params":["Particle Auth User Uuid", "Particle Auth User Token", {"chain": "evm_chain", "chainId": 42, "publicAddress": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF", "contractAddress": "0x27833fBD4FA313A11653D1D2372cB89f5f2D2f62", "time": 3600}]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```typescript
{
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": ""
}
```

### :fire: unlockUserWallet

> Unlock user wallet transactions for a contract/program

**Parameters:**

* `<string>` - user uuid
* `<string>` - user token
* `<object>` - a JSON object containing:
  * `chain: <string>`, wallets chain name, the type value is as below
    * `solana`
    * `evm_chain`
  * `chainId: <uint64>`, chain id, see [#structure](../../node-service/solana-api/#structure "mention") for solana, [#structure](../../node-service/evm-chains-api/#structure "mention") for evm-chains
  * `publicAddress: <string>`, user wallet public address
  * `contractAddress: <string>`, your contract/program address

{% hint style="info" %}
Only verified contract can be locked for user wallet, please add and verify your contract/program address first [👉 Dashboard](https://dashboard.particle.network/), and testnet will not check the contract is verified.
{% endhint %}

**Results:**

empty string

#### Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require("axios");

(async () => {
  const response = await axios.post(
    "https://api.particle.network/server/rpc",
    {
      jsonrpc: "2.0",
      id: 0,
      method: "unlockUserWallet",
      params: ["Particle Auth User Uuid", "Particle Auth User Token", {
        "chain": "evm_chain",
        "chainId": 42,
        "publicAddress": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF",
        "contractAddress": "0x27833fBD4FA313A11653D1D2372cB89f5f2D2f62"
    }],
    },
    {
      auth: {
        username: "Your Project Id",
        password: "Your Project Server Key",
      },
    }
  );

  console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network/server/rpc' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"jsonrpc":"2.0","id":0,"method":"getUserInfo","params":["Particle Auth User Uuid", "Particle Auth User Token", {"chain": "evm_chain", "chainId": 42, "publicAddress": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF", "contractAddress": "0x27833fBD4FA313A11653D1D2372cB89f5f2D2f62"}]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```typescript
{
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": ""
}
```

## Errors

[👉](https://particle.network/#/login) [Check RPC errors](../../node-service/error-reference.md)

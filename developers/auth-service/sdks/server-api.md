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
  * `twitterId: <string>`,  exist when user login with twitter
  * `twitterEmail: <string>`
  * `telegramId: <string>`,  exist when user login with telegram
  * `telegramPhone: <string>`
  * `discordId: <string>`,  exist when user login with discord
  * `discordEmail: <string>`
  * `githubId: <string>`,  exist when user login with github
  * `githubEmail: <string>`
  * `twitchId: <string>`,  exist when user login with twitch
  * `twitchEmail: <string>`
  * `miscrosoftId: <string>`,  exist when user login with miscrosoft
  * `miscrosoftEmail: <string>`
  * `linkedinId: <string>`,  exist when user login with linkedin
  * `linkedinEmail: <string>`
  * `jwtId: <string>`, exist when user login with JWT(Custom Auth)
    * Format: `projectId + : + jwt unique id`
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
        "twitterId": null,
        "twitterEmail": null,
        "telegramId": null,
        "telegramPhone": null,
        "discordId": null,
        "discordEmail": null,
        "githubId": null,
        "githubEmail": null,
        "twitchId": null,
        "twitchEmail": null,
        "microsoftId": null,
        "microsoftEmail": null,
        "linkedinId": null,
        "linkedinEmail": null,
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

### :fire: isProjectUser

> Check if the user's address has interacted with your project

**Parameters:**

* `<string>` - chain, wallets chain name, the type value is as below
  * `solana`
  * `evm_chain`
* `<string>` - user address

{% hint style="info" %}
Only verified contract can be locked for user wallet, please add and verify your contract/program address first [👉 Dashboard](https://dashboard.particle.network/), and testnet will not check the contract is verified.
{% endhint %}

**Results:**

`<bool>`&#x20;

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
      method: "isProjectUser",
      params: ["evm_chain", "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF"],
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
    {"jsonrpc":"2.0","id":0,"method":"isProjectUser","params":["evm_chain","0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF"]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```typescript
{
    "jsonrpc": "2.0", 
    "id": 0, 
    "result": true
}
```

## Errors

[👉](https://particle.network/#/login) [Check RPC errors](../../node-service/error-reference.md)

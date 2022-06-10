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

### getUserInfo

> Obtain user info by uuid and token

Use this API to integrate Particle Auth into your user ID system :tada:

**Parameters:**

* `<[string]>` - user uuid
* `<[string]>` - user token

**Results:**

* `<[object]>` - a JSON object array containing:
  * `uuid: <string>`, user uuid
  * `phone: <string>`, user phone number
  * `email: <string>`, user email
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

## Errors

[👉](https://particle.network/#/login) [Check RPC errors](../../node-service/error-reference.md)

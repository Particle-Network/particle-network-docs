# Server API

The Server API provides useful APIs to support application use. It follows a JSON-RPC 2.0 standard.

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

[👉 Sign up/log in and create your project now](https://particle.network/#/login)

Response example:

```typescript
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "uuid": "7e3b4c8d-7c90-4111-917c-ebd08a959972",
    "phone": null,
    "email": "BN0yBF7sVq@particle.network",
    "created_at": "2022-05-11T12:28:26.000Z",
    "updated_at": "2022-05-11T12:28:29.000Z"
  }
}
```

### Errors

[👉](https://particle.network/#/login) [Check RPC errors](../../node-service/error-reference.md)

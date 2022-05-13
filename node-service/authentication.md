---
description: Authenticate yourself to the Particle Node Service API.
---

# Authentication

## Endpoint

**https://api.particle.network**

All Web3.0 APIs have a common **URI prefix**: `/{chainName}/rpc`

{% hint style="info" %}
For example, the Solana RPC URL is:

https://api.particle.network/solana/rpc
{% endhint %}

## Authentication

{% hint style="info" %}
We assume you already have a [Particle Account](https://particle.network/#/login) and access to our [Dashboard](https://particle.network/#/login) where you can create projects and apps.
{% endhint %}

The Web3.0 APIs require **HTTP Basic Authentication**

| Basic Auth Key | Basic Auth Value        |   |
| -------------- | ----------------------- | - |
| Username       | Your Project Id         |   |
| Password       | Your Project Server Key |   |

### Code Example

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.get('https://api.particle.network', {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        },
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://api.particle.network' \
--header 'Authorization: Basic { Auth String }'
```
{% endtab %}
{% endtabs %}

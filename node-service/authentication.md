---
description: Authenticate yourself to the Particle Node Service API
---

# Authentication

## Endpoint

**Base URL: https://api.particle.network**

The Server RPC URL is:

{% hint style="info" %}
**https://api.particle.network/server/rpc**
{% endhint %}

The Solana chain's RPC URL is:

{% hint style="info" %}
**https://api.particle.network/solana/rpc**
{% endhint %}

All EVM-compatible chains' RPC URL is:

{% hint style="info" %}
**https://api.particle.network/evm-chain/rpc**
{% endhint %}

## Authentication

{% hint style="info" %}
We assume you already have a [Particle Account](https://dashboard.particle.network/#/login) and access to our [Dashboard](https://dashboard.particle.network/#/login), where you can create projects and apps.
{% endhint %}

The Web3 APIs require **HTTP Basic Authentication**:

| Basic Auth Key | Basic Auth Value        |
| -------------- | ----------------------- |
| Username       | Your Project Id         |
| Password       | Your Project Server Key |

## Code Example

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.get('https://api.particle.network/server/rpc', {
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
curl 'https://api.particle.network/server/rpc' \
--header 'Authorization: Basic { Auth String }'
```
{% endtab %}
{% endtabs %}

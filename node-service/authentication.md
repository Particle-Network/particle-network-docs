---
description: The authentication process for the Particle Node Service API.
---

# Authentication

## Endpoint

{% hint style="success" %}
https://api.particle.network
{% endhint %}

All Web3.0 APIs have a common **URI prefix**: `/{chainName}/rpc`

For example, the Solana RPC URL is:

{% hint style="success" %}
https://api.particle.network/solana/rpc
{% endhint %}

## Authentication

Server APIs require **HTTP Basic Authentication**.

Before you access the API, you need an account from

{% hint style="info" %}
**Username: sss**

**Password: bbb**

**If you do not ha**
{% endhint %}

A valid Particle Network `Project Id` and `Project Server Key` are required in the `Authorization` header of every request. Use `Project Id` as your username and `Project Server Key` as your password.

```typescript
const axios = require('axios');

await axios.get('/server/getUserInfo', {
    auth: {
        username: 'Your Project Id',
        password: 'Your Project Server Key',
    },
    params: {
        useruuid: 'Particle Auth User Uuid',
        usertoken: 'Particle Auth User Token',
    },
});
```

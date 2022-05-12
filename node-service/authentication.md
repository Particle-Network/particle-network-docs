---
description: Authenticate yourself to the Particle Node Service API.
---

# Authentication

## Endpoint

{% hint style="success" %}
`https://api.particle.network`
{% endhint %}

All Web3 APIs have a common URI prefix: `/{chainName}/rpc/`

For example, the solana rpc url is&#x20;

{% hint style="info" %}
```
https://api.particle.network/solana/rpc/
```
{% endhint %}

## Authentication

The Server APIs require `HTTP Basic Authentication`

A valid Particle Network `Project Id` and `Project Server Key` is required in the `Authorization` header of every request. Use `Project Id` as the username and `Project Server Key` as the password.

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

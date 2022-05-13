---
description: Authenticate yourself to the Particle Node Service API.
---

# Authentication

## Endpoint

**https://api.particle.network**

{% hint style="info" %}
For example, the Solana RPC URL is:

https://api.particle.network/solana/rpc
{% endhint %}

## Authentication

Server APIs require **HTTP Basic Authentication**.

{% hint style="info" %}
We assume you already have a [Particle Account](https://particle.network/#/login) and access to our [Console](https://particle.network/#/login).
{% endhint %}

The Node APIs require **HTTP Basic Authentication**

Before you access the apis, You need to get a account from&#x20;

| Basic Auth Key | Basic Auth Value                      |   |
| -------------- | ------------------------------------- | - |
| Username       | Your Project Id                       |   |
| Password       | Your Project Client Key Or Server Key |   |

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

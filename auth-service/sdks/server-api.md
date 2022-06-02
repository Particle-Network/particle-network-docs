# Server API

## Endpoint

`https://api.particle.network`

All APIs have a common URI prefix: `/server/`

## Authentication

The server APIs require `HTTP Basic Authentication`

A valid Particle Network `Project Id` and `Project Server Key` are required in the `Authorization` header of every request. Use `Project Id` as the username and `Project Server Key` as the password.

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

[👉 Sign up/log in and create your project now](https://particle.network/#/login)

## Errors

[ Check RPC errors](../../node-service/error-reference.md)

## APIs

{% swagger method="get" path="getUserInfo" baseUrl="https://api.particle.network/server/" summary="" %}
{% swagger-description %}
Get User Info
{% endswagger-description %}

{% swagger-parameter in="query" name="useruuid" required="true" %}
Particle Auth User Uuid
{% endswagger-parameter %}

{% swagger-parameter in="query" name="usertoken" required="true" %}
Particle Auth User Token
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
Basic Auth using Project Id and Project Server Key
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "uuid": "7e3b4c8d-7c90-4111-917c-ebd08a959972",
  "phone": null,
  "email": "BN0yBF7sVq@particle.network",
  "created_at": "2022-05-11T12:28:26.000Z",
  "updated_at": "2022-05-11T12:28:29.000Z"
}
```
{% endswagger-response %}
{% endswagger %}

Use this API to integrate Particle Auth into your user ID system :tada:

---
description: Using your own Custom Authentication
---

# Custom

## Introduction

You can use your existing user base or authentication method with Particle Auth. This way, your users can still log in with their current accounts.

## JWT Config

1. Example: [https://dev-qr6-59ee.us.auth0.com/.well-known/jwks.json](https://dev-qr6-59ee.us.auth0.com/.well-known/jwks.json)
2. We only support RS256 now.
3. Libraries:
   1. Java: [https://github.com/auth0/java-jwt](https://github.com/auth0/java-jwt)
   2. Nodejs: [https://github.com/auth0/node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)
   3. PHP: [https://github.com/lcobucci/jwt](https://github.com/lcobucci/jwt)
   4. Go: [https://github.com/auth0/go-jwt-middleware](https://github.com/auth0/go-jwt-middleware)
4. JWKS Creator
   1. [https://russelldavies.github.io/jwk-creator/](https://russelldavies.github.io/jwk-creator/)\
      ![](<../../.gitbook/assets/image (1) (2).png>)

## Dashboard

Set your custom JWT configuration on the dashboard

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## Auth Core

We also provide native libs for Custom Auth, in this way, you can have full control over UI/UX if you don't want to relay on the system browser.

It's optional but provides underlying control.

Pls check >> [Auth Core](core.md)

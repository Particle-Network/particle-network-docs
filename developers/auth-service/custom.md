---
description: Using your own Custom Authentication
---

# Custom

## Introduction

You can use your existing userbase or authentication method with Particle Auth. This way, your users can still log in with their existing accounts. If you need to use this function, please [contact us](https://calendly.com/particle-network).

## JWT Config

1. Example: [https://dev-qr6-59ee.us.auth0.com/.well-known/jwks.json](https://dev-qr6-59ee.us.auth0.com/.well-known/jwks.json)
2. We only support RS256 by now.
3. Libraries:
   1. Java: [https://github.com/auth0/java-jwt](https://github.com/auth0/java-jwt)
   2. Nodejs: [https://github.com/auth0/node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)
   3. PHP: [https://github.com/lcobucci/jwt](https://github.com/lcobucci/jwt)
   4. Go: [https://github.com/auth0/go-jwt-middleware](https://github.com/auth0/go-jwt-middleware)

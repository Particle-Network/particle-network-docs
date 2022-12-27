---
description: Using your own Custom Authentication
---

# Custom

## Introduction

You can use your existing userbase or authentication method with Particle Auth. This way, your users can still log in with their existing accounts. If you need to use this function, please [contact us](https://calendly.com/particle-network).

## JWT Config

Most of the login providers that support JWT-based login will provide you this URL, such as Firebase, Google, GitHub, Auth0 etc.

Example: [https://dev-qr6-59ee.us.auth0.com/.well-known/jwks.json](https://dev-qr6-59ee.us.auth0.com/.well-known/jwks.json)

We only support RS256 by now.

But if you're using `jose` or `jsonwebtoken` package to generate a JWT token for login.

Libraries:

1. Java: [https://github.com/auth0/java-jwt](https://github.com/auth0/java-jwt)
2. Nodejs: [https://github.com/auth0/node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)
3. PHP: [https://github.com/lcobucci/jwt](https://github.com/lcobucci/jwt)
4. Go: [https://github.com/auth0/go-jwt-middleware](https://github.com/auth0/go-jwt-middleware)

Then here's the step to do so:

1. Create a Private Key using openssl. `openssl genrsa -out privateKey.pem 512`: This privateKey will be used to sign the token.
2. Using the above `privateKey.pem` file, create a Public Key. `openssl rsa -in privateKey.pem -pubout -out publicKey.pem`: This `publicKey.pem` will be converted to JWKS.
3. Now, look for a tool that converts `.pem` to `jwk(s)` format.
   * One of the tools is [https://russelldavies.github.io/jwk-creator/](https://russelldavies.github.io/jwk-creator/)
     * Select Public Key Use: `Signing`
     * Algorithm: `RS256`
     * Key ID: `alpha-numeric-random-string`
     * PEM encoded key: `{paste-the-publicKey-pem-file-content-here`\
       ``![](../.gitbook/assets/image.png)``
   * Click on `Convert` button.
4. Now, save the output in a `.json` file, and it. This is your JWKS Endpoint, which you need to send to Particle Team to config.

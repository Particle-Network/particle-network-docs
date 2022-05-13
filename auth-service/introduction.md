---
description: Provides MPC (Multi-Party Computation)-based threshold signatures.
---

# Introduction

## PWhat is Particle Auth？

Particle Auth is a pluggable auth infrastructure for Web3.0 wallets and applications. Particle Auth supports mobile number and email login, providing users with a simple operating experience. Their login account is then associated with a private key, which is secured with a "Threshold Signatures vault".

![Particle Auth](../.gitbook/assets/未标题-1副本.jpg)

## What does Particle Auth do? <a href="#what-does-web3auth-do" id="what-does-web3auth-do"></a>

By integrating Particle Auth into your project, your users can experience the following services:

* **Email/Mobile number logins and passwordless flows**: **** users can register with a mobile phone number or email address, then log in using a verification code. This shortens the login process while maintaining a secure login. In the future, mainstream social media logins will also be made available.
* **Provide MPC (Multi-Party Computation) based Threshold Signatures:** With Threshold Signatures, each of the parties creates a key independently. Then they forge the vault’s lock together, in a modular way, in which each party shapes a part of the lock that corresponds to its key.

## What Are Particle Auth's features?

* **Speedy logins**: the log-in process is no different from that of a Web2.0 login, which greatly lowers the barriers to entry and improves user experience.
* **Non-custodial private key infrastructure**: the user is always in control of ownership and access to their cryptographic key pair. Login services only ever have access to one share, and it is therefore impossible for the provider to retrieve the user's private key on their own.
* **Whitelabel:** customize Particle Auth's UI so that it blends directly into your application.
* **Signature**: a full signature service.

![Approve & Signature](../.gitbook/assets/sign.jpg)

## How Do I Integrate Particle Auth? <a href="#how-can-i-use-web3auth" id="how-can-i-use-web3auth"></a>

Particle Auth comes with simple SDKs that can be integrated in multiple ways to provide the best experience for your users. We support all the available chains out there, with special providers available for EVM and Solana.

### 👉  [Auth SDKs](sdks/)

* 👉  [Server API](sdks/server-api.md)
* 👉  [Android](sdks/android.md)
* 👉  [iOS](sdks/ios.md)
* 👉  Unity: coming soon
* 👉  Web: coming soon

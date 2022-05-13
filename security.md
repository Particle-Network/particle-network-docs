---
description: Secure, Easy-to-Use, and Trusted Web3 Product Solutions
cover: .gitbook/assets/飞书20220513-140855.png
coverY: 0
---

# Security

## Our Key Management Philosophy

Private keys are the most valuable possessions in the blockchain space. If the user loses his private keys or they are compromised, he will never be able to get his currencies back. The most important part of designing and implementing Particle Network is the secure management of private keys.

To provide our extremely secure services, there are three fundamental points we insist**:**

**No one except the user himself has full control**

Only the user himself can start the **** signing process, other parties including Particle Network and developers can never access users' funds.

**Account needs to be restored safely**

Users may delete the app or lose their phone, we need to let them restore their account safely.

**No single point of failure**

When using the traditional private key, if something happens to the private key, users lose their crypto and can never get them back. We’ve replaced the traditional private key with two independently created mathematical secret shares and eliminated single point of failure.

## Next-Gen Key Management Architecture

**Particle Network** leverages secure multiparty computation (MPC) for the protection and management of cryptographic keys and secrets, and provides industry-grade MPC-based solutions for key management and key protection.

**Particle Network** provides the security benefits of no single point of compromise and shared responsibility (like Multi-Sig), but with a single signature and without any dependency on the ledger. More specifically, it provides three critical security properties:

_**Sensitive keys and secrets are split into two random shares**_

The two shares are stored on separate, segregated parties. Each of these shares by itself reveals nothing whatsoever about the key material.

_**All cryptographic operations performed throughout the key lifecycle are performed without ever combining these 2 shares together**._

This includes signing, derivation and even generation. There is no complete key material or secret in the memory, ever. It is proven mathematically that obtaining the key material requires access to both key shares, and therefore requires compromising both parties. A single party, even if completely compromised and controlled by an attacker, reveals nothing about the key material -- simply because the key material **never** resides in any single party.

_**Key shares refresh**_

The key shares are continually modified without modifying the key itself. It is computationally efficient and can be performed very frequently -- thus forcing the attacker to compromise both parties at virtually the same time in order to obtain key material.

![Next-Gen Key Management System](.gitbook/assets/os-key-shares.png)

**Particle Network** includes a secure MPC implementation of the following algorithms:

1. 2-Party ECDSA - Solana
2. 2-Party EdDSA - Ethereum, Binance Smart Chain, Polygon, Avalanche and more

As a result, Particle Network provides a **secure, easy-to-use and trusted** key management system which is **chain agnostic and robust**.

## Threshold Signatures Explained

## Our Infrastructure Architecture



## Further Reading

* [Fast Secure Two-Party ECDSA Signing](https://eprint.iacr.org/2017/552)
* [Fast Secure Multiparty ECDSA with Practical Distributed Key Generation and Applications to Cryptocurrency Custody](https://eprint.iacr.org/2018/987.pdf)
* [Two-Party ECDSA from Hash Proof Systems and Efficient Instantiations](https://eprint.iacr.org/2019/503.pdf)
* [Fast Multiparty Threshold ECDSA with Fast Trustless Setup](https://eprint.iacr.org/2019/114.pdf)
* [Secure Two-party Threshold ECDSA from ECDSA Assumptions](https://eprint.iacr.org/2018/499.pdf)

\

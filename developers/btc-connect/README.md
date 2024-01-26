---
description: The First Account Abstraction Protocol on Bitcoin
---

# 🚀 ₿ BTC Connect

## Introduction

The introduction of Bitcoin inscriptions and Ordinals has sparked an unprecedented level of innovation and acceleration within the #1 blockchain in crypto. From new L2s to fully-fledged Metaverses and DeFi-compatible tooling, the Bitcoin ecosystem has recovered its vitality, attracting new waves of builders, thinkers, and tinkerers. At Particle Network, we’ve made it our mission to create tools that serve the needs of developers and Web3 end-users alike. Our Modular Smart Wallet-as-a-Service stack has already made waves within the space, with 800+ integrations, over 15 million wallets created, and over 3M ERC-4337 User Operations across different chains. In light of the latest Bitcoin advancements, and leveraging the EVM compatibility of new BTC L2s, BTC Connect will spread the adoption of the account abstraction framework to Bitcoin for the first time. Thanks to this, developers will have an easier time building mass-market-ready applications that rely on the unique security and intrinsic value of the Bitcoin network.

## Product

BTC Connect takes advantage of the ERC-4337 design and L2 EVM-compatible chains to introduce a Smart Account, Paymaster, Bundler, and a unique Bitcoin-specific Modal. With it, developers in the fast-paced world of Ordinals and inscriptions will be able to offer their users access to gasless trades, account programmability, etc. Furthermore, all existing BTC wallets will have the ability to be used to control Smart Accounts on Layer-2s, opening a wide array of possibilities for dApps while further interconnecting BTC to the smart contract ecosystem. In other words, we are abstracting away the complexities associated with accounts and transactions in hopes to keep driving value and new users to BTC applications.

## Features

1. A modal that aggregates mainstream BTC wallets (Unisat, Xverse, OKX Wallet, etc.), simplifying the connection and interaction with the BTC network.
2. Utilize a BTC wallet to sign and control the BTC Smart Account on BTC EVM L2s; users can interact with BTC EVM L2s without the need to download EVM wallets (MetaMask, Trust, etc.).
3. On BTC EVM L2s, developers can perform batch transactions, leverage Paymaster for gasless transactions, and access other functionalities.
4. Fully-featured BTC Smart Wallet embedded within the dApp.

## Supported Networks

{% content-ref url="available-networks.md" %}
[available-networks.md](available-networks.md)
{% endcontent-ref %}

## Integration

Check demo: [https://btc-connect-demo.particle.network/](https://btc-connect-demo.particle.network/)

Check repo: [https://github.com/Particle-Network/particle-btc-connect](https://github.com/Particle-Network/particle-btc-connect)

{% content-ref url="sdks/web.md" %}
[web.md](sdks/web.md)
{% endcontent-ref %}

## Deeper Tech Intro

1. BTC and Ethereum both use ECDSA(secp256k1) for signing;
2. BTC wallets follow a unified standard for signing messages, based on the implementation of bitcore-lib, supporting the signing of strings.
   1. [https://github.com/bitpay/bitcore/blob/926c1f78e458fca436acec6cac67659e52403880/packages/bitcore-lib/lib/message.js#L35](https://github.com/bitpay/bitcore/blob/926c1f78e458fca436acec6cac67659e52403880/packages/bitcore-lib/lib/message.js#L35)
   2. [https://docs.unisat.io/dev/unisat-developer-service/unisat-wallet#signmessage](https://docs.unisat.io/dev/unisat-developer-service/unisat-wallet#signmessage)
3. Once the BTC wallet is successfully connected, you can obtain the public key.
4. We calculate an EVM address based on the BTC wallet's public key. This address will serve as the Owner of the BTC Smart Account, providing valid signatures.
5. The mapping relationship of addresses is as follows:
   1. BTC PublicKey -> **BTC Address** & EVM EOA Address
   2. EVM EOA Address -> **BTC Smart Account Address** (This address is calculated in SDK)

```typescript
function btcComputeEvmAddress(privateKey: string): string {
    const bitcoinPrivateKey = new bitcore.PrivateKey(privateKey);
    const bitcoinPublicKey = bitcoinPrivateKey.toPublicKey().toString();
    const bitcoinAddress = bitcoinPrivateKey.toAddress().toString();
    const evmAddress = computeAddress('0x' + bitcoinPublicKey).toString();

    console.log('Bitcoin Public Key: ', bitcoinPublicKey);
    console.log('Bitcoin Address: ', bitcoinAddress);
    console.log('EVM Address: ', evmAddress);

    return evmAddress;
}
```

6. **BTC Address** and **BTC Smart Account Address** are the addresses to be used.
7. Request a signature from BTC Wallet and convert the signature to EVM compatible signature:

```typescript
function btcSignatureToEvmSignature(bitcoinSignature: string) {
    const _bitcoinSignature: any = (bitcore.crypto.Signature as any).fromCompact(Buffer.from(bitcoinSignature, 'base64'));
    const evmSignature = joinSignature(
        splitSignature({
            recoveryParam: _bitcoinSignature.i,
            r: hexZeroPad('0x' + _bitcoinSignature.r.toString(16), 32),
            s: hexZeroPad('0x' + _bitcoinSignature.s.toString(16), 32),
        }),
    );

    return evmSignature;
}
```

8. Construct an EVM user operation and send it to BTC EVM L2s.
9. The BTC Smart Account will verify the BTC signature.

---
description: Particle provides a complete wallet service.
---

# Web

## Usage

### Open Wallet Use Particle Default Config

👉 [https://wallet.particle.network/](https://wallet.particle.network/)

#### Open Wallet in iframe

Particle Wallet support scan address QRCode and WalletConnect QRCode, so you need add camera permission to iframe.

```typescript
// get iframe element and add 'camera' permission
const iframe = document.getElementById('particle-wallet-iframe');
iframe?.allow = 'camera';
```

### Open Wallet By Particle Auth

When your DApp integration [Particle Auth](../../auth-service/sdks/web.md#open-particle-web-wallet), you can call `openWallet()` open wallet.

## Custom Particle Wallet Style

<figure><img src="https://static.particle.network/docs-images/wallet-custom-style.png" alt=""><figcaption><p>Wallet custom style</p></figcaption></figure>

You can preview wallet style in [Particle Wallet](https://wallet.particle.network/?customStyleSetting=true), and edit styles through the bottom right artboard. Copy style JSON set to [Auth Service](../../auth-service/) or [Connect Service](../../connect-service/).

<table><thead><tr><th width="310.3333333333333">name</th><th>description</th></tr></thead><tbody><tr><td>supportChains</td><td>The chains wallet support switch</td></tr><tr><td>displayTokenAddresses</td><td>The tokens wallet display in list</td></tr><tr><td>displayNFTContractAddresses</td><td>The NFT contracts display in list</td></tr><tr><td>priorityTokenAddresses</td><td>Token list priority</td></tr><tr><td>priorityNFTContractAddresses</td><td>NFT list priority</td></tr><tr><td>fiatCoin</td><td>The Currency Unit</td></tr><tr><td>evmSupportWalletConnect</td><td>The Wallet support WalletConnect protocol or not</td></tr><tr><td>supportUIModeSwitch</td><td>The Wallet support UI Mode switch or not</td></tr><tr><td>supportLanguageSwitch</td><td>The Wallet support Language switch or not</td></tr><tr><td>light</td><td>light mode style</td></tr><tr><td>dark</td><td>dark mode style</td></tr><tr><td></td><td></td></tr></tbody></table>












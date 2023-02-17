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

When your DApp integration [Particle Auth](../../auth-service/sdks/web.md#open-particle-web-wallet), you can call `openWalllet()` open wallet.

### Custom Particle Wallet Style

<figure><img src="https://static.particle.network/docs-images/wallet-custom-style.png" alt=""><figcaption><p>Wallet custom style</p></figcaption></figure>

You can preview wallet style in [Particle Wallet](https://wallet.particle.network/?customStyleSetting=true), and edit styles through the bottom right artboard. Copy style JSON set to [Auth Service](broken-reference) or [Connect Service](broken-reference).

| name                         | description                                      |
| ---------------------------- | ------------------------------------------------ |
| supportChains                | The chains wallet support switch                 |
| displayTokenAddresses        | The tokens wallet display in list                |
| displayNFTContractAddresses  | The NFT contracts display in list                |
| priorityTokenAddresses       | Token list priority                              |
| priorityNFTContractAddresses | NFT list priority                                |
| fiatCoin                     | The Currency Unit                                |
| evmSupportWalletConnect      | The Wallet support WalletConnect protocol or not |
| supportUIModeSwitch          | The Wallet support UI Mode switch or not         |
| supportLanguageSwitch        | The Wallet support Language switch or not        |
| light                        | light mode style                                 |
| dark                         | dark mode style                                  |
|                              |                                                  |












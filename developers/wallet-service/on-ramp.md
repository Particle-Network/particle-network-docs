---
description: D2C fiat-crypto exchanges on your platform
---

# On-Ramp

Particle has integrated multiple On-Ramp vendors, supporting dozens of fiat and various chains and cryptocurrencies.&#x20;

This lets our partners purchase cryptocurrency directly through various local and global fiat payment methods. This may be particularly useful for novice users who do not yet have any cryptocurrency holdings.

By allowing users to purchase cryptocurrency directly with fiat currency, exchanges, DeFi protocols, wallets, and Web3 services can more easily onboard new users into the crypto market and begin investing in digital assets.

### 👉[ ](../../overview/available-networks/)[Supported fiat currencies](https://ramp.particle.network/supported\_fiat.html)

### 👉[ ](../../overview/available-networks/)[Supported cryptocurrencies](https://ramp.particle.network/supported\_cryptocurrencies.html)

### Live

👉 [https://ramp.particle.network/](https://ramp.particle.network/)

👉 [https://wallet.particle.network/](https://wallet.particle.network/)\
\
👉 You can also enter the On-Ramp page by clicking the Buy button in the wallet.

![](<../../.gitbook/assets/image (2) (1).png>)

## SDK Integration

You can customize the open buy page by setting `options` parameters.

|      name     |                                         description                                         |  type  | required |
| :-----------: | :-----------------------------------------------------------------------------------------: | :----: | :------: |
|    network    | \[Solana, Ethereum, Binance Smart Chain,  Polygon, Tron, Optimism, Arbitrum One, Avalanche] | string |   False  |
|    fiatCoin   |                                        Fiat currency                                        | string |   False  |
|   cryptoCoin  |                                       crypto currency                                       | string |   False  |
|    fiatAmt    |      The amount of fiat currency that the user wants to spend.It's just for Buy Crypto      | number |   False  |
|    cryptAmt   |     The amount of crypto currency that the user wants to sell.It's just for Sell Crypto     | number |   False  |
|  fixFiatCoin  |                           Prevent user from changing fiat currency                          |  bool  |   False  |
| fixCryptoCoin |                        Prevent user from changing fiat cryptocurrency                       |  bool  |   False  |
|   fixFiatAmt  |                      Prevent user from changing amount of fiat currency                     |  bool  |   False  |
| walletAddress |                       Wallet address for the predefined cryptocurrency                      | string |   False  |
|     theme     |                                          light,dark                                         | string |   False  |
|    language   |                                     en,zh-CN,zh-TW,ja,ko                                    | string |   False  |

Integration can be achieved by using the API provided by different SDKs, or by splicing parameters according to your needs to obtain the corresponding link.

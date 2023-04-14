---
description: Changelog
---

# Changelog

## 20230407

1. Auth & Wallet Service
   1. Add new EVM chains
      1. Ethereum Sepolia
      2. Conflux espace
      3. MAPO (Map Protocol)
      4. Base
      5. Polygon zkEVM
   2. Add logic for processing pending transactions before sending a new tx
   3. Add support for binding/linking other accounts for login
   4. On-Ramp supports Avalanche
   5. Add support for Wallet Connect V2
   6. Optimize the UI for EIP712, Seaport protocol
   7. Add tag for `to` address
   8. Add support for redirect mode
   9. Add a new method to check user login status: `isLoginAsync`
   10. Add support for configuring Auth and Wallet UI Mode separately
2. Connect Service
   1. Add Wallet, Buy, and Copy wallet connect code to Connect UI Kit\
      ![](<../../.gitbook/assets/image (2).png>)<img src="../../.gitbook/assets/image (1) (5).png" alt="" data-size="original">
3. NFT Service
   1. Add support for Ethereum Sepolia and Conflux eSpace

## 20230310

1. Add Master Password
2. [Add new on-ramp service](../../developers/wallet-service/on-ramp.md)
3. Add twitter login
4. Add scroll support
5. Improve Custom UI
6. Bug fix

## 20230211

1. Add new EVM chains: Arbitrum Nova, Celo, Gnosis, Klaytn, Scroll, zkSync2.0, Metis
2. Enhanced UI customization: Login, Verification & Transaction
3. Web Wallet: WalletConnect supports paste code & shows scan at iframe
4. Transaction Smart Deserialization supports Pancacke swap
5. Optimize Tokens & NFTs api speed

## 20230112

1. Add new EVM chains: Oasis Emerald
2. Auth SDK supports silent logout/quick logout
3. Auth SDK: optimize login & logout logic
4. Approve Page: Parse Seaport, Uniswap, Element contracts
5. Web Wallet: bug fix & login session sync to auth
6. Optimize UI & UX

## 20230106

1. We add Payment Password for Particle Auth MPC wallet
2. Particle Auth SDK supports quick/silent logout feature
3. UI Optimization

## 20221227

1. Add new EVM chains: ThunderCore & Cronos
2. Add EVM Dapp Browser for Android & iOS
3. UI Optimization

## 20221223

1. Wallet supports Custom UI now
2. Web wallet supports Tron
3. Wallet New function: Hide Token

## 20221214

1. Add new EVM chain: OKC
2. Web Connect SDK supports Phantom Browser
3. Web Wallet iframe supports a close button
4. Auth Web page will hide phone/email by options

## 20221207

1. Localization support added
2. New Chain support: Tron
3. Optimize approve page for ERC 20, 721 & 1155
4. Add project statistic

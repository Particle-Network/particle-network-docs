---
description: Changelog
---

# Changelog

## 20230621

Version: `0.14.1`

1. New Chains
   1. Taiko
   2. OKBC
2. Auth Service
3. Wallet Service
   1. Add WalletConenctV2 for Particle Auth Wallet
   2. Wallet UI supports passing parameters to set AA 4337 mode.
   3. Account Abstraction Update
      1. Optimize the Paymaster logic

## 20230616

Version: `0.14.0`

1. New Chains
   1. opBNB
2. Auth Service
   1. Support for address labels
   2. Add transaction risk detection
3. Wallet Service
   1. Add Fusion mode for Swap powered by 1inch
   2. Enhanced display of transaction records information
   3. Add wallet to wallet chat powered by sending network
   4. Account Abstraction Update
      1. Batch send tokens
      2. Batch send nfts

## 20230606

1. Auth Service
   1. Add `personal_sign_uniq` & `eth_signTypedData_v4_uniq`
2. Connect Service
   1. Add Coinbase Wallet, Trust Wallet & OKX Wallet

## 20230517

1. New Chains
   1. Linea
   2. Mantle
   3. Combo
   4. zkMeta

## 20230509

1. Auth Service
   1. Add CAPTCHA for sending verification code

## 20230508

1. [Account Abstraction Support](../../developers/account-abstraction/)
2. Auth & Wallet Service
   1. Security Center adds a new feature: managing logged-in dapps
   2. Support for users to bind additional login accounts
   3. Support for users to close payment password
   4. Optimize the UX for EIP4361 - Login with Ethereum

## 20230410

1. SDKs support Cocos Creator

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

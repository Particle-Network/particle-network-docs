---
description: Changelog
---

# Changelog

## 20231023

1. New Chains
   1. Scroll Mainnet
   2. Manta Mainnet
   3. Taiko Jolnir Testnet
   4. ZetaChain Testnet
   5. OpSide Alpha Testnet
   6. EOS EVM Mainnet & Testnet
   7. Kava Mainnet & Testnet
2. Account Abstraction
   1. Add built-in support for Biconomy Account, Cyber Account, and Simple Account
   2. Bundler supports more chains🔥 [Check👀](../../developers/node-service/evm-chains-api/bundler-rpc.md)
   3. Releasing our Modular Smart Wallet-as-a-Service

## 20230925

1. We've released a brand new UI that supports simpler and more flexible configurations
2. Fixes an issue where Chrome may not persist a user's wallet login state
3. Wallet SDK
   1. Add the ability to hide scam NFTs

## 20230907

1. New Chains
   1. opBNB Mainnet
   2. Scroll Sepolia Testnet
2. Wallet SDK
   1. Web
      1. Add Bridge (powered by [Li.FI](https://li.fi/))
      2. Add DID support
      3. Add Swap support for Base and Polygon zkEVM

## 20230818

1. Auth SDK
   1. Calling the Login method supports automatic signing for messages that comply with the SWIE standard.
2. Connect SDK
   1. Web SDK Add Rabby Wallet&#x20;
3. New Chains
   1. GasZero

## 20230817

1. Account Abstraction
   1. Add support for Base Mainnet & Goerli, Optimism Mainnet & Goerli, Avalanche Mainnet & Goerli
   2. Refactor underlying RPC
   3. Add more ERC20 Tokens as Gas Fee Options
2. Wallet
   1. Automatically Hide Images of SCAM NFTs
   2. Add support to set your custom wallet icon & name showed in the wallet ui
   3. SDK embeds the native wallet within the Activity/Controller
3. Others
   1. Fix AA on Arbtrum error: out of gas
   2. Fix Node RPC error: filter id not found
   3. Fix Get Proxy Token contract info error

## 20230727

🎉 New milestone achieved! Version 1.0.0 has been released! We have further improved the SDK's composability, introducing the Core SDK, granting developers greater control over UI and UX.

Additionally, we have standardized APIs and data structures, reducing the integration complexity across different platforms.

Furthermore, we are fully prepared for L2 Summer and have extended support to all L2 chains.

Below are our specific changes:

1. New Chains
   1. Zora
   2. PGN
   3. SKALE Nebula
   4. Manta Testnet
   5. Base Mainnet
   6. ReadON Testnet
2. [Core SDK 👀](../../developers/auth-service/core/)
3. Wallet
   1. Add a control option to hide suspicious tokens and enhance security

## 20230712

1. New Chains
   1. Linea Mainnet
   2. Mantle Mainnet
2. Auth
   1. Add force setting configuration for Master password and Payment password
   2. Remove Form mode
3. Wallet
   1. On-Ramp supports zkSync Era and Celo
   2. Swap adds Li.fi and adds support for chains: OKTC, Moonbeam, Moonriver, Celo, Cronos, zkSync Era, Mantle
4. Node
   1. Add Bridge RPC
   2. Add DID RPC

## 20230705

1. Wallet
   1. Add Transak provider for our on-ramp aggregator
   2. Add zkSync Era support for on-ramp
2. Dashboard
   1. Add Team feature
   2. Add support for Custom Auth configuration

## 20230703

1. Connect
   1. Web SDK add new wallets: bitkeep, math wallet, safepal, token pocket & onto

## 20230629

1. Auth
   1. Optimize load speed
   2. Add `authorization` for login method: login and get a signature
2. Connect
   1. Add WalletConnectV2 support

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
      ![](<../../.gitbook/assets/image (2) (1).png>)<img src="../../.gitbook/assets/image (1) (5).png" alt="" data-size="original">
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

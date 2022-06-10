# iOS

## Add Wallet Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 12

### Create a Particle Project and App

Before you can add the Wallet Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://particle.network/#/login)

### Add the Wallet Service SDK to your app <a href="#add-sdks" id="add-sdks"></a>

Wallet Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Wallet Service's CocoaPods distribution requires Xcode 13.3.1 and CocoaPods 1.10.0 or higher. Here's how to install the Wallet Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```ruby
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app.

```ruby
// Particle Wallet API
pod 'ParticleWalletAPI'
//  Particle Wallet GUI, you can remove it if custom GUI.
pod 'ParticleWalletGUI'
```

3\. Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

```ruby
pod install --repo-update
```

```ruby
open our-project.xcworkspace
```

4\. Add Privacy - Camera Usage Description in your info.plist file, we need to take photos for the QR code.

The Wallet Service can only be used after a successful log-in with Auth Service.

{% hint style="info" %}
Wallet GUI depends on Auth Service, you must import [Auth Service](broken-reference).
{% endhint %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

## Wallet API Reference

### Solana Service

#### Get the real-time exchange rate of the Solana token

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: enhancedGetPrice
// native token address is "native"
let addresses: [String] = ["native"]
ParticleWalletAPI.getSolanaService().enhancedGetPrice(by: addresses, currencies: ["usd"]).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Get the token list and NFT list by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: enhancedGetTokensAndNFTs
// address is user public address
let address: String = ""
ParticleWalletAPI.getSolanaService().enhancedGetTokensAndNFTs(by: address).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Get the parsed transaction history by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: enhancedGetTransactionsByAddress
// address is user public address
let address: String = ""
ParticleWalletAPI.getSolanaService().enhancedGetTransactions(by: address, beforeSignature: nil, untilSignature: nil, limit: 1000).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Serialize unsigned transaction

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: enhancedSerializeTransaction
let transactionType: SolanaTransactionType = .transferSol
let sender: String = ""
let receiver: String = ""
let lamports: BInt = BInt(0)
let mintAddress: String? = nil
let payer: String? = nil
ParticleWalletAPI.getSolanaService().enhancedSerializeTransaction(type: transactionType, sender: sender, receiver: receiver, lamports: lamports, mintAddress: mintAddress, payer: payer).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Get the token info list&#x20;

{% tabs %}
{% tab title="Swift" %}
```swift
// get solana chain all SPL token info
ParticleWalletAPI.getSolanaService().getTokenList().subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Access any standard RPC

{% tabs %}
{% tab title="Swift" %}
```swift
// such as getBalance
let method: String = "getBalance"
let params: [Encodable?] = ["8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ"]
ParticleWalletAPI.getSolanaService().rpc(method: method, params: params).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

### EVM Service

#### Get the real-time exchange rate of the EVM token

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: particle_getPrice
// address is user public address
let addresses = ["native"]
ParticleWalletAPI.getEvmService().particleGetPrice(by: addresses, vsCurrencies: ["usd"]).subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Get the token list and NFT list by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: particle_getTokensAndNFTs
// address is user public address
let address = ""
ParticleWalletAPI.getEvmService().particleGetTokensAndNFTs(by: address).subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Get the parsed transaction history by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// call enhanced method: particle_getTransactionsByAddress
// address is user public address
let address = ""
ParticleWalletAPI.getEvmService().particleGetTransactions(by: address).subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Get the token info list&#x20;

{% tabs %}
{% tab title="Swift" %}
```swift
// get any EVM chain all token info
ParticleWalletAPI.getEvmService().getTokenList().subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Access any standard RPC

{% tabs %}
{% tab title="Swift" %}
```swift
// such as eth_getBalance
let method = "eth_getBalance"
let params: [Encodable?] = ["0xfe3b557e8fb62b89f4916b721be55ceb828dbd73", "latest"]
ParticleWalletAPI.getEvmService().rpc(method: method, params: params).subscribe { [weak self] _ in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

#### Serialization the data of contract

{% tabs %}
{% tab title="Swift" %}
```kotlin
// call 'transfer(to, amount)' function in erc20
func erc20Transfer() {
    let contractAddress = ""
    let to = ""
    let amount = BInt(1)
    ParticleWalletAPI.getEvmService().erc20Transfer(contractAddress: contractAddress, to: to, amount: amount).subscribe { [weak self] _ in
        guard let self = self else { return }
        // handle result
    }.disposed(by: bag)
}
// call 'approve(spender, amount)' function in erc20 
func erc20Approve() {
    let contractAddress = ""
    let spender = ""
    let amount = BInt(1)
    ParticleWalletAPI.getEvmService().erc20Approve(contractAddress: contractAddress, spender: spender, amount: amount).subscribe { [weak self] _ in
        guard let self = self else { return }
        // handle result
    }.disposed(by: bag)
}
// call 'transferFrom(from, to, amount)' function in erc20
func erc20TransferFrom() {
    let contractAddress = ""
    let from = ""
    let to = ""
    let amount = BInt(1)
    ParticleWalletAPI.getEvmService().erc20TransferFrom(contractAddress: contractAddress, from: from, to: to, amount: amount).subscribe { [weak self] _ in
        guard let self = self else { return }
        // handle result
    }.disposed(by: bag)
}
// call 'safeTransferFrom(from, to, tokenId)' function in erc721
func erc721SafeTransferFrom() {
    let contractAddress = ""
    let from = ""
    let to = ""
    let tokenId = ""
    ParticleWalletAPI.getEvmService().erc721SafeTransferFrom(contractAddress: contractAddress, from: from, to: to, tokenId: tokenId).subscribe { [weak self] result in
        guard let self = self else { return }
        // handle result
    }.disposed(by: bag)
}
// call 'safeTransferFrom(from, to, id, amount, data)' function in erc1155
func erc1155SafeTransferFrom() {
    let contractAddress = ""
    let from = ""
    let to = ""
    let id = ""
    let amount = BInt(0)
    let data: [UInt8] = []
    ParticleWalletAPI.getEvmService().erc1155SafeTransferFrom(contractAddress: contractAddress, from: from, to: to, id: id, amount: amount, data: data).subscribe { [weak self] result in
        guard let self = self else { return }
        // handle result
    }.disposed(by: bag)
}
```
{% endtab %}
{% endtabs %}

## Wallet GUI Reference

### Open Wallet

{% tabs %}
{% tab title="Swift" %}
```swift
PNRouter.navigator(routhPath: .wallet)
```
{% endtab %}
{% endtabs %}

### Open Send Token

{% tabs %}
{% tab title="Swift" %}
```swift
//open send spl token
let tokenSendConfig = TokenSendConfig(tokenAddress: nil, toAddress: nil, amount: nil)
PNRouter.navigatorTokenSend(tokenSendConfig: tokenSendConfig)

//open send default token by chain name
PNRouter.navigator(routhPath: .tokenSend)
```
{% endtab %}
{% endtabs %}

`TokenSendConfig` has three parameters:

1. `tokenAddress`: token mint address(optional)
2. `toAddress`: receiver address (optional)
3. `amount`: send token amount, token minimum unit (optional)

### Open Receive Token

Display your address QR code.

{% tabs %}
{% tab title="Swift" %}
```swift
PNRouter.navigator(routhPath: .tokenReceive)
```
{% endtab %}
{% endtabs %}

### Open Transaction Records

{% tabs %}
{% tab title="Swift" %}
```swift
// open spl token transaction records
let tokenTransactionRecordsConfig = TokenTransactionRecordsConfig(tokenAddress: tokenAddress)
PNRouter.navigatorTokenTransactionRecords(tokenTransactionRecordsConfig: tokenTransactionRecordsConfig)

//open default token transaction records by chain name
PNRouter.navigator(routhPath: .tokenTransactionRecords)
```
{% endtab %}
{% endtabs %}

### Open NFT Details

{% tabs %}
{% tab title="Swift" %}
```swift
// open nft details page
// in solana, tokenId should be an empty string, address is the nft mint address.
// in evm, address is the nft contract address.
let address = ""
let tokenId = ""
let config = NFTDetailsConfig(address: address, tokenId: tokenId)
PNRouter.navigatorNFTDetails(nftDetailsConfig: config)
```
{% endtab %}
{% endtabs %}

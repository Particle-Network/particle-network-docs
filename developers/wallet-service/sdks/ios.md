# iOS

## Add Wallet Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 14.1 or later
  * CocoaPods 1.11.0 or higher
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

### Create a Particle Project and App

Before you can add the Wallet Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](../../../getting-started/dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Wallet Service SDK to your app <a href="#add-sdks" id="add-sdks"></a>

Wallet Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Wallet Service's CocoaPods distribution requires Xcode 14.1 and CocoaPods 1.11.0 or higher. Here's how to install the Wallet Service using CocoaPods:

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
Wallet GUI depends on Connect Service, you must import [Connect Service](../../connect-service/).
{% endhint %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

### Edit Podfile

{% hint style="info" %}
#### It is required for every iOS project that integrates the Wallet Service SDK.

```ruby
// From 0.9.12, you should add more in podfile
If you use ParticleWalletGUI, you need add this one.
pod 'SkeletonView', :git => 'https://github.com/SunZhiC/SkeletonView.git', :branch => 'main'
pod 'SwiftMessages', :git => 'https://github.com/SunZhiC/SwiftMessages', :branch => 'master'

// If you use PartcleWalletConnect or ConenctWalletConnectAdapter, you need add this one.
pod 'WalletConnectSwift', :git => 'https://github.com/SunZhiC/WalletConnectSwift', :branch => 'master'

// paste there code into pod file
post_install do |installer|
installer.pods_project.targets.each do |target|
  target.build_configurations.each do |config|
  config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
    end
  end
 end
```
{% endhint %}

## Wallet Core Reference

### Solana Service

#### Get the real-time exchange rate of the Solana token

{% tabs %}
{% tab title="Swift" %}
```swift
// native token address is "native", you can add other token mint address.
let addresses: [String] = ["native"]
ParticleWalletAPI.getSolanaService().getPrice(by: addresses, currencies: ["usd"]).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// native token address is "native", you can add other token mint address.
NSArray *addresses = @[@"native"];
NSArray *currencies = @[@"usd"];
[[ParticleWalletAPI getSolanaService] getPriceBy:addresses currencies:currencies successHandler:^(NSArray<TokenPrice *> * tokenPrice) {
    // handle token price
} failureHandler:^(NSError * error) {
    // handle error
}];
```
{% endtab %}
{% endtabs %}

#### Get the token list and NFT list by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// address is user public address
let address: String = ""
ParticleWalletAPI.getSolanaService().getTokensAndNFTs(by: address).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)

// also get from database
ParticleWalletAPI.getEvmService().getTokensAndNFTsFromDB(by: address).subscribe { [weak self] _ in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// address is user public address
NSString *address = @"";
[[ParticleWalletAPI getSolanaService] getTokensAndNFTsBy:address successHandler:^(TokenResult * tokenResult) {
    // handle tokenResult
} failureHandler:^(NSError * error) {
    // handle error
}];

// also get from database
[[ParticleWalletAPI getSolanaService] getTokensAndNFTsFromDBBy:address successHandler:^(TokenResult * tokenResult) {
    // handle tokenResult
} failureHandler:^(NSError * error) {
    // handle error
}];
```
{% endtab %}
{% endtabs %}

#### Get the parsed transaction history by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// address is user public address
let address: String = ""
ParticleWalletAPI.getSolanaService().getTransactions(by: address, beforeSignature: nil, untilSignature: nil, limit: 1000).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)

// also get from database
ParticleWalletAPI.getSolanaService().getTransactionsFromDB(by: address, beforeSignature: nil, untilSignature: nil, limit: 1000).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// address is user public address
NSString *address = @"";
[[ParticleWalletAPI getSolanaService] getTransactionsBy:address beforeSignature:nil untilSignature:nil limit:1000 successHandler:^(NSArray<SolanaTransactionModel *> * transactions) {
    // handle transactions
} failureHandler:^(NSError * error) {
    // handle error
}];

// also get from database
[[ParticleWalletAPI getSolanaService] getTransactionsFromDBBy:address beforeSignature:nil untilSignature:nil limit:1000 successHandler:^(NSArray<SolanaTransactionModel *> * transactions) {
    // handle transactions
} failureHandler:^(NSError * error) {
    // handle error
}];
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
ParticleWalletAPI.getSolanaService().serializeTransaction(type: transactionType, sender: sender, receiver: receiver, lamports: lamports, mintAddress: mintAddress, payer: payer).subscribe { [weak self] result in
    guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
SolanaTransactionType transactionType = SolanaTransactionTypeTransferSol;
NSString *sender = @"";
NSString *receiver = @"";
NSString *lamports = @"";
NSString *mintAddress = nil;
NSString *payer = nil;

[[ParticleWalletAPI getSolanaService] serializeTransactionWithType:transactionType sender:sender receiver:receiver lamports:lamports mintAddress:mintAddress payer:payer successHandler:^(NSString * result) {
    // handle result
} failureHandler:^(NSError * error) {
    // handle error
}];
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

{% tab title="Objective-C" %}
```objectivec
// get solana chain all SPL token info
[[ParticleWalletAPI getSolanaService] getTokenListWithSuccessHandler:^(NSArray<TokenInfo *> * tokenList) {
    // handle tokenList
} failureHandler:^(NSError * error) {
    // handle error
}]
```
{% endtab %}
{% endtabs %}

#### Access 👉 [any RPC ](../../node-service/solana-api/)

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
// native token address is "native", you can add other token contract address.
let addresses = ["native"]
ParticleWalletAPI.getEvmService().getPrice(by: addresses, vsCurrencies: ["usd"]).subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// native token address is "native", you can add other token contract address.
NSArray *addresses = @[@"native"];
NSArray *currencies = @[@"usd"];
[[ParticleWalletAPI getSolanaService] getPriceBy:addresses currencies:currencies successHandler:^(NSArray<TokenPrice *> * tokenPrice) {
    // handle token price
} failureHandler:^(NSError * error) {
    // handle error
}];
```
{% endtab %}
{% endtabs %}

#### Get the token list and NFT list by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// address is user public address
let address = ""
ParticleWalletAPI.getEvmService().getTokensAndNFTs(by: address).subscribe { [weak self] result in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)

// also get from database, no rpc request.
ParticleWalletAPI.getEvmService().getTokensAndNFTsFromDB(by: address).subscribe { [weak self] result in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// address is user public address
NSString *address = @"";
[[ParticleWalletAPI getEvmService] getTokensAndNFTsBy:address successHandler:^(TokenResult * tokenResult) {
    // handle tokenResult
} failureHandler:^(NSError * error) {
    // handle error
}];

// also get from database
[[ParticleWalletAPI getEvmService] getTokensAndNFTsFromDBBy:address successHandler:^(TokenResult * tokenResult) {
    // handle tokenResult
} failureHandler:^(NSError * error) {
    // handle error
}];
```
{% endtab %}
{% endtabs %}

#### Get only tokens or only NFTs

```swift
// get only tokens
let address = ""
ParticleWalletAPI.getEvmService().getTokens(by: address, tokenAddresses: []).subscribe { result in
    // handle result
}.disposed(by: self.bag)

// get only NFTs
let address = ""
ParticleWalletAPI.getEvmService().getNFTs(by: address, tokenAddresses: []).subscribe { result in
    // handle result
}.disposed(by: self.bag)
```

#### Get the parsed transaction history by giving an address

{% tabs %}
{% tab title="Swift" %}
```swift
// address is user public address
let address = ""
ParticleWalletAPI.getEvmService().getTransactions(by: address).subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)

// also get from database
ParticleWalletAPI.getEvmService().getTransactionsFromDB(by: address).subscribe { [weak self] _ in
guard let self = self else { return }
    // handle result
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```
// address is user public address
NSString *address = @"";
[[ParticleWalletAPI getEvmService] getTransactionsBy:address beforeSignature:nil untilSignature:nil limit:1000 successHandler:^(NSArray<SolanaTransactionModel *> * transactions) {
    // handle transactions
} failureHandler:^(NSError * error) {
    // handle error
}];

// also get from database
[[ParticleWalletAPI getEvmService] getTransactionsFromDBBy:address beforeSignature:nil untilSignature:nil limit:1000 successHandler:^(NSArray<SolanaTransactionModel *> * transactions) {
    // handle transactions
} failureHandler:^(NSError * error) {
    // handle error
}];
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

{% tab title="Objective-C" %}
```
// get any EVM chain all token info
[[ParticleWalletAPI getEvmService] getTokenListWithSuccessHandler:^(NSArray<TokenInfo *> * tokenList) {
    // handle tokenList
} failureHandler:^(NSError * error) {
    // handle error
}]
```
{% endtab %}
{% endtabs %}

#### Access 👉 [any RPC](../../node-service/evm-chains-api/)

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

#### Serialize the data of any contract

{% tabs %}
{% tab title="Swift" %}
```swift
// call custom abi function
func customMethodAbiEncode() {
    let contractAddress = ""
    let methodName = ""
    let params: [String] = []
    let abiJsonString = ""
    ParticleWalletAPI.getEvmService().abiEncodeFunctionCall(contractAddress: contractAddress, methodName: methodName, params: params, abiJsonString: abiJsonString).subscribe { [weak self] _ in
        guard let self = self else { return }
        // handle result
    }.disposed(by: bag)
}
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

{% tab title="Objective-C" %}
```objectivec
// call 'transfer(to, amount)' function in erc20
- (void)erc20Transfer {
    NSString *contractAddress = @"";
    NSString *to = @"";
    NSString *amount = @"";
    [[ParticleWalletAPI getEvmService] erc20TransferWithContractAddress:contractAddress to:to amount:amount successHandler:^(NSString * result) {
            // handle result
    } failureHandler:^(NSError * error) {
        // handle error
    }];
}

// call 'approve(spender, amount)' function in erc20 
- (void)erc20Approve {
    NSString *contractAddress = @"";
    NSString *spender = @"";
    NSString *amount = @"";
    [[ParticleWalletAPI getEvmService] erc20ApproveWithContractAddress:contractAddress spender:spender amount:amount successHandler:^(NSString * result) {
            // handle result
    } failureHandler:^(NSError * error) {
        // handle error
    }];
}

// call 'transferFrom(from, to, amount)' function in erc20
- (void)erc20TransferFrom {
    NSString *contractAddress = @"";
    NSString *from = @"";
    NSString *to = @"";
    NSString *amount = @"";
    
    [[ParticleWalletAPI getEvmService] erc20TransferFromContractAddress:contractAddress from:from to:to amount:amount successHandler:^(NSString * result) {
            // handle result
    } failureHandler:^(NSError * error) {
        // handle error
    }];
}

// call 'safeTransferFrom(from, to, tokenId)' function in erc721
- (void)erc721SafeTransferFrom {
    NSString *contractAddress = @"";
    NSString *from = @"";
    NSString *to = @"";
    NSString *tokenId = @"";
    
    [[ParticleWalletAPI getEvmService] erc721SafeTransferFromContractAddress:contractAddress from:from to:to tokenId:tokenId successHandler:^(NSString * result) {
            // handle result
    } failureHandler:^(NSError * error) {
        // handle error
    }];
}

// call 'safeTransferFrom(from, to, id, amount, data)' function in erc1155
- (void)erc1155SafeTransferFrom {
    NSString *contractAddress = @"";
    NSString *from = @"";
    NSString *to = @"";
    NSString *tokenId = @"";
    NSString *amount = @"";
    NSArray *data = [[NSArray alloc] init];
    
    [[ParticleWalletAPI getEvmService] erc1155SafeTransferFromContractAddress:contractAddress from:from to:to id:tokenId amount:amount data:data successHandler:^(NSString * result) {
        // handle result
    } failureHandler:^(NSError * error) {
        // handle error
    }];
}
```
{% endtab %}
{% endtabs %}

#### Create Transaction

{% tabs %}
{% tab title="Swift" %}
```swift
/// Create a transaciton
/// - Parameters:
///   - from: From address
///   - to:If you send a erc20, erc721, erc1155 or interact with a contract, this is the contract address, if you send native, this is receiver address.
///   - contractParams: Acontract parameters
///   - value: Native value, default is nil
///   - type: TxData type, 0x2 is EIP1155, 0x1 is EIP2930, 0x0 is legacy.
///   - nonce: Default value "0x0", particle auth manage nonce without cancel or speed transaction.
///   - gasFeeLevel: Default is medium, transaction gas fee level.
///   - action: Default is normal, means send, if you cancel/speed tracsaction, set the vaule
/// - Returns: A transacion
public func createTransaction(from: String, to: String, value: String? = nil, contractParams: ContractParams? = nil, type: String = "0x2", nonce: String = "0x0", gasFeeLevel: GasFeeLevel = .medium, action: Action = .normal) -> Single<String>


/// Create a transaciton if you have got the data by yourself.
/// - Parameters:
///   - from: From address
///   - to: If you send a erc20, erc721, erc1155 or interact with a contract, this is the contract address, if you send native, this is receiver address.
///   - value: Native value, default is nil, expressed as a hex string.
///   - data: Data
///   - type: TxData type, 0x2 is EIP1155, 0x1 is EIP2930, 0x0 is legacy.
///   - gasFeeLevel: Gas fee level
///   - action: Normal is send, or cancel, speedUp.
/// - Returns: A transacion presented in hex string.
public func createTransaction(from: String, to: String, value: String? = nil, data: String = "0x", type: String = "0x2", gasFeeLevel: GasFeeLevel = .medium, action: Action = .normal) -> Single<String>

func sendNativeEVM() {
    showLoading()
    // firstly, make sure current user has some native token for test there methods
    // send 0.0001 native from self to receiver
    let sender = ParticleAuthService.getAddress()
    let receiver = "0xAC6d81182998EA5c196a4424EA6AB250C7eb175b"
    let amount = BDouble(0.0001 * pow(10, 18)).rounded()
    
    ParticleWalletAPI.getEvmService().createTransaction(from: sender, to: receiver, value: amount.toHexString()).flatMap { transaction -> Single<String> in
        print(transaction)
        return ParticleAuthService.signAndSendTransaction(transaction)
    }.subscribe { [weak self] result in
        guard let self = self else { return }
        switch result {
        case .failure(let error):
            print(error)
        case .success(let signature):
            print(signature)
        }
        self.hideLoading()
    }.disposed(by: self.bag)
}

func sendErc20Token() {
    showLoading()
    // firstly, make sure current user has some native token and erc20 token for test there methods
    // send 0.0001 erc20 token from self to receiver
    let from = ParticleAuthService.getAddress()
    // this token's decimals is 18, convert 0.0001 to minimum unit
    let amount = BDouble(0.0001 * pow(10, 18)).rounded()
    // this is your token contract address
    let contractAddress = "0xa36085F69e2889c224210F603D836748e7dC0088"
    // this is receiver address
    let receiver = "0xAC6d81182998EA5c196a4424EA6AB250C7eb175b"
    
    let contractParams = ContractParams.erc20Transfer(contractAddress: contractAddress, to: receiver, amount: amount)
    
    // because you want to send erc20 token, interact with contact, 'to' should be the contract address.
    // and value could be nil.
    ParticleWalletAPI.getEvmService().createTransaction(from: from, to: contractAddress, value: nil, contractParams: contractParams).flatMap { transaction -> Single<String> in
        print(transaction)
        return ParticleAuthService.signAndSendTransaction(transaction)
    }.subscribe { [weak self] result in
        guard let self = self else { return }
        switch result {
        case .failure(let error):
            print(error)
        case .success(let signature):
            print(signature)
        }
        self.hideLoading()
    }.disposed(by: self.bag)
}

func sendErc721NFT() {
    showLoading()
    // firstly, make sure current user has some native token and the NFT for test there methods
    // send 1 erc721 NFT from self to receiver
    let from = ParticleAuthService.getAddress()
    // this is your nft contract address
    let contractAddress = "0xD18e451c11A6852Fb92291Dc59bE35a59d143836"
    // this is receiver address
    let receiver = "0xAC6d81182998EA5c196a4424EA6AB250C7eb175b"
    // your NFT token id, make sure you own the NFT
    let tokenId = "2302"
    
    let contractParams = ContractParams.erc721SafeTransferFrom(contractAddress: contractAddress, from: from, to: receiver, tokenId: tokenId)
    
    // because you want to send erc721 NFT, interact with contact, 'to' should be the contract address.
    // and value could be nil.
    ParticleWalletAPI.getEvmService().createTransaction(from: from, to: contractAddress, value: nil, contractParams: contractParams).flatMap { transaction -> Single<String> in
        print(transaction)
        return ParticleAuthService.signAndSendTransaction(transaction)
    }.subscribe { [weak self] result in
        guard let self = self else { return }
        switch result {
        case .failure(let error):
            print(error)
        case .success(let signature):
            print(signature)
        }
        self.hideLoading()
    }.disposed(by: self.bag)
}

func sendErc1155NFT() {
    showLoading()
    // firstly, make sure current user has some native token and the NFT for test there methods
    // send 10 erc1155 NFT from self to receiver
    let from = ParticleAuthService.getAddress()
    // this is your nft contract address
    let contractAddress = "0xD18e451c11A6852Fb92291Dc59bE35a59d143836"
    // this is receiver address
    let receiver = "0xAC6d81182998EA5c196a4424EA6AB250C7eb175b"
    // your NFT token id, make sure you own the NFT
    let tokenId = "2302"
    // send amount
    let tokenAmount: BInt = 10
    
    let contractParams = ContractParams.erc1155SafeTransferFrom(contractAddress: contractAddress, from: from, to: receiver, id: tokenId, amount: tokenAmount, data: "0x")
    
    // because you want to send erc1155 NFT, interact with contact, 'to' should be the contract address.
    // and value could be nil.
    ParticleWalletAPI.getEvmService().createTransaction(from: from, to: contractAddress, value: nil, contractParams: contractParams).flatMap { transaction -> Single<String> in
        print(transaction)
        return ParticleAuthService.signAndSendTransaction(transaction)
    }.subscribe { [weak self] result in
        guard let self = self else { return }
        switch result {
        case .failure(let error):
            print(error)
        case .success(let signature):
            print(signature)
        }
        self.hideLoading()
    }.disposed(by: self.bag)
}

func deployContract() {
    // firstly, make sure current user has some native token to pay gas fee.
    // deploy a contract then sign and send with Particle Auth.
    let data = getContractData()
    let from = ParticleAuthService.getAddress()
    let to = ""
    ParticleWalletAPI.getEvmService().createTransaction(from: from, to: to, data: data).flatMap {
        transaction -> Single<String> in
        print("transaction = \(transaction)")
        return ParticleAuthService.signAndSendTransaction(transaction)
    }.subscribe { [weak self] result in
        switch result {
        case .failure(let error):
            print(error)
        case .success(let signature):
            print(signature)
        }
    }.disposed(by: bag)
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
/// Create a transaciton
/// - Parameters:
///   - from: From address
///   - to:If you send a erc20, erc721, erc1155 or interact with a contract, this is the contract address, if you send native, this is receiver address.
///   - contractParams: Acontract parameters
///   - value: native value, default is nil, expressed as a hex string.
///     for example:
///     100000000000000 is “0x5AF3107A4000”
///     3000 is “0xBB8”
///   - type: TxData type, 0x2 is EIP1155, 0x1 is EIP2930, 0x0 is legacy.
///   - nonce: Default value "0x0", particle auth manage nonce without cancel or speed transaction.
///   - gasFeeLevel: Default is medium, transaction gas fee level.
///   - action: Default is normal, means send, if you cancel/speed tracsaction, set the vaule
/// - Returns: A transacion
- (void)createTransactionFrom:(NSString * _Nonnull)from to:(NSString * _Nonnull)to value:(NSString * _Nullable)value contractParams:(ContractParams * _Nullable)contractParams type:(NSString * _Nonnull)type nonce:(NSString * _Nonnull)nonce gasFeeLevel:(enum GasFeeLevel)gasFeeLevel action:(enum Action)action successHandler:(void (^ _Nonnull)(NSString * _Nonnull))successHandler failureHandler:(void (^ _Nonnull)(NSError * _Nonnull))failureHandler;

// Reference cases in github demo.
- (void)sendNativeEVM {
    [self showobjLoading];
    
    // firstly, make sure current user has some native token for test there methods
    // send 0.0001 native from self to receiver
    // 0.0001 native expressed as a hex string is "0x5AF3107A4000"
    NSString *sender = [ParticleAuthService getAddress];
    NSString *receiver = @"0xAC6d81182998EA5c196a4424EA6AB250C7eb175b";
    NSString *amount = @"0x5AF3107A4000";
    __weak APIReferenceViewController *weakSelf = self;
    [[ParticleWalletAPI getEvmService] createTransactionFrom:sender to:receiver value:amount contractParams: nil type:@"0x2" nonce:@"0x0" gasFeeLevel:GasFeeLevelMedium action:ActionNormal successHandler:^(NSString * transaction) {
        NSLog(@"%@", transaction);
        
        [ParticleAuthService signAndSendTransaction:transaction successHandler:^(NSString * signature) {
            NSLog(@"%@", signature);
            [weakSelf hideLoading];
            } failureHandler:^(NSError * error) {
                NSLog(@"%@", error);
                [weakSelf hideLoading];
            }];
        
    } failureHandler:^(NSError * error) {
        NSLog(@"%@", error);
        [weakSelf hideLoading];
    }];
}


- (void)sendErc20Token {
    [self showLoading];
    
    // firstly, make sure current user has some native token and erc20 token for test there methods
    // send 0.0001 erc20 token from self to receiver
    // 0.0001 erc20 token expressed as a hex string is "0x5AF3107A4000", because the token decimals is 18.
    NSString *from = [ParticleAuthService getAddress];
    NSString *to = @"0xa36085F69e2889c224210F603D836748e7dC0088";
    NSString *amount = @"0x5AF3107A4000";
    NSString *contractAddress = @"0xa36085F69e2889c224210F603D836748e7dC0088";
    NSString *receiver = @"0xAC6d81182998EA5c196a4424EA6AB250C7eb175b";
    ContractParams *contractParams = [ContractParams erc20TransferWithContractAddress:contractAddress to:receiver amount:amount];
    
    // because you want to send erc20 token, interact with contact, 'to' should be the contract address.
    // and value could be nil.
    __weak APIReferenceViewController *weakSelf = self;
    [[ParticleWalletAPI getEvmService] createTransactionFrom:from to:to value:nil contractParams:contractParams type:@"0x2" nonce:@"0x0" gasFeeLevel:GasFeeLevelMedium action:ActionNormal successHandler:^(NSString * transaction) {
            NSLog(@"%@", transaction);
            [ParticleAuthService signAndSendTransaction:transaction successHandler:^(NSString * signature) {
                NSLog(@"%@", signature);
                [weakSelf hideLoading];
                } failureHandler:^(NSError * error) {
                    NSLog(@"%@", error);
                    [weakSelf hideLoading];
                }];
        } failureHandler:^(NSError * error) {
            NSLog(@"%@", error);
            [weakSelf hideLoading];
        }];
}
```
{% endtab %}
{% endtabs %}

You can create contractParams object by these `ContractParams`static methods&#x20;

* `ContractParams.erc20Transfer()`
* `ContractParams.erc20Approve()`
* `ContractParams.erc20TransferFrom()`
* `ContractParams.erc721SafeTransferFrom()`
* `ContractParams.erc1155SafeTransferFrom()`
* `ContractParams.customAbiEncodeFunctionCall()`

#### Read Contract

{% tabs %}
{% tab title="Swift" %}
```swift
// example for evm read contract
func readContract() {
    let params = ContractParams.customAbiEncodeFunctionCall(contractAddress: "0xd000f000aa1f8accbd5815056ea32a54777b2fc4", methodName: "balanceOf", params: ["0xBbc1CA8776EfDeC12C75e218C64e96ce52aC6671"])
    ParticleWalletAPI.getEvmService().readContract(contractParams: params).subscribe {
        [weak self] result in
            switch result {
            case .failure(let error):
                print(error)
            case .success(let json):
                print(json)
            }
    }.disposed(by: self.bag)
}
```
{% endtab %}
{% endtabs %}

#### Write Contract

{% tabs %}
{% tab title="Swift" %}
```swift
// example for evm write contract
// mint 1 NFT in kovan testnet, the from is sender address or user address in general.
// result is a transaciton string which presented in hex format.
func writeContract() {
    let params = ContractParams.customAbiEncodeFunctionCall(contractAddress: "0xd000f000aa1f8accbd5815056ea32a54777b2fc4", methodName: "mint", params: ["1"])
    let from = "0x0cf3ffe33e45ad43fcd0aa7016c590b5f629d9aa"
    ParticleWalletAPI.getEvmService().writeContract(contractParams: params, from: from)
    .subscribe {
        [weak self] result in
            switch result {
            case .failure(let error):
                print(error)
            case .success(let transaction):
                print(transaction)
            }
    }.disposed(by: self.bag)
}
```
{% endtab %}
{% endtabs %}

## Wallet UI Reference

### Custom Wallet UI &#x20;

Control show or hide test network, swap feature, buy crypto feature, support wallet connect, manage wallet page, support chain, set language, set user interface style and more.

Because ParticleWalletGUI dependent on Particle Connect, Particle Connect initialize chain will add to support chain automatically.&#x20;

{% tabs %}
{% tab title="Swift" %}
<pre class="language-swift"><code class="lang-swift">// You can disable buy crypto feature in UI, default is true.
ParticleWalletGUI.enablePay(false)

// You can disable swap feature in UI, default value is true.
ParticleWalletGUI.enableSwap(false)

// You can disable wallet connect feature, default value is true.
ParticleWalletGUI.supportWalletConnect(false)

// You can disable dapp browser in wallet page, default value is true.
ParticleWalletGUI.supportDappBrowser(false)

// show or hide test network, default value is false.
ParticleWalletGUI.showTestNetwork(false)

// support chain, Particle Connect initialize chain will add as a support chain automatically.
// default support all chains in Particle Network base
ParticleWalletGUI.supportChain([.bsc, .arbitrum, .harmony])

// show or hide manage wallet page, default value is true.
ParticleWalletGUI.showManageWallet(true)

// show language setting in setting page, default value is false.
ParticleWalletGUI.showLanguageSetting(true)

// show appearance setting in setting page, default value is false.
ParticleWalletGUI.showAppearanceSetting(true)

// set language, default value value is en.
ParticleWalletGUI.setLanguage(Language.en)

// set support add token, default value is true, true will show add token button, false will hide add token button.
ParticleWalletGUI.setSupportAddToken(false)

// Set display token addresses 
// If you called this method, 
// Wallet SDK will only show these tokens in the token addresses, 
// works only on the wallet page token list.
// You can pass nil to reset.
ParticleWalletGUI.setDisplayTokenAddresses([tokenAddress])

// Set display NFT contract addresses
// If you called this method, Wallet SDK will only show NFTs in the NFT contract addresses.
// You can pass nil to reset.
ParticleWalletGUI.<a data-footnote-ref href="#user-content-fn-1">setDisplayNFTContractAddresses</a>([nftContractAddress])

// Set priority token addresses, priority token will show in top part of the list.
// If you called this method, 
// Wallet SDK will show these tokens in top part of the list, 
// works on the wallet page token list, token send select page.
// You can pass nil to reset.
ParticleWalletGUI.setPriorityTokenAddresses([tokenAddress])

// Set priority NFT contract addresses, priority NFT will show in top part of list.
// If you called this method, Wallet SDK will only show NFTs in top part of list.
// You can pass nil to reset.
ParticleWalletGUI.setPriorityNFTContractAddresses([nftContractAddress])

// Set custom token addresses
// If you called this method, Wallet SDK will show these tokens in token list, unless the user hide them
// If you called setDisplayTokenAddresses, this method won't work in wallet page token list.
// The method works on the wallet page token list and token send select page.
ParticleWalletGUI.setCustomTokenAddresses([tokenAddress])

// Set fait coin, currently support USD, CNY, JPY, HKD, INR, KRW.
// If you called this method, we will hide curreny change button in setting page.
// You can pass nil to reset, after reset default fiatcoin is USD and curreny change button will show in setting page.
ParticleWalletGUI.setFiatCoin("HKD")

// There is two way to set custom UI
// 1.You can set custom ui json path to enable custom UI
// In demo, make sure customUIConfig.json is mark Target Membership Demo.
if let path = Bundle.main.path(forResource: "customUIConfig", ofType: "json") {
    try! ParticleWalletGUI.loadCustomUI(path: path)
}
        
// 2.Another way to set custom ui is pass json string
let jsonString = "your custom ui json string, keys should be the same with customUIConfig.json in demo"
try! ParticleWalletGUI.loadCustomUIJsonString(jsonString)
</code></pre>
{% endtab %}
{% endtabs %}

If your want to support wallet connect feature in GUI, should call method `handleWalletConnectUrl`

```swift
// in AppDelegate.swift, under application(_:open:options:)
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
    if ParticleWalletGUI.handleWalletConnectUrl(url, withScheme: "particlewallet") {
        return true
    } else {
        return ParticleConnect.handleUrl(url)
    }
}
```

### Switch Wallet

Particle Wallet GUI support multi wallets, if you switch to another wallet or connect another wallet , you should call this method to make sure Particle Wallet GUI show the right address.

{% tabs %}
{% tab title="Swift" %}
```swift
/// Switch wallet before wallet open any ParticleWalletGUI page.
/// If you swich another wallet in your app, you should call this method to tell gui, which to show later.
/// GUI will sync with ParticleConnect and set default wallet with wallet type and public address.
/// - Parameters:
///   - walletType: Wallet type
///   - publicAddress: Public address
/// - Returns: If wallet is found, return ture and set the default wallet, otherwise return false.
ParticleWalletGUI.switchWallet(walletType: .metaMask, publicAddress: "YOUR_PUBLIC_ADDRESS")
```
{% endtab %}
{% endtabs %}

### Open Wallet

{% tabs %}
{% tab title="Swift" %}
```swift
PNRouter.navigatorWallet()
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
[PNRouter navigatorWalletWithDisplay:DisplayToken];
```
{% endtab %}
{% endtabs %}

### Open Send Token

{% tabs %}
{% tab title="Swift" %}
```swift
//open send token
let tokenSendConfig = TokenSendConfig(tokenAddress: nil, toAddress: nil, amount: nil)
PNRouter.navigatorTokenSend(tokenSendConfig: tokenSendConfig)

//open send default token by chain name
PNRouter.navigator(routhPath: .tokenSend)
```
{% endtab %}

{% tab title="Objctive-C" %}
```objectivec
//open send token
TokenSendConfig *config = [[TokenSendConfig alloc] initWithTokenAddress:nil toAddress:nil amountString:nil];
[PNRouter navigatorTokenSendWithTokenSendConfig:config];
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
// display default QR code
PNRouter.navigatorTokenReceive()
// display QR code with token image in center.
let config = TokenReceiveConfig(tokenAddress: "")
PNRouter.navigatorTokenReceive(tokenReceiveConfig: config)
```
{% endtab %}

{% tab title="Objctive-C" %}
```objectivec
TokenReceiveConfig *config = [[TokenReceiveConfig alloc] initWithTokenAddress:nil];
[PNRouter navigatorTokenReceiveWithTokenReceiveConfig:config];
```
{% endtab %}
{% endtabs %}

### Open Transaction Records

{% tabs %}
{% tab title="Swift" %}
```swift
// open token transaction records
let tokenTransactionRecordsConfig = TokenTransactionRecordsConfig(tokenAddress: tokenAddress)
PNRouter.navigatorTokenTransactionRecords(tokenTransactionRecordsConfig: tokenTransactionRecordsConfig)

//open default token transaction records by chain name
PNRouter.navigator(routhPath: .tokenTransactionRecords)
```
{% endtab %}

{% tab title="Objctive-C" %}
```objectivec
// open token transaction records
NSString *tokenAddress = @"";
TokenTransactionRecordsConfig *config = [[TokenTransactionRecordsConfig alloc] initWithTokenAddress:tokenAddress];
[PNRouter navigatorTokenTransactionRecordsWithTokenTransactionRecordsConfig:config];
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

{% tab title="Objctive-C" %}
```objectivec
// open nft details page
// in solana, tokenId should be an empty string, address is the nft mint address.
// in evm, address is the nft contract address.
NSString *mintAddress = @"";
NSString *tokenId = @"";
NFTDetailsConfig *config = [[NFTDetailsConfig alloc] initWithAddress:mintAddress tokenId:tokenId];
[PNRouter navigatorNFTDetailsWithNftDetailsConfig:config];
```
{% endtab %}
{% endtabs %}

### Open Buy Crypto

{% tabs %}
{% tab title="Swift" %}
```swift
// open buy crypto with parameters
// every parameter is optional
let walletAddress = ""
let network: OpenBuyNetwork = .ethereum
let cryptoCoin = "USDT"
let fiatCoin = "USD"
let fiatAmt = 1000
PNRouter.navigatorBuy(walletAddress: walletAddress, network: network, cryptoCoin: cryptoCoin, fiatCoin: fiatCoin, fiatAmt: fiatAmt)

// open buy crypto with full parameters
let buyCryptoConfig = BuyCryptoConfig(walletAddress: "YOUR WALLET ADDRESS",
                                              network: .ethereum,
                                              cryptoCoin: "ETH",
                                              fiatAmt: 1000,
                                              fiatCoin: "USD",
                                              fixFiatCoin: false,
                                              fixFiatAmt: false,
                                              fixCryptoCoin: false,
                                              theme: "light",
                                              language: "en-US")
        
PNRouter.navigatorBuy(buyCryptoConfig: buyCryptoConfig)
```
{% endtab %}
{% endtabs %}

### Open Swap&#x20;

{% tabs %}
{% tab title="Swift" %}
<pre class="language-swift"><code class="lang-swift"><strong>// open default swap page
</strong><strong>PNRouter.navigatorSwap()
</strong><strong>
</strong>// open swap page with from token, to token, and amount, all parameters is optional.
let config = SwapConfig(fromTokenAddress: nil,
                toTokenAddress: nil,
                fromTokenAmount: nil)
PNRouter.navigatorSwap(swapConfig: config)
</code></pre>
{% endtab %}
{% endtabs %}

### Open dapp browser page

```swift
if let url = URL(string: "app.uniswap.org") {
    PNRouter.navigatorDappBrowser(url: url)
}
```

[^1]: 

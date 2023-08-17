# iOS

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Finish integration with either [Auth Service](../auth-service/sdks/ios.md) or [Connect Service](../connect-service/sdks/ios.md)

Add ParticleBiconomy to your Podfile.

```
pod 'ParticleBiconomy'
```

Install the pods

```
pod install --repo-update
```

## Initialize

The dappApiKeys comes from Biconomy, visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

```swift
let dappApiKeys: [Int: String] = [
    ParticleNetwork.ChainInfo.ethereum(.mainnet).chainId: "YOUR_DAPP_API_KEY",
    ParticleNetwork.ChainInfo.ethereum(.goerli).chainId: "YOUR_DAPP_API_KEY",
    ParticleNetwork.ChainInfo.bsc(.testnet).chainId: "YOUR_DAPP_API_KEY",
    ParticleNetwork.ChainInfo.polygon(.mainnet).chainId: "YOUR_DAPP_API_KEY",
    ParticleNetwork.ChainInfo.polygon(.mumbai).chainId: "YOUR_DAPP_API_KEY"
]
BiconomyService.initialize(dappApiKeys: dappApiKeys)

// after initialize, set a biconomy service object to ParticleNetwork.
let biconomyService = BiconomyService()
ParticleNetwork.setBiconomyService(biconomyService)
```

### Enable biconomy service

After enable biconomy service, The address displayed on the wallet page will display the SmartAccount address. and the signAndSendTransaction method will change the way transactions are sent.

```swift
biconomyService.enableBiconomyMode()
```

### Disable biconomy service

After disable biconomy service, smart account will not work.

```swift
biconomyService.disableBiconomyMode()
```

### Get biconomy service enable state

```swift
let isEnable = biconomyService.isBiconomyModeEnable()
```

### Get fee quotes before send

before send you can check if you have enough native or tokens to pay, or if gasless is available.

```swift
let biconomy = ParticleNetwork.getBiconomyService()!
        
let eoaAddress = ""
let transactions = ["", ""]
let wholeFeeQuote = try await biconomy.rpcGetFeeQuotes(eoaAddress: eoaAddress, transactions: transactions).value
    
if wholeFeeQuote.gasless != nil {
    // you can gasless
} else {
    // you cant gasless
}
    
let natvieFeeQuote = Biconomy.FeeQuote(json: wholeFeeQuote.native.feeQuote, tokenPaymasterAddress: "")
if natvieFeeQuote.isEnoughForPay {
    // you can pay native for gas fee
} else {
    // you dont have enough native to pay
}

let tokenPaymasterAddress = wholeFeeQuote.token.tokenPaymasterAddress
let tokenFeeQuotes = wholeFeeQuote.token.feeQuotes.map {
    Biconomy.FeeQuote(json: $0, tokenPaymasterAddress: tokenPaymasterAddress)
}.filter {
    // filter out balance >= fee
    $0.isEnoughForPay
}

if tokenFeeQuotes.count > 0 {
    // you can select token to pay
} else {
    // you can't select token to pay
}
```

### Send transaction, support multi transactions&#x20;

you need create a BiconomyService instance, then call quickSendTransactions mehod&#x20;

```swift
let biconomyService = BiconomyService()
///   - transactions: The transaction requires a hexadecimal string starting with "0x".
///   - feeMode: Fee mode, if you want to custom feeMode, you need to get a feeQuote from rpcGetFeeQuotes method
///   - messageSigner: Message signer
///   - wholeFeeQuote: Optional, the result of from rpcGetFeeQuotes
biconomyService.quickSendTransactions(transactions, feeMode: .gasless, messageSigner: messageSigner, wholeFeeQuote: wholeFeeQuote)
```

### Send one transaction with [ParticleAuthService](../auth-service/sdks/ios.md)

There are three way to send AA transaction

```swift
// 1. gasless
// you can call the same method to send a transaction in AA mode.
// send gasless transaction, just call rpc to send, never check if it is available.
ParticleAuthService.signAndSendTransaction(tranaction, feeMode: .gasless)

// 2. native
// send user paid transaction, select native to pay gas fee.
ParticleAuthService.signAndSendTransaction(tranaction, feeMode: .native)

// 3. token
// send user paid transaction, select token to pay gas fee.
// select a feeQuote from rpcGetFeeQuotes method
var feeQuote: Biconomy.FeeQuote
ParticleAuthService.signAndSendTransaction(tranaction, feeMode: .token(feeQuote))
```

### Send one transaction with [ParticleConnectService](../connect-service/sdks/ios.md)

There are three way to send AA transaction

```swift
// 1. gasless
// you can call the same method to send a transaction in AA mode.
// send gasless transaction, just call rpc to send, never check if it is available.
adapter.signAndSendTransaction(publicAddress: publicAddress, tranaction, feeMode: .gasless)

// 2. native
// send user paid transaction, select native to pay gas fee.
// just call rpc to send, never check if it is available.
adapter.signAndSendTransaction(publicAddress: publicAddress, tranaction, feeMode: .native)

// 3. token
// send user paid transaction, select token to pay gas fee.
// get a feeQuote from rpcGetFeeQuotes method
// just call rpc to send, never check if it is available.
var feeQuote: Biconomy.FeeQuote
adapter.signAndSendTransaction(publicAddress: publicAddress, tranaction, feeMode: .token(feeQuote))
```

### Deploy Wallet Contract Manual

```typescript
let biconomyService = BiconomyService()
// check the constract is deployed
let isDeploy = try await biconomyService.isDeploy(eoaAddress: eoaAddress).value

// if isDeploy is false, call deployWalletContract to deploy
biconomyService.deployWalletContract(messageSigner: signer, feeMode: .gasless)
```

### More methods

There are other methods, and all Account Abstraction methods are defined in  BiconomyServiceProtocol.

```swift
public protocol BiconomyServiceProtocol {
    /// Rpc method, get smart account
    /// - Parameter addresses: Eoa address array
    /// - Returns: Smart account info array
    func rpcGetSmartAccount(eoaAddresses: [String]) -> Single<[Biconomy.SmartAccountInfo]>
    
    /// Rpc method, get fee quotes
    /// - Parameters:
    ///   - eoaAddress: Eoa address
    ///   - transactions: Transactions array
    /// - Returns: WholeFeeQuote
    func rpcGetFeeQuotes(eoaAddress: String, transactions: [String]) -> Single<Biconomy.WholeFeeQuote>
    
    /// Rpc method, create user op
    /// - Parameters:
    ///   - eoaAddress: Eoa address
    ///   - transactions: Transactions array
    ///   - feeQuote: Fee quote, you can get this feeQuote from rpcGetFeeQuotes method.
    ///   - tokenPaymasterAddress: Token paymaster address
    /// - Returns: UserOp object.
    func rpcCreateUserOp(eoaAddress: String, transactions: [String], feeQuote: Biconomy.FeeQuote, tokenPaymasterAddress: String) -> Single<Biconomy.UserOp>

    /// Rpc method, send user paid transaction
    /// - Parameters:
    ///   - eoaAddress: Eoa address
    ///   - userOp: JSON, get from rpcGetFeeQuotes or rpcCreateUserOp
    /// - Returns: Signature
    func rpcSendUserOp(eoaAddress: String, userOp: JSON) -> Single<String>
    
    /// Get smart account, if it can get smart account from local database, should return it directly, otherwise get it from rpc.
    /// - Parameter eosAddress: Eoa address
    /// - Returns: Smart account info
    func getSmartAccount(by eosAddress: String) -> Single<Biconomy.SmartAccountInfo>
    
    /// Get smart accounts, if it can get smart accounts from local database, should return them directly, otherwise get them from rpc.
    /// - Parameter publicAddreses: Eoa addresses
    /// - Returns: Smart account infos
    func getSmartAccounts(by publicAddreses: [String]) -> Single<[Biconomy.SmartAccountInfo]>
    
    /// Check if support chain info
    /// - Parameter chainInfo: Chain info
    /// - Returns: Result
    func isSupportChainInfo(_ chainInfo: ParticleNetwork.ChainInfo) -> Bool
    
    /// Get all support chain infos
    /// - Returns: ChainInfos
    func getSupportChainInfos() -> [ParticleNetwork.ChainInfo]
}

public protocol MessageSigner {
    /// Sign message
    /// - Parameter message: Message string, such as "0x68656c6c6f20776f726c64"
    /// - Returns: Signature
    func signMessage(_ message: String) -> Single<String>
    
    /// Signer eos address
    /// - Returns: Eoa address
    func getEoaAddress() -> String
}
```


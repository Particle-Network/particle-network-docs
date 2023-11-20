# iOS

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Finish integration with either [Auth Service](../auth-service/sdks/ios.md), [Connect Service](../connect-service/sdks/ios.md) or [Auth Core Service](../auth-service/core/ios.md)

Add ParticleAA to your Podfile.

```
pod 'ParticleAA'
```

Install the pods

```
pod install --repo-update
```

## Before Start

1. All testnets have Particle Verifying Paymaster enabled, you can send any gasless transaction;
2. If you want to use [Particle Verifying Paymaster](paymaster.md) on Mainnet, you can deposit and use it directly without extra configration in SDK, [check docs](paymaster.md);
3. If you want to use Biconomy Paymaster, please go to [https://dashboard.biconomy.io/](https://dashboard.biconomy.io/), create Paymaster & get biconomyApiKey;
4. Token Paymaster is automatically enabled (we are using Biconomy Token Paymaster).

## Initialize

(Optional) The biconomyApiKeys comes from Biconomy, visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

```swift
let biconomyApiKeys: [Int: String] = [
    ParticleNetwork.ChainInfo.ethereum(.mainnet).chainId: "YOUR_BICONOMY_API_KEY",
    ParticleNetwork.ChainInfo.ethereum(.goerli).chainId: "YOUR_BICONOMY_API_KEY",
    ParticleNetwork.ChainInfo.bsc(.testnet).chainId: "YOUR_BICONOMY_API_KEY",
    ParticleNetwork.ChainInfo.polygon(.mainnet).chainId: "YOUR_BICONOMY_API_KEY",
    ParticleNetwork.ChainInfo.polygon(.mumbai).chainId: "YOUR_BICONOMY_API_KEY"
]

// We support biconomy | cyberConnect | simple
AAService.initialize(name: .biconomy, version: .v1_0_0, biconomyApiKeys: biconomyApiKeys)

// Optinonal, if you also add Wallet Service, 
// you can call the method to hidden smart account setting in setting page,
// incase user change your smart account name.
ParticleWalletGUI.setShowSmartAccountSetting(false)

// After initialize, set a aa service object to ParticleNetwork.
let aaService = AAService()
ParticleNetwork.setAAService(aaService)
```

### Enable aa service

After enable aa service, The address displayed on the wallet page will display the smart account address, then the `signAndSendTransaction` method will send transaction as a user operation.

```swift
aaService.enableAAMode()
```

### Disable aa service

After disable aa service, smart account will not work.

```swift
aaService.disableAAMode()
```

### Get aa service enable state

```swift
let isEnable = aayService.isAAModeEnable()
```

### Get smart account address

```swift
func getSmartAccountAddress() async throws {
    let aa = ParticleNetwork.getAAService()!
    let eoaAddress = ""
    let chainInfo = ParticleNetwork.getChainInfo()
    let smartAccount = try await aa.getSmartAccount(by: eoaAddress, chainInfo: chainInfo).value
    print(smartAccount.smartAccountAddress)
}
```

### Send transaction

Here is an example, shows how to send transaction with AA service.

```swift
extension APIReferenceViewController {
    func sendTransactionInAA() async throws {
        let aa = ParticleNetwork.getAAService()!
                
        let chainInfo = ParticleNetwork.getChainInfo()
        let eoaAddress = ""
        let transaction = ""
        let wholeFeeQuote = try await aa.rpcGetFeeQuotes(eoaAddress: eoaAddress, transactions: [transaction], chainInfo: chainInfo).value
            
        if wholeFeeQuote.gasless != nil {
            // There are two ways to send gasless
            // 1, call adapter or ParticleAuthService, requires one transaction
            let txHash1 = try await ParticleAuthService.signAndSendTransaction(transaction, feeMode: .gasless, chainInfo: chainInfo).value
            // 2, call aa, support multi transactions, requires implement MessageSigner delegate.
            let txHash2 = try await aa.quickSendTransactions([transaction], feeMode: .gasless, messageSigner: self, wholeFeeQuote: wholeFeeQuote, chainInfo: chainInfo).value
        } else {
            // you cant gasless
        }
            
        let nativeFeeQuote = AA.FeeQuote(json: wholeFeeQuote.native.feeQuote, tokenPaymasterAddress: nil)
        if nativeFeeQuote.isEnoughForPay {
            // There are two ways to send, pay token
            // 1, call adapter or ParticleAuthService, requires one transaction
            let txHash1 = try await ParticleAuthService.signAndSendTransaction(transaction, feeMode: .native, chainInfo: chainInfo).value
            // 2, call aa, support multi transactions, requires implement MessageSigner delegate.
            let txHash2 = try await aa.quickSendTransactions([transaction], feeMode: .native, messageSigner: self, wholeFeeQuote: wholeFeeQuote, chainInfo: chainInfo).value
        } else {
            // you dont have enough native to pay
        }

        if let token = wholeFeeQuote.token {
            let tokenPaymasterAddress = token.tokenPaymasterAddress
            let tokenFeeQuotes = token.feeQuotes.map {
                AA.FeeQuote(json: $0, tokenPaymasterAddress: tokenPaymasterAddress)
            }.filter {
                // filter out balance >= fee
                $0.isEnoughForPay
            }
            if tokenFeeQuotes.count > 0 {
                // select a feeQuote, here we select the first one.
                let feeQuote = tokenFeeQuotes[0]
                // There are two ways to send, pay token
                // 1, call adapter or ParticleAuthService, requires one transaction
                let txHash1 = try await ParticleAuthService.signAndSendTransaction(transaction, feeMode: .token(feeQuote), chainInfo: chainInfo).value
                // 2, call aa, support multi transactions, requires implement MessageSigner delegate.
                let txHash2 = try await aa.quickSendTransactions([transaction], feeMode: .token(feeQuote), messageSigner: self, wholeFeeQuote: wholeFeeQuote, chainInfo: chainInfo).value
            } else {
                // you can't select token to pay
            }
        }
    }
}

extension APIReferenceViewController: MessageSigner {
    func signMessage(_ message: String, chainInfo: ParticleNetworkBase.ParticleNetwork.ChainInfo?) -> RxSwift.Single<String> {
        // if you are using ParticleAuthService
        return ParticleAuthService.signMessage(message, chainInfo: chainInfo)
        
        // if you are using ParticleConnectService
        // get current adapter by walletType and publicAddress
        // here we assume currentWalletType is Metamask
        
        let currentWalletType = WalletType.metaMask
        let publicAddress = "0x123..."
        let adapters = ParticleConnect.getAllAdapters().filter {
            $0.walletType == currentWalletType
        }
        if let adapter = adapters.first {
            return adapter.signMessage(publicAddress: publicAddress, message: message)
        } else {
            return .error(NSError(domain: "", code: 0))
        }
    }
    
    func getEoaAddress() -> String {
        // if you are using ParticleAuthService
        return ParticleAuthService.getAddress()
        
        // if you are using ParticleConnectService
        let publicAddress = "0x123..."
        return publicAddress
    }
}
```

### Send transaction, support multi transactions

You need create a AAService instance, then call `quickSendTransactions` method

```swift
let aaService = Service()
///   - transactions: The transaction requires a hexadecimal string starting with "0x".
///   - feeMode: Fee mode, if you want to custom feeMode, you need to get a feeQuote from rpcGetFeeQuotes method
///   - messageSigner: Message signer
///   - wholeFeeQuote: Optional, the result of from rpcGetFeeQuotes
aaService.quickSendTransactions(transactions, feeMode: .gasless, messageSigner: messageSigner, wholeFeeQuote: wholeFeeQuote)
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
var feeQuote: AA.FeeQuote
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
var feeQuote: AA.FeeQuote
adapter.signAndSendTransaction(publicAddress: publicAddress, tranaction, feeMode: .token(feeQuote))
```

### Manually deploy a Smart Account

```typescript
let aaService = AAService()
// check the constract is deployed
let isDeploy = try await aaService.isDeploy(eoaAddress: eoaAddress).value

// if isDeploy is false, call deployWalletContract to deploy
aaService.deployWalletContract(messageSigner: signer, feeMode: .gasless)
```

### All methods list

All Account Abstraction methods are defined in AAServiceProtocol.

```swift
public protocol AAServiceProtocol {
    /// Rpc method, get smart account
    /// - Parameters:
    ///   - eoaAddresses: Eoa address array
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Smart account info array
    func rpcGetSmartAccount(eoaAddresses: [String], chainInfo: ParticleNetwork.ChainInfo?) -> Single<[AA.SmartAccountInfo]>
    
    /// Rpc method, get fee quotes
    /// - Parameters:
    ///   - eoaAddress: Eoa address
    ///   - transactions: Transactions array
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: WholeFeeQuote
    func rpcGetFeeQuotes(eoaAddress: String, transactions: [String], chainInfo: ParticleNetwork.ChainInfo?) -> Single<AA.WholeFeeQuote>
    
    /// Rpc method, create user op
    /// - Parameters:
    ///   - eoaAddress: Eoa address
    ///   - transactions: Transactions array
    ///   - feeQuote: Fee quote, you can get this feeQuote from rpcGetFeeQuotes method.
    ///   - tokenPaymasterAddress: Token paymaster address
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: UserOp object.
    func rpcCreateUserOp(eoaAddress: String, transactions: [String], feeQuote: AA.FeeQuote, tokenPaymasterAddress: String, chainInfo: ParticleNetwork.ChainInfo?) -> Single<AA.UserOp>

    /// Rpc method, send user paid transaction
    /// - Parameters:
    ///   - eoaAddress: Eoa address
    ///   - userOp: JSON, get from rpcGetFeeQuotes or rpcCreateUserOp
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Signature
    func rpcSendUserOp(eoaAddress: String, userOp: JSON, chainInfo: ParticleNetwork.ChainInfo?) -> Single<String>
    
    /// Check is eoa address is deployed in current chain info.
    /// - Parameters:
    ///   - eoaAddress: Eoa address.
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Result
    func isDeploy(eoaAddress: String, chainInfo: ParticleNetwork.ChainInfo?) -> Single<Bool>
    
    /// Send a deploy wallet contract transaction
    /// - Parameters:
    ///   - messageSigner: Message signer
    ///   - feeMode: Fee mode
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Signature
    func deployWalletContract(messageSigner: MessageSigner, feeMode: AA.FeeMode, chainInfo: ParticleNetwork.ChainInfo?) -> Single<String>
    
    /// Get is aa mode enable
    /// - Returns: Result
    func isAAModeEnable() -> Bool
    
    /// Enable aa mode
    func enableAAMode()
    
    /// Disable aa mode
    func disableAAMode()
    
    /// Get smart account, if it can get smart account from local database, should return it directly, otherwise get it from rpc.
    /// - Parameters:
    ///   - eosAddress: Eoa address
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Smart account info
    func getSmartAccount(by eosAddress: String, chainInfo: ParticleNetwork.ChainInfo?) -> Single<AA.SmartAccountInfo>
    
    /// Get smart accounts, if it can get smart accounts from local database, should return them directly, otherwise get them from rpc.
    /// - Parameters:
    ///   - publicAddresses: Eoa addresses
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Smart account infos
    func getSmartAccounts(by publicAddresses: [String], chainInfo: ParticleNetwork.ChainInfo?) -> Single<[AA.SmartAccountInfo]>
    
    /// Quick send transaction method
    /// - Parameters:
    ///   - transactions: The transaction requires a hexadecimal string starting with "0x".
    ///   - feeMode: Fee mode
    ///   - messageSigner: Message signer
    ///   - wholeFeeQuote: Optional, get from rpcGetFeeQuotes
    ///   - chainInfo: Optional, default value is current chain info.
    /// - Returns: Signature
    func quickSendTransactions(_ transactions: [String], feeMode: AA.FeeMode, messageSigner: MessageSigner, wholeFeeQuote: AA.WholeFeeQuote?, chainInfo: ParticleNetwork.ChainInfo?) -> Single<String>
    
    /// Check if support chain info, base on current account name and version
    /// - Parameter chainInfo: Chain info
    /// - Returns: Result
    func isSupportChainInfo(_ chainInfo: ParticleNetwork.ChainInfo) -> Bool
    
    /// Get support chain infos, base on current account namr and version
    /// - Returns: Chain infos
    func getSupportChainInfos() -> [ParticleNetwork.ChainInfo]
}

public protocol MessageSigner {
    /// Sign message
    /// - Parameter message: Message string, such as "0x68656c6c6f20776f726c64"
    /// - Parameter chainInfo: Chain info, generally pass your current chain info.
    /// - Returns: Signature
    func signMessage(_ message: String, chainInfo: ParticleNetwork.ChainInfo?) -> Single<String>
    
    /// Signer eos address
    /// - Returns: Eoa address
    func getEoaAddress() -> String
}
```

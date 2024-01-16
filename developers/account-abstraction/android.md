# Android

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

1.Finish integration with either [Auth Service](../auth-service/sdks/android.md) or [Connect Service](../auth-service/sdks/android.md)

2.Add aa service to your build.gradle.

```gradle
dependencies {
    // Particle AA Service
    implementation("network.particle:aa-service:${latest_version}") 
    
    //find the latest version of the sdk:
    //https://search.maven.org/search?q=g:network.particle
    //...
}
```

## Before Start

1. All testnets have Particle Verifying Paymaster enabled, you can send any gasless transaction;
2. If you want to use [Particle Verifying Paymaster](paymaster.md) on Mainnet, you can deposit and use it directly without extra configration in SDK, [check docs](paymaster.md);
3. If you want to use Biconomy Paymaster, please go to [https://dashboard.biconomy.io/](https://dashboard.biconomy.io/), create Paymaster & get dappApiKey;
4. Token Paymaster is automatically enabled (we are using Biconomy Token Paymaster).

## Initialize

```kotlin
val apiKey = mapOf(
            Ethereum.id to "YOUR_DAPP_API_KEY",
            EthereumGoerli.id to "YOUR_DAPP_API_KEY",
            Polygon.id to "YOUR_DAPP_API_KEY"
        )
// apiKey can be empty        
// aaService can be BiconomyV1AAService、BiconomyV2AAService、CyberConnectAAService、 SimpleAAService. It can be null, default  BiconomyV2AAService.
ParticleNetwork.initAAMode(apiKey, aaService = BiconomyAAService)
```

### Enable aa service

After enable AA service, The address displayed on the wallet page will display the SmartAccount address. and the signAndSendTransaction method will change the way transactions are sent.

```kotlin
ParticleNetwork.getAAService().enableAAMode()
```

### Disable AA service

After disable AA service, smart account will not work.

```kotlin
ParticleNetwork.getAAService().disableAAMode()
```

### Get AA service enable state

```kotlin
val isEnable = ParticleNetwork.getAAService().isAAModeEnable()
```

### Send one transaction with [ParticleAuthService](../auth-service/sdks/android.md)

```kotlin
// 1. gasless
// you can call the same method to send a transaction in AA mode.
// send gasless transaction
ParticleNetwork.signAndSendTransaction(tranaction, callback ,FeeModeGasless())

// 2. auto(default）
// send user paid transaction, select native to pay gas fee.
ParticleNetwork.signAndSendTransaction(tranaction, callback , FeeModeAuto())

// 3.custom
// send user paid transaction, select token to pay gas fee.
// get a feeQuote from rpcGetFeeQuotes method
val feeQuote: Erc4337FeeQuote  //get Erc4337FeeQuote by ParticleNetwork.getAAService().rpcGetFeeQuotes()
ParticleNetwork.signAndSendTransaction(tranaction, callback , FeeModeCustom(feeQuote))
```

### Send one transaction with [ParticleConnectService](../connect-service/sdks/android.md)

There are three way to send AA transaction

```kotlin
// 1. gasless
// you can call the same method to send a transaction in AA mode.
// send gasless transaction
connectAdapter.signAndSendTransaction(publicAddress = publicAddress, transaction =  tranaction, FeeModeGasless(), callback)
   
// 2. auto(default）
// send user paid transaction, select native to pay gas fee.
connectAdapter.signAndSendTransaction(publicAddress = publicAddress, transaction =  tranaction, FeeModeAuto(), callback)

// 3.custom
// send user paid transaction, select token to pay gas fee.
// get a feeQuote from rpcGetFeeQuotes method
val feeQuote: Erc4337FeeQuote  //get Erc4337FeeQuote by ParticleNetwork.getAAService().rpcGetFeeQuotes()
connectAdapter.signAndSendTransaction(publicAddress = publicAddress, transaction =  tranaction, FeeModeCustom(feeQuote), callback)
```

### Send batch transactions

you need create a AAService instance, then call quickSendTransactions mehod

```kotlin
ParticleNetwork.getAAService().quickSendTransaction(transactions, feeMode, messageSigner, callback)
```

### More methods

There are other methods, and all Account Abstraction methods are defined in AAServiceProtocol.

```
interface IAAService {
    /**
     * Rpc method, get smart account
     * @param  addresses: Eoa address List
     * @return Smart SmartAccountInfo List
     */
    suspend fun rpcGetSmartAccount(addresses: List<String>): RpcOutput<List<SmartAccountInfo>>

    /**
     * Rpc method, get fee quotes
     * @param  eoaAddress: Eoa address
     * @param  transactions: Transaction List
     * @return RpcOutput<ArrayList<Erc4337FeeQuote>>
     */
    suspend fun rpcGetFeeQuotes(
        eoaAddress: String, transactions: List<PrefixedHexString>
    ): RpcOutput<ArrayList<Erc4337FeeQuote>>


    /**
     *  Rpc method, create user paid transaction
     *  @param  eoaAddress: Eoa address
     *  @param  transactions: Transaction List
     *  @param  erc4337FeeQuote: Fee quote, you can get this feeQuote from rpcGetFeeQuotes method.
     *  @return ResponseBody, parse it by JSONObject.
     */
    suspend fun rpcCreateUserPaidTransaction(
        eoaAddress: String, transactions: List<PrefixedHexString>, erc4337FeeQuote: Erc4337FeeQuote
    ): ResponseBody

    /**
     *  Rpc method, send user paid transaction
     *  @param  eoaAddress: Eoa address
     *  @param  walletTransaction: WalletTransaction
     *  @param  signature: Signature
     *  @return RpcOutput<String>
     */
    suspend fun rpcSendUserPaidTransaction(
        eoaAddress: String, walletTransaction: WalletTransaction, signature: String
    ): RpcOutput<String>


    /**
     *  Rpc method, send Gasless transaction
     *  @param  eoaAddress: Eoa address
     *  @param  userOp: WalletUserOp
     *  @param  signature: Signature
     *  @return RpcOutput<String>
     */
    suspend fun rpcSendGaslessTransaction(
        eoaAddress: String, userOp: WalletUserOp, signature: String
    ): RpcOutput<String>

    /**
     *  Rpc method, create Gasless transaction
     *  @param  eoaAddress: Eoa address
     *  @param  transaction: transaction
     *  @param  signature: Signature
     *  @return RpcOutput<GaslessTransaction>
     */
    suspend fun rpcCreateGaslessTransaction(
        eoaAddress: String, transactions: List<PrefixedHexString>
    ): RpcOutput<GaslessTransaction>

    suspend fun isDeploy(eoaAddress: String): Boolean

    suspend fun deployWalletContract(messageSigner: MessageSigner, feeMode: FeeMode)

    fun isAAModeEnable(): Boolean
    
    fun enableAAMode()
    
    fun disableAAMode()

    suspend fun getSmartAccount(eosAddress: String): SmartAccountInfo?

    fun getSmartAccountSync(eosAddress: String): SmartAccountInfo?
    
    fun getSmartAddress(eosAddress: String): String?
    
    suspend fun getSmartAccounts(eosAddresses: List<String>): List<SmartAccountInfo>

    fun isSupportChainInfo(chainInfo: ChainInfo): Boolean

    fun getSupportChainInfos(): List<ChainInfo>
    
    suspend fun quickSendTransaction(
        transactions: String, feeMode: FeeMode, messageSigner: MessageSigner, sendCallback: WebServiceCallback<SignOutput>
    )

    suspend fun quickSendTransaction(
        transactions: List<String>, feeMode: FeeMode, messageSigner: MessageSigner, sendCallback: WebServiceCallback<SignOutput>
    )

}
```

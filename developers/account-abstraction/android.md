# Android

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

1.Finish integration with either [Auth Service](../auth-service/sdks/android.md) or [Connect Service](../auth-service/sdks/android.md)

2.Add ParticleBiconomy to your build.gradle.

```gradle
dependencies {
    // Particle Biconomy Service
    implementation("network.particle:biconomy-service:${latest_version}") 
    
    //find the latest version of the sdk:
    //https://search.maven.org/search?q=g:network.particle
    //...
}
```

## Initialize

The dappApiKeys comes from Biconomy, visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

```kotlin
 val apiKey = mapOf(
            EthereumChainId.Mainnet.id to "YOUR_DAPP_API_KEY",
            EthereumChainId.Goerli.id to "YOUR_DAPP_API_KEY",
            BscChainId.Testnet.id to "YOUR_DAPP_API_KEY",
            PolygonChainId.Mainnet.id to "YOUR_DAPP_API_KEY",
            PolygonChainId.Mumbai.id to "YOUR_DAPP_API_KEY",
        )
ParticleNetwork.initBiconomyMode(apiKey,BiconomyVersion.V100)
ParticleNetwork.setBiconomyService(BiconomyService)
```

### Enable biconomy service

After enable biconomy service, The address displayed on the wallet page will display the SmartAccount address. and the signAndSendTransaction method will change the way transactions are sent.

```kotlin
ParticleNetwork.getBiconomyService().enableBiconomyMode()
```

### Disable biconomy service

After disable biconomy service, smart account will not work.

```kotlin
ParticleNetwork.getBiconomyService().disableBiconomyMode()
```

### Get biconomy service enable state

```kotlin
val isEnable = ParticleNetwork.getBiconomyService().isBiconomyModeEnable()
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
val feeQuote: Erc4337FeeQuote  //get Erc4337FeeQuote by ParticleNetwork.getBiconomyService().rpcGetFeeQuotes()
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
val feeQuote: Erc4337FeeQuote  //get Erc4337FeeQuote by ParticleNetwork.getBiconomyService().rpcGetFeeQuotes()
connectAdapter.signAndSendTransaction(publicAddress = publicAddress, transaction =  tranaction, FeeModeCustom(feeQuote), callback)
```

### Send batch transactions&#x20;

you need create a BiconomyService instance, then call quickSendTransactions mehod&#x20;

```kotlin
ParticleNetwork.getBiconomyService().quickSendTransaction(transactions, feeMode, messageSigner, callback)
```

### More methods

There are other methods, and all Account Abstraction methods are defined in  BiconomyServiceProtocol.

```
interface IBiconomyService {
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

    fun isBiconomyModeEnable(): Boolean
    
    fun enableBiconomyMode()
    
    fun disableBiconomyMode()

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


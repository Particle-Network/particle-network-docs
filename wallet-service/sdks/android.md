# Android

## Add Wallet Service to Your Android Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Make sure that your project meets the following requirements:

* Targets API Level 23 (Marshmallow) or higher
* Uses Android 6.0 or higher
* Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)

### Create a Particle Project and App

Before you can add Wallet Service to your Android app, you need to create a Particle project to connect to your Android app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Wallet Service SDK to your app <a href="#add-sdks" id="add-sdks"></a>

Declare them in your module (app-level) Gradle file (usually app/build.gradle).

{% tabs %}
{% tab title="Kotlin" %}
```kts
repositories {
    google()
    mavenCentral()
    maven { setUrl("https://jitpack.io") }
    //...
}

dependencies {
    // Particle Auth Service
    implementation("network.particle:auth-service${latest_version}")
    // Particle Wallet Api
    implementation("network.particle:api-service-api${latest_version}")
    // Particle Wallet GUI, you can remove it if custom GUI.
    implementation("network.particle:wallet-service${latest_version}")
    //...
}
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
repositories {
    google()
    mavenCentral()
    maven { url "https://jitpack.io" }
    //...
}

dependencies {
    // Particle Auth Service
    implementation 'network.particle:auth-service${latest_version}'
    // Particle Wallet Api
    implementation 'network.particle:wallet-service-api${latest_version}'
    // Particle Wallet GUI, you can remove it if custom GUI.
    implementation 'network.particle:wallet-service${latest_version}'
    //...
}
```
{% endtab %}
{% endtabs %}

The Wallet Service can only be used after a successful log-in with Auth Service.

{% hint style="info" %}
Wallet Service depends on Auth Service, you must import [Auth Service](../../auth-service/sdks/android.md).
{% endhint %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

## Wallet API Reference

### Solana Service

#### Get the real-time exchange rate of the Solana token

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call enhanced method: enhancedGetPrice
ParticleNetwork.solana.getPrice(addresses, currencies)
```
{% endtab %}
{% endtabs %}

#### Get token list and NFT list by giving an address

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call enhanced method: enhancedGetTokensAndNFTs
//fetch from server、save to db and return 
ParticleNetwork.solana.getTokensAndNFTs(address)
//get data from db only
ParticleNetwork.solana.getTokensAndNFTsFromDB(address)
```
{% endtab %}
{% endtabs %}

#### Get parsed transaction history by giving an address

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call enhanced method: enhancedGetTransactionsByAddress
ParticleNetwork.solana.getTransactionsByAddress(address, optBody)
//get trans from db
ParticleNetwork.solana.getTransactionsByAddressFromDB(address, optBody)
```
{% endtab %}
{% endtabs %}

#### Serialize unsigned transaction

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call enhanced method: enhancedSerializeTransaction
ParticleNetwork.solana.serializeTransaction(txBody)
```
{% endtab %}
{% endtabs %}

#### Get Token info list&#x20;

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// get solana chain all SPL token info
ParticleNetwork.solana.getTokenList()
```
{% endtab %}
{% endtabs %}

#### Access any standard RPC

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// such as getBalance
ParticleNetwork.solana.rpc("getBalance", 
    listOf("8FE27ioQh3T7o22QsYVT5Re8NnHFqmFNbdqwiF3ywuZQ"))
```
{% endtab %}
{% endtabs %}

### EVM Service

#### Get the real-time exchange rate of the EVM token

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
val parmas1 = arrayListOf("native") 
val parmas2 = arrayListOf("usd") 
val rateResultList = ParticleNetwork.evm.getPrice(parmas1, parmas2)
```
{% endtab %}
{% endtabs %}

#### Get token list and NFT list by giving an address

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call enhanced method: particle_getTokensAndNFTs
//fetch from server、save to db  return result 
ParticleNetwork.evm.getTokensAndNFTs(address)
//get data from db only
ParticleNetwork.evm.getTokensAndNFTsFromDB(address)
```
{% endtab %}
{% endtabs %}

#### Get parsed transaction history by giving an address

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call enhanced method: particle_getTransactionsByAddress
ParticleNetwork.evm.getTransactionsByAddress(address)
//get data from db only
ParticleNetwork.evm.getTransactionsByAddressFromDB(address)
```
{% endtab %}
{% endtabs %}

#### Get suggested GasFees

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.evm.particleSuggestedGasFees()
```
{% endtab %}
{% endtabs %}

#### Get Token info list&#x20;

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// get any EVM chain all token info
ParticleNetwork.evm.getTokenList()
```
{% endtab %}
{% endtabs %}

#### Access any standard RPC

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// such as eth_estimateGas
ParticleNetwork.evm.rpc("eth_estimateGas",listOf(mapOf("from" to "0xXXX...", "to" to "0xXXX...")))
```
{% endtab %}
{% endtabs %}

#### Serialization the data of contract

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.evm.abiEncodeFunctionCall(params: List<Any>)

// 1. The first parameter is contract address
// 2. The second parameter is the name of the method to be called.
//    Currently, we support:'erc20_transfer'、'erc20_approve'、'erc20_transferFrom'、'erc721_safeTransferFrom'、'erc1155_safeTransferrom'
//    or arbitrary method call: 'custom_{methodName}'
//    for example if you need to call mint method, then pass custom_mint as parameter
// 3. The third parameter is the parameter value array of the method to be called, which should correspond to the above method.
// 4. The fourth parameter is optional，an ABI JOSN string
//    For erc20/721/1155 methods we support, this parameter is not needed
//    And if your contract is verified at the chain's scan website, this parameter is not needed
```


{% endtab %}
{% endtabs %}

#### Serialization the data of contract

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// call 'transfer(to, amount)' function in erc20
suspend fun erc20Transfer(){
    val contractAddress =""
    val to=""
    val amount:BigInteger = BigInteger.valueOf(1111)
    ParticleNetwork.evm.erc20Transfer(contractAddress,to,amount)
}
// call 'approve(spender, amount)' function in erc20 
suspend fun erc20Approve() {
    val contractAddress = ""
    val spender = ""
    val amount = BigInteger.valueOf(1111)
    val result: ResponseBody =
        ParticleNetwork.evm.erc20Approve(contractAddress, spender, amount)
}
// call 'transferFrom(from, to, amount)' function in erc20
suspend fun erc20TransferFrom() {
    val contractAddress = ""
    val from = ""
    val to = ""
    val amount = BigInteger.valueOf(1111)
    val result: ResponseBody =
        ParticleNetwork.evm.erc20TransferFrom(contractAddress, from, to, amount)
}
// call 'safeTransferFrom(from, to, tokenId)' function in erc721
suspend fun erc721SafeTransferFrom() {
    val contractAddress = ""
    val from = ""
    val to = ""
    val tokenId = ""
    val result: ResponseBody =
        ParticleNetwork.evm.erc721SafeTransferFrom(contractAddress, from, to, tokenId)
}
// call 'safeTransferFrom(from, to, id, amount, data)' function in erc1155
suspend fun erc1155SafeTransferFrom() {
    val contractAddress = ""
    val from = ""
    val to = ""
    val id = ""
    val amount = BigInteger.valueOf(1111)
    val data: String;
    ParticleNetwork.evm.erc1155SafeTransferFrom(contractAddress, from, to, id, amount, data)
}
```
{% endtab %}
{% endtabs %}

## Wallet Service Reference

### Open Wallet

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
PNRouter.build(RouterPath.Wallet).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
PNRouter.build(RouterPath.Wallet).navigation();
```
{% endtab %}
{% endtabs %}

`navigation()`can add parameters, `callback` to listen to results.

### Open Send Token

{% tabs %}
{% tab title="Kotiln" %}
```kotlin
//open send spl token
val params = WalletSendParams(tokenAddress, toAddress?, toAmount?)
PNRouter.build(RouterPath.TokenSend, params).navigation()

//open send default token by chain name
PNRouter.build(RouterPath.TokenSend).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
//open send spl token
WalletSendParams params = new WalletSendParams(tokenAddress, toAddress, toAmount);
PNRouter.build(RouterPath.TokenSend, params).navigation();

//open send default token by chain name
PNRouter.build(RouterPath.TokenSend).navigation();
```
{% endtab %}
{% endtabs %}

`WalletSendParam` has three parameters:

1. `tokenAddress`: token mint address
2. `toAddress`: receiver address (optional)
3. `toAmount`: send token amount, token minimum unit (optional)

### Open Receive Token

Display your address QRCode.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
PNRouter.build(RouterPath.TokenReceive).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
PNRouter.build(RouterPath.TokenReceive).navigation();
```
{% endtab %}
{% endtabs %}

### Open Transaction Records

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//open spl token transaction records
val params = TokenTransactionRecordsParams(tokenAddress)
PNRouter.build(RouterPath.TokenTransactionRecords, params).navigation()

//open default token transaction records by chain name
PNRouter.build(RouterPath.TokenTransactionRecords).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
//open spl token transaction records
TokenTransactionRecordsParams params = new TokenTransactionRecordsParams(tokenAddress);
PNRouter.build(RouterPath.TokenTransactionRecords, params).navigation();

//open default token transaction records by chain name
PNRouter.build(RouterPath.TokenTransactionRecords).navigation();
```
{% endtab %}
{% endtabs %}

### Open NFT Details

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
var params = NftDetailParams("nftMint")
PNRouter.build(RouterPath.NftDetails, params).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
 NftDetailParams params = new NftDetailParams("nftMint")
 PNRouter.build(RouterPath.NftDetails, params).navigation()
```
{% endtab %}
{% endtabs %}


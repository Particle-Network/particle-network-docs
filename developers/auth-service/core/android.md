# Android

## Add Auth Core to Your Android Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Make sure that your project meets the following requirements:

* minSdkVersion must be 23 or higher
* compileSdkVersion and targetSdkVersion must be 33 or higher
* Uses JavaVersion 11
* Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)

### Create a Particle Project and App

Before you can add Auth Core to your Android app, you need to create a Particle project to connect your app. Visit [Particle Dashboard](../../../getting-started/dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Auth Core SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Declare them in your module (app-level) Gradle file (usually app/build.gradle).

<pre class="language-groovy"><code class="lang-groovy">repositories {
    google()
    mavenCentral()
    maven { setUrl("https://jitpack.io") }
    //...
}
dependencies {
    // Particle Auth Core
    implementation("network.particle:auth-core:${latest_version}")
    implementation("network.particle:mpc-core:${latest_version}")
    
    //find the latest version of the sdk:
<strong>    //https://search.maven.org/search?q=g:network.particle
</strong>    //...
}
</code></pre>

### Add Configuration File

Declare them in you app AndroidManifest.xml (usually app/src/main/AndroidManifest.xml).

```xml
<!-- add fullBackupContent to exclude particle encrypted file -->
<application android:fullBackupContent="@xml/pn_backup_rules">
        <!--   Particle Network config start     -->
        <meta-data
                android:name="particle.network.project_id"
                android:value="${pn_project_id}"
                />
        <meta-data
                android:name="particle.network.project_client_key"
                android:value="${pn_project_client_key}"
                />
        <meta-data
                android:name="particle.network.app_id"
                android:value="${pn_app_id}"
                />
        <!--   Particle Network config end     -->
</application>
```

{% hint style="info" %}
Replace <mark style="color:red;">**pn\_project\_id**</mark>, <mark style="color:red;">**pn\_project\_client\_key**</mark>, <mark style="color:red;">**pn\_app\_id**</mark> with the new values created in Particle Dashboard.
{% endhint %}

## API Reference

### Initialize the SDK

Initialize the SDK by calling the `ParticleNetwork.init()`method, passing the method a context. Do this as soon as your app starts, like in the `onCreate()`method of your `Application`.

```kotlin
import com.particle.base.Env
import com.particle.base.LanguageEnum
import com.particle.base.ParticleNetwork
import network.particle.chains.ChainInfo

class App : Application() {
    override fun onCreate() {
        super.onCreate()
        ParticleNetwork.init(this, Env.PRODUCTION, ChainInfo.Ethereum)
    }
}
```

Particle Network support Solana and EVM chains, you can init with below chain info:

[👉 chainId and chainName configs](../../node-service/evm-chains-api/)

### Login

Login Particle with JWT.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
AuthCore.connect(jwt, callback)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
After log-in success, you can obtain user info by calling AuthCore`.getUserInfo()`
{% endhint %}

### Logout

The SDK will delete users' account information in cache.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
AuthCore.disconnect(callback)
```
{% endtab %}
{% endtabs %}

### Check user login status

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
AuthCore.isConnected()
```
{% endtab %}
{% endtabs %}

### Get Wallet Address

```
// EVM wallet address
val evmAddress = AuthCore.evm.getAddress()

// Solana wallet address
val solanaAddress = AuthCore.solana.getAddress()
```

### Switch ChainInfo

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
AuthCore.switchChain(ChainInfo.BNBChain, callback)
```
{% endtab %}
{% endtabs %}

### Signatures

Use Auth Core SDK to sign a transaction or message.

{% tabs %}
{% tab title="EVM" %}
```kotlin
// personal sign
AuthCore.evm.personalSign(message, object : AuthCoreSignCallback<SignOutput> {
        override fun success(output: SignOutput) {
            val signature = output.signature
        }

        override fun failure(errMsg: ErrorInfo) {
            
        }
    })
    
// personal sign unique
AuthCore.evm.personalSignUnique(message, callback)

// sign typed data
AuthCore.evm.ko(typedData, callback)

//sign typed data unique
AuthCore.evm.signTypedDataUnique(typedData, callback)

//send evm transaction
AuthCore.evm.sendTransaction(transaction, callback)

// request public rpc
AuthCore.evm.request(payload)
```
{% endtab %}

{% tab title="Solana" %}
```
// sign message
AuthCore.solana.signMessage(message, callback)

// sign transaction
AuthCore.solana.signTransaction(transaction, callback)

// sign all transactions
AuthCore.solana.signAllTransactions(transactions, callback)

// sign and send transaction
AuthCore.solana.signAndSendTransaction(transaction, callback)

// request public rpc
AuthCore.solana.request(payload)
```
{% endtab %}
{% endtabs %}

### Master Password

Wallet can set master password to protect assets.

```kotlin
// check user has master password or not
AuthCore.hasMasterPassword()

// set master password
AuthCore.setMasterPassword(callback)

//change master password
AuthCore.changeMasterPassword(callback)
```

### Payment Password

If set a payment password, user should input password before sign message and transaction.

```kotlin
// check user has payment password or not
AuthCore.hasPaymentPassword()
```

### Open account and security

Bind more login account, manage payment password etc.

```kotlin
AuthCore.openAccountAndSecurity(context)
```

### Open Buy

You can open Particle Ramp to buy crypto tokens.

```kotlin
AuthCore.openBuy(
    activity, 
    walletAddress: String? = null,//nullable,default is current wallet address
    amount: Int? = null, //1000
    fiatCoin: String = "usd", 
    cryptoCoin: String = "eth",
    fixFiatCoin: Boolean = false,
    fixFiatAmt: Boolean = false,
    fixCryptoCoin: Boolean = false,
    theme: String = "light",  // light or dark
    language: String = "en-us", //en-us、ko-kr、zh-cn、zh-tw、ja-jp
    chainName: String? = null // ChainInfo.Ethereum.name、ChainInfo.Polygon.name ... , nullable,default ParticleNetwork.chainInfo.name
)
```

### Set security account config

```kotlin
// set security account config, 
// promptSettingWhenSign default value is 1.
// promptMasterPasswordSettingWhenLogin default value is 0.
// 0 no prompt
// 1 first time show prompt
// 2 every time show prompt
 ParticleNetwork.setSecurityAccountConfig(
    SecurityAccountConfig(
        promptSettingWhenSign = 1,
        promptMasterPasswordSettingWhenLogin = 2
    )
)
```

### Custom interface style

```kotlin
//this is the default setting
ParticleNetwork.setAppearence(ThemeEnum.AUTO)
//dark 
ParticleNetwork.setAppearence(ThemeEnum.DARK)
//light
ParticleNetwork.setAppearence(ThemeEnum.LIGHT)
```

### Language setting

```kotlin
//Set the language of the SDK
//support LanguageEnum.EN、LanguageEnum.JA、LanguageEnum.KO、LanguageEnum.ZH_CN、LanguageEnum.ZH_TW
ParticleNetwork.setLanguage(LanguageEnum.EN)

//Get Applied Language
val languageEnum = ParticleNetwork.getLanguage()
```

### Blind sign enable

This switch will work if the following conditions are met:

1. your account is connected with JWT
2. your account does not set payment password
3. SecurityAccountConfig.promptSettingWhenSign is 0, you can call ParticleNetwork.setSecurityAccountConfig to update its value.

```kotlin
// set blind sign enable
AuthCore.setBlindEnable(true)
// get current blind sign enable state
val result = AuthCore.getBlindEnable()
```

### Error

`ErrorInfo` contains error details. You can check the information by printing the `message` attribute.


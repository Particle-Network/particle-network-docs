# Android

## Add Auth Service to Your Android Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Make sure that your project meets the following requirements:
  * Targets API Level 23 (Marshmallow) or higher
  * Uses Android 6.0 or higher
  * Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)

### Create a Particle Project and App

Before you can add Auth Service to your Android app, you need to create a Particle project to connect to your Android app. Visit [Particle Dashboard](../../../getting-started/dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Auth Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

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
    implementation("network.particle:auth-service:${latest_version}") 
    
    //find the latest version of the sdk:
    //https://search.maven.org/search?q=g:network.particle
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
    //...
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

### Add Configuration File

Declare them in you app AndroidManifest.xml (usually app/src/main/AndroidManifest.xml).

```xml
<application>
        <!--   Particle Network config start     -->
        <activity
                android:name="com.particle.network.controller.WebActivity"
                android:exported="true"
                android:launchMode="singleTask"
                android:configChanges="orientation|keyboardHidden|screenSize"
                android:theme="@style/ThemeAuthWeb"
                >
            <intent-filter>
                <data android:scheme="pn${pn_app_id}" />
        
                <action android:name="android.intent.action.VIEW" />
        
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
            </intent-filter>
        </activity>
        
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
Replace <mark style="color:red;">**pn\_project\_id**</mark>, <mark style="color:red;">**pn\_project\_client\_key**</mark>, <mark style="color:red;">**pn\_app\_id**</mark> with the new values created in your Dashboard.
{% endhint %}

## API Reference

### Initialize the SDK

Initialize the SDK by calling the `ParticleNetwork.init()`method, passing the method a context. Do this as soon as your app starts, like in the `onCreate()`method of your `Application`.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
import com.particle.network.ChainId
import com.particle.network.ChainName
import com.particle.network.Env
import com.particle.network.ParticleNetwork

class App : Application() {
    override fun onCreate() {
        super.onCreate()
        //init Particle SDK for solana chain 
        ParticleNetwork.init(this, Env.DEV, SolanaChain(SolanaChainId.Mainnet))
        
        //init wallet service if you use wallet-service
        ParticleWallet.init(this)
    }
}
```
{% endtab %}

{% tab title="Java" %}
```java
import com.particle.network.ChainId;
import com.particle.network.ChainName;
import com.particle.network.Env;
import com.particle.network.ParticleNetwork;

public class App extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        //init Particle SDK with solana chain info
        ParticleNetwork.init(this, Env.DEV, new SolanaChain(SolanaChainId.Mainnet));
        
        //init wallet service if you use wallet-service
        ParticleWallet.init(this);
    }
}
```
{% endtab %}
{% endtabs %}

Particle Network support Solana and EVM chains, you can init with below chain info:

* `EthereumChain`
* `BscChain`
* `PolygonChain`
* `AvalancheChain`
* `MoonbeamChain`
* `MoonriverChain`
* `HecoChain`
* `FantomChain`
* `ArbitrumChain`
* `HarmonyChain`
* `AuroraChain`

### Switch  ChainInfo

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// Async switch chain name, it will check if user has logged in this chain name.
// For example, if a user logged in with ethereum, then switch to  bsc,
// it will switch to bsc directly, beacuse both bsc and ethereum are evm,
// but if switch to solana, beacuse user didn't log in solana before, it will 
// present a web browser for additional information automatically.
val chainInfo = EthereumChain(EthereumChainId.Mainnet)
ParticleNetwork.setChainInfo(
    chainInfo,
    object : ChainChangeCallBack {
        override fun success() {
            ToastUtils.showLong("currChain:${ParticleNetwork.chainInfo.chainName}")
        }

        override fun failure() {
            ToastUtils.showLong("success failure:${ParticleNetwork.chainInfo.chainName}")
        }
    })
    
// Sync switch chain name. it will wont check if user has logged in.
val chainInfo = EthereumChain(EthereumChainId.Mainnet)
ParticleNetwork.setChainInfo(chainInfo)
```
{% endtab %}

{% tab title="Java" %}
```java

// Async switch chain name, it will check if user has logged in this chain name.
// For example, if a user logged in with ethereum, then switch to  bsc,
// it will switch to bsc directly, beacuse both bsc and ethereum are evm,
// but if switch to solana, beacuse user didn't log in solana before, it will 
// present a web browser for additional information automatically.
ChainInfo chainInfo = new SolanaChain(SolanaChainId.Mainnet);
ParticleNetworkAuth.setChainInfo(ParticleNetwork.INSTANCE, chainInfo,
        new ChainChangeCallBack() {
            @Override
            public void success() {
                
            }

            @Override
            public void failure() {

            }
        });
// Sync switch chain name. it will wont check if user has logged in.
ParticleNetwork.setChainInfo(chainInfo);
```
{% endtab %}
{% endtabs %}



### Login

To auth login with Particle, call `ParticleNetwork.login(...)`with `activity` and `callback`. You can log in with your email or phone number by changing the `loginType` parameter. Your wallet is created when you log in successfully for the first time.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//   - Parameters:
//   - loginType: Login type, support jwt,email, phone, google, apple and facebook
//   - account: When login type is email or phone, you could pass email address or phone number,Optional.
                When login type is jwt, you must pass json web token.
//   - supportAuthType: Controls whether third-party login buttons are displayed. default will show none third-party login buttons.
//   - loginFormMode: Controls whether show light UI in web, default is false.
//   - prompt: Social login prompt. default is null
ParticleNetwork.login(
        loginType = LoginType.PHONE,
        account = "",
        supportAuthTypeValues = SupportAuthType.FACEBOOK.value or SupportAuthType.GOOGLE.value or SupportAuthType.APPLE.value,
        loginFormMode = false,
        prompt = null,
        loginCallback = object : WebServiceCallback<LoginOutput> {
            override fun success(output: LoginOutput) {
            }
            override fun failure(errMsg: WebServiceError) {
            }
        })

// - without account parameter you can use this method overwrite
ParticleNetwork.login(
        loginType = LoginType.PHONE,
        supportAuthTypeValues = SupportAuthType.FACEBOOK.value or SupportAuthType.GOOGLE.value or SupportAuthType.APPLE.value,
        loginFormMode = false,
        loginCallback = object : WebServiceCallback<LoginOutput> {
            override fun success(output: LoginOutput) {
            }
            override fun failure(errMsg: WebServiceError) {
            }
        })
        
        
// Login with JWT
val account = "json web token"
    ParticleNetwork.login(
        loginType = LoginType.JWT,
        account = account,
        supportAuthTypeValues = SupportAuthType.ALL.value,
        loginFormMode = false,
        object : WebServiceCallback<LoginOutput> {
            override fun success(output: LoginOutput) {
            }
            override fun failure(errMsg: WebServiceError) {
            }
        })
```
{% endtab %}

{% tab title="Java" %}
```java
//   - Parameters:
//   - loginType: Login type, support email, phone, google, apple and facebook
//   - account: When login type is email or phone, you could pass email address or phone number,Optional.
//   - supportAuthType: Controls whether third-party login buttons are displayed. default will show none third-party login buttons.
//   - loginFormMode:Controls whether show light UI in web, default is false.
//   - prompt: Social login prompt. default is null
ParticleNetworkAuth.login(ParticleNetwork.INSTANCE,LoginType.PHONE,"",
        SupportAuthType.FACEBOOK.getValue() | SupportAuthType.GOOGLE.getValue()| SupportAuthType.APPLE.getValue(),
        false,
        null,
        new WebServiceCallback<LoginOutput>() {
            @Override
            public void failure(@NonNull WebServiceError errMsg) {
            }
            @Override
            public void success(@NonNull LoginOutput output) {
            }
        });

// - without account parameter you can use this method overwrite
ParticleNetworkAuth.login(ParticleNetwork.INSTANCE,LoginType.PHONE,
        SupportAuthType.FACEBOOK.getValue() | SupportAuthType.GOOGLE.getValue()| SupportAuthType.APPLE.getValue(),
        false,
        new WebServiceCallback<LoginOutput>() {
            @Override
            public void failure(@NonNull WebServiceError errMsg) {
            }
            @Override
            public void success(@NonNull LoginOutput output) {
            }
        });
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
After log-in success, you can obtain user info by calling `ParticleNetwork.getUserInfo()`
{% endhint %}

### Logout

The SDK will delete users' account information in cache.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.logout(object : WebServiceCallback<WebOutput> {
    override fun success(output: WebOutput) {
        //logout success
    }

    override fun failure(errMsg: WebServiceError) {
        //handle error
    }
})
```
{% endtab %}

{% tab title="Java" %}
```java
ParticleNetworkAuth.logout(ParticleNetwork.INSTANCE,new WebServiceCallback<WebOutput>() {
    @Override
    public void success(@NonNull WebOutput output) {
        //logout success
    }

    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});
```
{% endtab %}
{% endtabs %}

### Fast Logout

Logout without broswer,The SDK will delete users' account information in cache.

{% tabs %}
{% tab title="Kotlin" %}
<pre class="language-kotlin"><code class="lang-kotlin"><strong>ParticleNetwork.fastLogout(object : FastLogoutCallBack {
</strong>    override fun success() {
        //logout success
    }

    override fun failure() {
        //handle error
    }
})
</code></pre>
{% endtab %}

{% tab title="Java" %}
```java
ParticleNetworkAuth.fastLogout(ParticleNetwork.INSTANCE, new FastLogoutCallBack() {
    @Override
    public void success() {
        //logout success
    }

    @Override
    public void failure() {
        //logout failure
    }
});
```
{% endtab %}
{% endtabs %}

### Is User Logged In

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.isLogin()
```
{% endtab %}

{% tab title="Java" %}
```java
ParticleNetworkAuth.isLogin(ParticleNetwork.INSTANCE)
```
{% endtab %}
{% endtabs %}

### Signatures

Use Particle SDK to sign a transaction or message. The SDK provides three methods for signing:

1. `signAndSendTransaction`: sign and send the transaction with Particle Node, then return the signature.
2. `signTransaction`: sign transaction, return signed message, only Solana blockchain support it.
3. `signMessage`: sign message, return signed message.
4. `signTypedData` : sign typed data，only EVM blockchain support it.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//solana transaction: base58 string. 
//evm transcation:'0x' prefixed hex string.
ParticleNetwork.signAndSendTransaction(transaction, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign and send transaction success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})


//solana transaction: base58 string. evm not support this method.
ParticleNetwork.signTransaction(transaction, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign transaction success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})

//transaction: request base58 string list in solana, not support evm
val allTrans = listOf(trans1, trans2)
ParticleNetwork.signAllTransactions(
    activity,
    allTrans,
    object : WebServiceCallback<SignOutput> {
        override fun success(output: SignOutput) {
            //sign success
        }
        override fun failure(errMsg: WebServiceError) {
            // handle error
        }
    })

//solana message: any string.
//evm message: '0x' prefixed hex string, use 'personal_sign' to sign.
ParticleNetwork.signMessage(message, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})

//sign typed data, only evm support this method.
//message: '0x' prefixed hex string.
ParticleNetwork.signTypedData(message, version, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})

```
{% endtab %}

{% tab title="Java" %}
```java
//transaction: base58 string
 ParticleNetworkAuth.signAndSendTransaction(ParticleNetwork.INSTANCE,transaction, new WebServiceCallback<SignOutput>() {

    @Override
    public void success(@NonNull SignOutput output) {
        //sign and send transaction success
    }

    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});


//transaction: base58 string
ParticleNetworkAuth.signTransaction(ParticleNetwork.INSTANCE,transaction, new WebServiceCallback<SignOutput>() {

    @Override
    public void success(@NonNull SignOutput output) {
        //sign transaction success
    }

    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});


//sign any string
ParticleNetworkAuth.signMessage(ParticleNetwork.INSTANCE,message, new WebServiceCallback<SignOutput>() {

    @Override
    public void success(@NonNull SignOutput output) {
        //sign success
    }

    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});
```
{% endtab %}
{% endtabs %}

You can create `transaction` with `TxData`.There's an easy way to do this with [Wallet Service](../../wallet-service/)

### Set Display Wallet

```kotlin
// set display wallet when call sign and send transaction. default is false
ParticleNetwork.setDisplayWallet(true) 
```

### Open Web Wallet

```kotlin
// if user is login, it will open web wallet.
ParticleNetwork.openWebWallet()
```

### Open account and security

```kotlin
ParticleNetwork.openAccountAndSecurity()
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

### Language setting&#x20;

```kotlin
//Set the language of the SDK
//support LanguageEnum.EN、LanguageEnum.JA、LanguageEnum.KO、LanguageEnum.ZH_CN、LanguageEnum.ZH_TW
ParticleNetwork.setAppliedLanguage(LanguageEnum.EN)

//Get Applied Language
val languageEnum = ParticleNetwork.getAppliedLanguage()
```

### Control setpassword dialog

If the user does not set a password, a dialog box will pop up when the user sends a transaction,

If you don't want to prompt the user, you can call the following code

```kotlin
ParticleNetwork.setSecurityAccountConfig(SecurityAccountConfig(false))
```

### Error

`WebServiceError` contains error details. You can check the information by printing the `message` attribute.

## Particle Wallet Connect V1

Integrate your app as a wallet connect wallet.

### Add the Particle Wallet Connect  SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
repositories {
    google()
    mavenCentral()
    maven { setUrl("https://jitpack.io") }
    //...
}
dependencies {
    // Particle Wallet Connect Service
    implementation("network.particle:particle-wallet-connect:${latest_version}")
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
    implementation 'network.particle:particle-wallet-connect:${latest_version}'
    //...
}
```
{% endtab %}
{% endtabs %}

### Initialize

{% tabs %}
{% tab title="Kotlin" %}
<pre class="language-kotlin"><code class="lang-kotlin">// Note: If you want to add listening events, you need to put them before init
<strong>ParticleWalletConnect.onXXListeners = {
</strong>    ...
}
// Initialize Particle Wallet Connect SDK, a wallet meta data
val wcPeerMeta = WCPeerMeta( "Particle Wallet", "https://particle.network",icons = listOf("https://connect.particle.network/icons/512.png"))
ParticleWalletConnect.init(context,wcPeerMeta)
</code></pre>
{% endtab %}
{% endtabs %}

### Connect

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
// Connect to a wallet connect code
val wcCode = "wc:DDCBBAA8-B1F1-4B98-8F74-84939E0B1533@1?bridge=https%3A%2F%2Fbridge%2Ewalletconnect%2Eorg%2F&key=3da9dbb33b560beeb1750203a8d0e3487b4fe3fdd7b7953d79fbccadae8aab48"
ParticleWalletConnect.connect()
```
{% endtab %}
{% endtabs %}

### Set Provider  And Listeners&#x20;

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//require: This method needs to be called before ParticleWalletConnect.init
//AuthProvider Implemented the following methods：
//personal_sign、eth_signTypedData_v1、eth_signTypedData_v3、eth_signTypedData_v4、eth_sendTransaction、eth_chainId、eth_accounts、eth_requestAccounts
//Here you can replace  your own Provider
ParticleWalletConnect.iprovider = { topic, method, params ->
    AuthProvider.request(method, params, object : WebServiceCallback<WebOutput> {
        override fun success(output: WebOutput) {
            when (output) {
                is SignOutput -> {}
                is EthChainId -> {}
                is EthAccounts -> {}
            }
        }
        override fun failure(errMsg: WebServiceError) {
            LogUtils.d("AuthProvider request failure", method, errMsg)
        }
    })
}

//approve or reject
ParticleWalletConnect.onSessionRequest = { id, peerMeta ->
    //approve
    ParticleWalletConnect.approveSession(context,"PublicAddress",chainId)
    //or reject
    ParticleWalletConnect.rejectSession()
}
//disconnect session
ParticleWalletConnect.onDisconnect = { code, topic, reason ->
    //disconnect
    ParticleWalletConnect.delWCSessionStoreItem(topic)
}

//receive change network request
ParticleWalletConnect.onWalletChangeNetwork = { id, topic, chainId ->
    //show dialog to switch network
}
```
{% endtab %}
{% endtabs %}

### Session Operation

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//when ParticleWalletConnect.approveSession() is call,a new Session can be saved
//get All Session Info
ParticleWalletConnect.getAllWCSessionStoreItem()
//get Session by topic
ParticleWalletConnect.getWCSessionStoreItem(topic)
//delete Session by topic
ParticleWalletConnect.delWCSessionStoreItem(topic)
//delete all Sessions
ParticleWalletConnect.clearWCSessionStore()
```
{% endtab %}
{% endtabs %}


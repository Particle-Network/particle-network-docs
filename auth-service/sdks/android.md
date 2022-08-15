# Android

## Add Auth Service to Your Android Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Make sure that your project meets the following requirements:
  * Targets API Level 23 (Marshmallow) or higher
  * Uses Android 6.0 or higher
  * Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)

### Create a Particle Project and App

Before you can add Auth Service to your Android app, you need to create a Particle project to connect to your Android app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

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
                android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen"
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
//   - loginType: Login type, support email, phone, google, apple and facebook
//   - account: When login type is email or phone, you could pass email address or phone number,Optional.
//   - supportAuthType: Controls whether third-party login buttons are displayed. default will show none third-party login buttons.
//   - loginFormMode: Controls whether show light UI in web, default is false.
ParticleNetwork.login(
        loginType = LoginType.PHONE,
        account = "",
        supportAuthTypeValues = SupportAuthType.FACEBOOK.value or SupportAuthType.GOOGLE.value or SupportAuthType.APPLE.value,
        loginFormMode = false,
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
```
{% endtab %}

{% tab title="Java" %}
```java
//   - Parameters:
//   - loginType: Login type, support email, phone, google, apple and facebook
//   - account: When login type is email or phone, you could pass email address or phone number,Optional.
//   - supportAuthType: Controls whether third-party login buttons are displayed. default will show none third-party login buttons.
//   - loginFormMode:Controls whether show light UI in web, default is false.
ParticleNetworkAuth.login(ParticleNetwork.INSTANCE,LoginType.PHONE,"",
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

You can create `transaction` with `TxData`.There's an easy way to do this with [Wallet Service](broken-reference)

### Error

`WebServiceError` contains error details. You can check the information by printing the `message` attribute.

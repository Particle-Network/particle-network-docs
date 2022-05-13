# Android

## Add Auth Service to your Android project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Make sure that your project meets these requirements:
  * Targets API level 23 (Marshmallow) or higher
  * Uses Android 6.0 or higher
  * Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)

### Create Particle project and app

Before you can add Auth Service to your Android app, you need to create a Particle project to connect to your Android app. Visit [Broken link](broken-reference "mention") to learn more about Particle projects and app.

[👉 Sign up/log in and create your project now](https://particle.network/#login)

### Add Auth Service SDK to your app <a href="#add-sdks" id="add-sdks"></a>

Declare them in your module (app-level) Gradle file (usually app/build.gradle).

{% tabs %}
{% tab title="Kotlin" %}
```kts
repositories {
    google()
    mavenCentral()
    //...
}
dependencies {
    // Particle Auth Service
    implementation("particle.network:auth-service${latest_version}")
    //...
}
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
repositories {
    google()
    mavenCentral()
    //...
}
dependencies {
    // Particle Auth Service
    implementation 'particle.network:auth-service${latest_version}'
    //...
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

### Add configuration file

Declare them in you app AndroidManifest.xml (usually app/src/main/AndroidManifest.xml).

```xml
<application>
        <!--   Particle Network config start     -->
        <activity
                android:name="com.particle.network.controller.WebActivity"
                android:exported="true"
                android:launchMode="singleTop"
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
Replace <mark style="color:red;">**pn\_project\_id**</mark>, <mark style="color:red;">**pn\_project\_client\_key**</mark>, <mark style="color:red;">**pn\_app\_id**</mark> with you created in Dashboard**.**
{% endhint %}

## API Reference

### Initialize the SDK

Initialize the SDK by calling the `ParticleNetwork.init()`method, passing that method a context. Do this as soon as possible after your app starts, for example in the `onCreate()`method of your `Application`.

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
        //init Particle SDK
        ParticleNetwork.init(this, 
            env = Env.DEV, 
            chainName = ChainName.Solana, 
            chainId = ChainId.SolanaDevnet,
            )
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
        //init Particle SDK
        ParticleNetwork.init(this, Env.DEV, ChainName.Solana, 
                ChainId.SolanaDevnet);
    }
}
```
{% endtab %}
{% endtabs %}

You can dynamic switching chain id by call `ParticleNetwork.setChainId()`method`, env` needs to be modified to be Env.`PRODUCTION` for release.

### Login

To auth login with Particle, call `ParticleNetwork.login(...)`with `activity` and `callback`. You can login with email or phone by change `loginType` param. Your wallet will be created when login success first time.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.login(activity, LoginType.EMAIL, object : WebServiceCallback<LoginOutput> {
    override fun success(output: LoginOutput) {
        //login success, return user info in output
    }

    override fun failure(errMsg: WebServiceError) {
        //handle error
    }
})
```
{% endtab %}

{% tab title="Java" %}
```java
ParticleNetwork.login(activity, LoginType.EMAIL, new WebServiceCallback<LoginOutput>() {
    @Override
    public void success(@NonNull LoginOutput output) {
        //login success, return user info in output
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle err
    }
});
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
After login success, you call get user info by call `ParticleNetwork.getUserInfo()`
{% endhint %}

### Logout

SDK will delete user info in cache.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.logout(activity, object : WebServiceCallback<WebOutput> {
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
ParticleNetwork.logout(this, new WebServiceCallback<WebOutput>() {
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

### Sign

Use Particle SDK sign a transaction or message, SDK provides three methods for sign:

1. `signAndSendTransaction`: sign transaction and send with particle node, return signature.
2. `signTransaction`: sign transaction, return signed message.
3. `signMessage`: sign message, return signed message.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//transaction base58 string
ParticleNetwork.signAndSendTransaction(activity, transaction, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign and send transaction success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})


//transaction base58 string
ParticleNetwork.signTransaction(activity, transaction, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign transaction success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})


//sign any string
ParticleNetwork.signMessage(activity, message, object : WebServiceCallback<SignOutput>{
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
//transaction base58 string
ParticleNetwork.signAndSendTransaction(activity, transaction, new WebServiceCallback<SignOutput>() {
    
    @Override
    public void success(@NonNull SignOutput output) {
        //sign and send transaction success
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});


//transaction base58 string
ParticleNetwork.signTransaction(activity, transaction, new WebServiceCallback<SignOutput>() {
    
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
ParticleNetwork.signMessage(activity, message, new WebServiceCallback<SignOutput>() {
    
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

### Error

`WebServiceError` contain error details, you can check the information by printing the  `message` attribute.

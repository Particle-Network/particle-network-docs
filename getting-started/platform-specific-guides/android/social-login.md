# Social Login

This documentation provides a comprehensive guide on integrating and utilizing the ParticleConnect SDK in your Android application, with a primary focus on Social Login functionality. ParticleConnect facilitates seamless connectivity and authentication using social media accounts in your mobile applications.

By leveraging Social Login, your users can easily authenticate themselves using their preferred social media accounts, simplifying the onboarding process. Upon successful authentication, ParticleConnect enables you to obtain a web3 wallet address associated with the user's account.

**Configuration**

To integrate ParticleConnect into your project, follow these steps:

**Gradle Dependencies:**

```gradle
dependencies {
    // Required
    modules {
        module("org.bouncycastle:bcprov-jdk15to18") {
            replacedBy("org.bouncycastle:bcprov-jdk15on")
        }
    }
    implementation 'network.particle:connect:{latest-version}'
    implementation 'network.particle:connect-auth-adapter:{latest-version}'
}
```

Find the latest version of the SDK [here](https://search.maven.org/search?q=g:network.particle).

**Manifest Configuration:**

```xml
<application>
    <!-- Particle Network Configuration Start -->
    <!-- Web Activity -->
    <activity
        android:name="com.particle.network.controller.WebActivity"
        android:exported="true"
        android:launchMode="singleTask"
        android:configChanges="orientation|keyboardHidden|screenSize"
        android:theme="@style/ThemeAuthWeb">
        <intent-filter>
            <data android:scheme="pn${PN_APP_ID}" />
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
        </intent-filter>
    </activity>
    
    <!-- Redirect Activity -->
    <activity
        android:name="com.connect.common.controller.RedirectActivity"
        android:exported="true"
        android:launchMode="singleTask"
        android:configChanges="orientation|keyboardHidden|screenSize"
        android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="connect${PN_APP_ID}" />
        </intent-filter>
    </activity>
​
    <!-- Metadata Configuration -->
    <meta-data
        android:name="particle.network.project_id"
        android:value="${PN_PROJECT_ID}" />
    <meta-data
        android:name="particle.network.project_client_key"
        android:value="${PN_PROJECT_CLIENT_KEY}" />
    <meta-data
        android:name="particle.network.app_id"
        android:value="${PN_APP_ID}" />
    <!-- Particle Network Configuration End -->
</application>
```

**Gradle Properties:** In your `app/build.gradle.kts` file, add the following placeholders:

```groovy
android {
   defaultConfig {
        minSdk = 23 // minimum requirements
        targetSdk = pnTargetSdk
        //...
    }
    // ...
    defaultConfig {
        // https://dashboard.particle.network/
        manifestPlaceholders["PN_PROJECT_ID"] = "772f7499-1d2e-40f4-8e2c-7b6dd47db9de"
        manifestPlaceholders["PN_PROJECT_CLIENT_KEY"] = "ctWeIc2UBA6sYTKJknT9cu9LBikF00fbk1vmQjsV"
        manifestPlaceholders["PN_APP_ID"] = "01a23ce8-d2e9-4b37-9eab-bf477279e53e"
    }
    // ...
}
```

You can obtain these values from [Particle Network Dashboard](https://dashboard.particle.network/).

**Initialization**

Initialize ParticleConnect in your application:

```kotlin
ParticleConnect.init(this, Env.PRODUCTION, Ethereum){
    listOf(ParticleConnectAdapter()) 
}
```

**Login**

Use the following code snippet to perform login:

```kotlin
val pnAdapter = ParticleConnect.getAdapters().first { it.name == MobileWCWalletName.Particle.name }
val config = ParticleConnectConfig(
    loginType = LoginType.EMAIL,
    supportAuthTypeValues =SupportAuthType.PHONE.value or  SupportAuthType.GOOGLE.value or SupportAuthType.FACEBOOK.value
)
pnAdapter.connect(config, object : ConnectCallback {
    override fun onConnected(account: Account) {
        //current chain address,
        //if current chain is evm chain, it is 0x address, 
        //if current chain is solana chain, it is base58 address
        val address = account.publicAddress  
    }

    override fun onError(error: ConnectError) {
        //handle error
    }
})
```

**Conclusion**

This concludes the integration of ParticleConnect into your Android application with a focus on Social Login, enabling you to effortlessly obtain a web3 wallet address through the authentication process.

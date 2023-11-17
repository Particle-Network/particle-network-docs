# Wallet Connect

**Introduction**

This documentation provides a detailed guide on integrating and utilizing the ParticleConnect SDK in your Android application, focusing on Wallet Connect functionality. ParticleConnect simplifies the connection of decentralized wallets to your DApp through the Wallet Connect protocol.

**Configuration**

To integrate ParticleConnect into your project for Wallet Connect, follow these steps:

**Gradle Dependencies:**

```gradle
dependencies {
    // Required
    modules {
        module("org.bouncycastle:bcprov-jdk15to18") {
            replacedBy("org.bouncycastle:bcprov-jdk15on")
        }
    }
    implementation("network.particle:connect:{latest-version}")
    implementation("network.particle:connect-wallet-connect-adapter:{latest-version}")
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

1.  **Gradle Properties:** In your `app/build.gradle.kts` file, add the following placeholders:

    ```gradle
    android {
        defaultConfig {
            minSdk = 23
            targetSdk = pnTargetSdk
            //...
        }
        // ...
        defaultConfig {
            manifestPlaceholders["PN_PROJECT_ID"] = "Replace-With-Your-Project-ID"
            manifestPlaceholders["PN_PROJECT_CLIENT_KEY"] = "Replace-With-Your-Project-Client-Key"
            manifestPlaceholders["PN_APP_ID"] = "Replace-With-Your-App-ID"
        }
        // ...
    }
    ```

    You can obtain these values from [Particle Network Dashboard](https://dashboard.particle.network/).

**Initialization**

Initialize ParticleConnect in your application with Wallet Connect support:

<pre class="language-kotlin"><code class="lang-kotlin">
val dAppMetadata = DAppMetadata(
   <a data-footnote-ref href="#user-content-fn-1">walletConnectProjectId</a> = "",
   name = "Particle Connect",
   icon = "https://connect.particle.network/icons/512.png",
   url = "https://particle.network",
   description = "Particle Connect is a decentralized wallet connection protocol that makes it easy for users to connect their wallets to your DApp.",
   )
​
ParticleConnect.init(this.application, Env.PRODUCTION, Ethereum, dAppMetadata) {
    listOf(
        MetaMaskConnectAdapter(),
        RainbowConnectAdapter(),
        TrustConnectAdapter(),
        WalletConnectAdapter(),
        ImTokenConnectAdapter(),
        BitKeepConnectAdapter()
    )
}
</code></pre>

**Login**

Use the following code snippet to perform login with Wallet Connect:

<pre class="language-kotlin"><code class="lang-kotlin">val walletName = MobileWCWalletName.<a data-footnote-ref href="#user-content-fn-2">MetaMask</a>
val metaMaskConnectAdapter = ParticleConnect.getAdapters().first {
     it.name == walletName.name 
 }
val config = null // ParticleConnectConfig only used for ParticleConnectAdapter, other adapter can be null
metaMaskConnectAdapter.connect(config, object : ConnectCallback {
    override fun onConnected(account: Account) {
        //connected wallet's  address, it is 0x address
        val address = account.publicAddress
    }

    override fun onError(error: ConnectError) {
        //process error
    }
})
</code></pre>

**Conclusion**

This concludes the Wallet Connect integration guide for ParticleConnect in your Android application. Utilize this functionality to seamlessly connect decentralized wallets to your DApp.

[^1]: get it from [https://walletconnect.com/](https://walletconnect.com/)

[^2]: ```
    ImToken
    BitKeep
    Trust
    Rainbow
    WalletConnect
    ```

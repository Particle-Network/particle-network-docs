---
description: SSO for Web3
---

# Android

## Github Demo

[https://github.com/Particle-Network/particle-connect-android](https://github.com/Particle-Network/particle-connect-android)

## Summary

Modular Kotlin wallet adapters and components for EVM & Solana chains. Manage wallet and custom RPC request.

![Particle Connect](https://static.particle.network/docs-images/particle-connect.jpeg)

<mark style="color:red;">**It is strongly discouraged to use private key or mnemonic import/generate function, if you use it, you need to secure the data yourself, Particle's SDK has no relationship with the imported/generated mnemonic or private key.**</mark>

## Quick Start

```groovy
dependencies {
    //required 
     modules {
        module("org.bouncycastle:bcprov-jdk15to18") {
            replacedBy("org.bouncycastle:bcprov-jdk15on")
        }
    }
    //required dependencies
    implementation 'network.particle:auth-service:{latest-version}'
    implementation 'network.particle:connect:{latest-version}'
    // pick one of two
    implementation 'network.particle:connect-auth-adapter:{latest-version}'
    //implementation 'network.particle:connect-auth-core-adapter:{latest-version}'
    
    //Optional: support evm chain Generate & Import wallet
    implementation 'network.particle:connect-evm-adapter:{latest-version}'
    
    //Optional: support solana chain Generate & Import wallet
    implementation 'network.particle:connect-solana-adapter:{latest-version}'
    
    //Optional: support connect Phantom wallet
    implementation 'network.particle:connect-phantom-adapter:{latest-version}'
    
    //Optional: support WalletConnect Protocol, include MetaMask, Rainbow, Trust, imToken etc.
    implementation 'network.particle:connect-wallet-connect-adapter:{latest-version}'
    
    //find the latest version of the sdk:
    //https://search.maven.org/search?q=g:network.particle
}
```

Add below config to AndroidManifest.xml

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

### Init Particle Connect in `Application#Create()`

```kotlin
val dAppMetadata = DAppMetadata(
                walletConnectProjectId = "f431aaea6e4dea6a669c0496f9c009c1",
                name ="Particle Connect",
                icon= "https://connect.particle.network/icons/512.png",
                url = "https://particle.network",
                description = "Particle Connect is a decentralized wallet connection solution that allows users to connect to DApps with their wallets.",
                redirect = "redirect://",
                verifyUrl = "verifyUrl",
            )
ParticleConnect.init(
        this,
        Env.DEV,    //debug mode
        ChainInfo.Ethereum,    //chain info
        dAppMetadata   //DApp or Wallet info
    ) {
        listOf(
            ParticleConnectAdapter(),
            //AuthCoreAdapter(),
            MetaMaskConnectAdapter(),
            RainbowConnectAdapter(),
            TrustConnectAdapter(),
            ImTokenConnectAdapter(),
            BitKeepConnectAdapter(),
            WalletConnectAdapter(),
            PhantomConnectAdapter(),
            EVMConnectAdapter(),
            SolanaConnectAdapter(),
        )  //list all support adapters, lazy create.
    }
```

### Switch chain.

```kotlin
ParticleConnect.setChain(chain)
```

### Get all wallet adapters.

```kotlin
var adapters = ParticleConnect.getAdapters(chainTypes)
//or
var adapters = ParticleConnect.getAdapterByAddress(address)
```

### Get all connected accounts.

```kotlin
val accounts = ParticleConnect.getAccounts(chainTypes)
```

### Connect wallet.&#x20;

(For `EVMConnectAdapter` or `SolanaConnectAdapter` will generate new wallet)

```kotlin
connectAdapter.connect(callback)
```

### Disconnect wallet.

```kotlin
connectAdapter.disconnect(address, callback)
```

### Check whether the account is connected.

```kotlin
val result = connectAdapter.connected(address)
```

### Import wallet.

(Only `EVMConnectAdapter` and `SolanaConnectAdapter` support this method)

```kotlin
// import wallet with private key
val account = connectAdapter.importWalletFromPrivateKey(privateKey)

// import wallet with mnemonic(Split with space).
val account = connectAdapter.importWalletFromMnemonic(mnemonic)
```

### Export wallet.&#x20;

(Only `EVMConnectAdapter` and `SolanaConnectAdapter` support this method)

```kotlin
val privateKey = connectAdapter.exportWalletPrivateKey(address)
```

### Sign and send transaction.

```kotlin
// todo: check connected before sign
connectAdapter.signAndSendTransaction(address, transaction, callback)
```

### Sign transaction.

(Only Solana chain support this method)

```kotlin
connectAdapter.signTransaction(address, transaction, callback)
```

### Sign all transactions.

(Only Solana chain support this method)

```kotlin
connectAdapter.signAllTransactions(address, transactions, callback)
```

### Sign message.

(EVM call `personal_sign`)

```kotlin
connectAdapter.signMessage(address, message, callback)
```

### Sign typed data.

(Only EVM chains support this method)

<pre class="language-kotlin"><code class="lang-kotlin"><strong>connectAdapter.signTypedData(address, data, callback)
</strong></code></pre>

### Login\&Verify locally

```kotlin
// We have full example in github demo.
val msg = Eip4361Message.createWithRequiredParameter(

            "particle.network",
            "https://service.org/login",
            "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc21"
        )
val message = eip4361Message.toString()
adapter.login(
    publicAddress,
    eip4361Message,
    object : SignCallback {
        override fun onSigned(signature: String) {
            // Verify locally
            val verifyResult = adapter.verify(
                publicAddress,
                signature,
                message
            )
        }

        override fun onError(error: ConnectError) {
        }
    })
```

### Custom wallet connect adapter

```kotlin
// How to define a custom wallet connect adapter
// for examle coin98 wallet
// 1. add queries in manifest ,
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.connect.demo">
        <queries>
            ...
            <package android:name="coin98.crypto.finance.media" />
        </queries>
        <application>
        </application>
    </manifest>
// 2. subclass from BaseWalletConnectAdapter
// 3. override name、icon、url、 mobileWallet and readyState
class Coin98ConnectAdapter : BaseWalletConnectAdapter() {

    val coin98 = MobileWCWallet(name = "Coin98", packageName = "coin98.crypto.finance.media", scheme = "coin98")

    override val name: WalletName = coin98.name

    override val icon: IconUrl = "https://registry.walletconnect.com/v2/logo/md/dee547be-936a-4c92-9e3f-7a2350a62e00"

    override val url: WebsiteUrl = "https://coin98.com"

    override val mobileWallet: MobileWCWallet = coin98

    override val readyState: WalletReadyState
        get() {
            if (supportChains.contains(ConnectManager.chainType)) {
                return if (AppUtils.isAppInstalled(
                        ConnectManager.context, coin98.packageName
                    )
                ) WalletReadyState.Installed else WalletReadyState.NotDetected
            }
            return WalletReadyState.Unsupported
        }
}
```

## Give Feedback

You can join our [Discord](https://discord.gg/2y44qr6CR2).

# How to use WalletService

Before using WalletService, ensure successful integration of [Social Login](social-login.md) or [Wallet Connect](wallet-connect.md).

**Gradle Dependencies:**

```gradle
dependencies {
    // Required
    modules {
        module("org.bouncycastle:bcprov-jdk15to18") {
            replacedBy("org.bouncycastle:bcprov-jdk15on")
        }
    }
    // Wallet-service includes connect-auth-adapter and connect-wallet-connect-adapter by default,
    // so there is no need to include them again
    implementation("network.particle:wallet-service:$sdkVersion")
}
```

**Manifest Configuration:**

Add `android:fullBackupContent="@xml/pn_backup_rules"` in the `application` element.

```xml
<application
    android:fullBackupContent="@xml/pn_backup_rules">
    ...
</application>
```

**Initialization:**

```kotlin
/**
 * supportChains is optional, if not provided, all chains will be supported
 * if provided, only the chains in the list will be supported.
 * Only the main chain is required; if you want to support the devnet, 
 * you can call showTestNetworks() to display the devnet networks.
 */
val supportChains = mutableListOf<ChainInfo>(
    ChainInfo.BNBChain, ChainInfo.Ethereum
)
ParticleWallet.init(application, supportChains).apply {
    setShowTestNetworkSetting(true) // Default is false
    setShowManageWalletSetting(true) // Default is true
    hideMainBackIcon() // Hide the back icon in the main page
    setShowAppearanceSetting(true) // Default is true
    setShowLanguageSetting(true) // Default is true
    setSupportDappBrowser(true) // Default is true
}
```

**Setting Wallet Information:**

After successfully logging in with Social Login or Wallet Connect, use the following code to set wallet information:

```kotlin
val adapter = ParticleConnect.getAdapters().first { it.name == MobileWCWalletName.Particle.name }
adapter.connect(null, object : ConnectCallback {
    override fun onConnected(account: Account) {
        lifecycleScope.launch {
            ParticleWallet.setWallet(account.publicAddress, adapter)
        }
    }

    override fun onError(error: ConnectError) {
        // Handle connection error
    }
})
```

**Open Wallet:**

```kotlin
PNRouter.build(RouterPath.Wallet).navigation()
```

**Open Send Interface:**

```kotlin
PNRouter.build(RouterPath.TokenSend).navigation()
```

**Open Receive Interface:**

```kotlin
PNRouter.build(RouterPath.TokenReceive).navigation()
```

**Open Swap:**

```kotlin
PNRouter.navigatorSwap()
```


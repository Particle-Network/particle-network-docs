# Use AA

This guide will assist you in initiating Particle Connect and Particle AA Android SDK, enabling you to set up your environment, create a wallet with a social account, and send a User Operation. By the end of this guide, you will have a fundamental understanding of using the SDK and where to find more advanced use cases.

If you encounter any problems during the process, you can always refer to the Particle Community for prompt assistance!

**Prerequisites:**

Ensure you have integrated either Social Login, Wallet Connect, or WalletService before using AA Service.

**Gradle Dependencies:**

Add the following dependency to your `build.gradle` file:

```gradle
dependencies {
    //... 
    implementation("network.particle:aa-service:$sdkVersion")
}
```

**Initialization:**

Initialize AA Mode in your application, providing the necessary API keys:

```kotlin
ParticleNetwork.initAAMode(
    mapOf(
        Ethereum.id to "your apikey",
    ),
    aaService = BiconomyAAService
)
```

Choose the appropriate `aaService` from options like `BiconomyAAService`, `CyberConnectAAService`, or `SimpleAAService`.

**Enable AA:**

Activate AA Mode in your application:

```kotlin
ParticleNetwork.getAAService().enableAAMode()
```

**Batch Send Transactions:**

Construct and send multiple transactions in a batch using AA Service:

```kotlin
val trans1 = ParticleNetwork.evm.createTransaction(from, to, "0x$amount")!!.serialize()
val trans2 = ParticleNetwork.evm.createTransaction(from, to, "0x$amount")!!.serialize()

ParticleNetwork.getAAService().quickSendTransaction(
    listOf(trans1, trans2),
    FeeModeNative(),
    messageSigner = object : MessageSigner {
        override fun signMessage(
            message: String,
            callback: WebServiceCallback<SignOutput>,
            chainId: Long?
        ) {
            adapter.signMessage(from, message, object : SignCallback {
                override fun onSigned(signature: String) {
                    callback.success(SignOutput(signature))
                }

                override fun onError(error: ConnectError) {
                    callback.failure(ErrorInfo(error.message, error.code))
                }
            })
        }

        override fun eoaAddress(): String {
            return adapter.address
        }
    },
    object : WebServiceCallback<SignOutput> {
        override fun success(output: SignOutput) {
            ToastUtils.showLong(output.signature)
        }

        override fun failure(errMsg: ErrorInfo) {
            ToastUtils.showLong(errMsg.message)
        }
    }
)
```

Feel free to adjust the parameters based on your specific use case and handle exceptions appropriately.

By following these steps, you should be able to get started with Particle Connect and Particle AA Android SDK, sending transactions seamlessly. If you encounter any issues, refer to the Particle Community for assistance.






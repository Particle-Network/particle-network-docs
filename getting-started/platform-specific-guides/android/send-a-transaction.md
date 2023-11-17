# Send a Transaction

\
To send a transaction using the provided code, follow these steps:

1.  **Choose the Adapter:** Choose the appropriate adapter based on your scenario. The `adapterName` variable should be set to the desired adapter's name, such as "MetaMask," "Trust," "BitKeep," or any other supported wallet.

    ```kotlin
    // Choose your adapter based on the scenario (e.g., MetaMask, Trust, BitKeep)
    val adapterName = MobileWCWalletName.Particle.name 
    val adapter = ParticleConnect.getAdapters().first {
        it.name == adapterName
    }
    ```
2.  **Prepare Transaction Data:** Prepare the transaction data that you want to send. This data should be constructed according to the tutorial or documentation you are following. It can be in the form of a hex string for EVM (Ethereum Virtual Machine) transactions or a Base58 string for Solana transactions.

    ```kotlin
    val trans ="Replace this with the constructed transaction data (hex for EVM or Base58 for Solana)"
    ```
3.  **Get Wallet Address:** Obtain the wallet address after the user has logged in.

    ```kotlin
    val address = "Replace this with the wallet address obtained after login"
    ```
4.  **Sign and Send Transaction:** Use the obtained adapter to sign and send the transaction.

    ```
    adapter!!.signAndSendTransaction(address, trans, object : TransactionCallback {
        override fun onError(error: ConnectError) {
            // Handle transaction sending error
            // This block will be executed if there is an error sending the transaction
        }
    ​
        override fun onTransaction(transactionId: String?) {
            // Handle successful transaction
            // This block will be executed if the transaction is sent successfully
        }
    })
    ```

    * The `signAndSendTransaction` method takes the wallet address, transaction data, and a callback for handling errors and successful transactions.
    * If an error occurs during the transaction, the `onError` method will be called, and if the transaction is successful, the `onTransaction` method will be called with the transaction ID.

\

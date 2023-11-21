# Social Login + AA

This guide will help you get started with Particle Unity AA SDK by setting up your environment, creating a wallet with social account, and sending a User Operation from it. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

Before read the guide, you should read [last guide](social-login-+-embeded-wallet.md), understand how to connect, AA service should be used with Connect Service.

**Now, let's start with last guide example.**

### Initialization AA in Unity

You can add these code below Connect Service Initialization.

Set your AccountName, there are the options, `BICONOMY | SIMPLE | CYBERCONNECT`.

```csharp
ParticleAAInteraction.Init(AAAccountName.SIMPLE, AAVersionNumber.V1_0_0(), new Dictionary<int, string>());
ParticleAAInteraction.EnableAAMode();
```

After get account, you can get smart account address, then each time you send transaction, you should use smart account address to create transaction.

```csharp
public async void GetSmartAccount()
{
    try
    {
        var smartAccountResult = await EvmService.GetSmartAccount(new[]
        {
            new SmartAccountObject(AAAccountName.SIMPLE.ToString(), AAVersionNumber.V1_0_0().version,
                this._publicAddress)
        });
        this._smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
        Debug.Log($"_smartAccountAddress: {_smartAccountAddress}");
    }
    catch (Exception e)
    {
        Debug.LogError($"An error occurred: {e.Message}");
    }
}
```

Let's send a transaction in AA, it use the same method `signAndSendTransaction`.

Show how to send transaction in AA mode and use native token to pay gas fee.

The method `SignAndSendTransaction` last parameter is `FeeMode`, if enable AA, default value is native, which means pay native for gas fee.

You can also use gasless and pay with token. For more detail, explore [Account Abstraction](../../../developers/account-abstraction/unity.md).

```csharp
public async void SendTransaction()
{
    if (this._publicAddress == null || this._walletType == null || this._smartAccountAddress == null)
    {
        Debug.Log("did not connect any account");
        return;
    }

    try
    {
        // use smart account to create transaction
        var address = this._smartAccountAddress;
        const string to = "0x0000000000000000000000000000000000000000";
        var transaction =
            await EvmService.CreateTransaction(address, "0x", BigInteger.Parse("10000000000000000"), to);
        // call SignAndSendTransaction must use Account.publicAddress (Your EOA address) to send transaction
        var nativeResultData =
            await ParticleConnect.Instance.SignAndSendTransaction((WalletType)this._walletType, _publicAddress,
                transaction);
        if (nativeResultData.isSuccess)
        {
            Debug.Log(nativeResultData.data);
            var result = nativeResultData.data;
            Debug.Log($"result: {result}");
        }
        else
        {
            var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
            Debug.Log(errorData);
        }
    }
    catch (Exception e)
    {
        Debug.LogError($"An error occurred: {e.Message}");
    }
}
```

## AA support batch send multi transactions

Here we make two transactions, and call `ParticleAA.Instance.RpcGetFeeQuotes` to check if there transactions can send with pay native, gasless, or pay token.



<pre class="language-csharp"><code class="lang-csharp"><strong>public async void BatchSendTransactions()
</strong>{
    if (this._publicAddress == null || this._walletType == null || this._smartAccountAddress == null)
    {
        Debug.Log("did not connect any account");
        return;
    }

    tryc
    {
        // use smart account to create transaction
        var address = this._smartAccountAddress;
        const string to = "0x0000000000000000000000000000000000000000";
        // Now, create two transactions
        var transaction1 =
            await EvmService.CreateTransaction(address, "0x", BigInteger.Parse("10000000000000000"), to);
        var transaction2 =
            await EvmService.CreateTransaction(address, "0x", BigInteger.Parse("20000000000000000"), to);

        // Before send
        // check if pay native available
        var feeQuotesResultData = await ParticleAA.Instance.RpcGetFeeQuotes(this._publicAddress,
            new List&#x3C;string> { transaction1, transaction2 });

        var verifyingPaymasterNative = JObject.Parse(feeQuotesResultData.data)["verifyingPaymasterNative"];
        var nativeFeeQuote = verifyingPaymasterNative["feeQuote"];

        var fee = BigInteger.Parse((string)nativeFeeQuote["fee"]);
        var balance = BigInteger.Parse((string)nativeFeeQuote["balance"]);

        if (balance &#x3C; fee)
        {
            Debug.Log("native balance if not enough for gas fee");
        }
        else
        {
            Debug.Log("could send pay native");
        }
        // is balance >= fee, you can send with native
        // here is the code
        // var batchSendResultData =
        // await ParticleConnect.Instance.BatchSendTransactions((WalletType)this._walletType, _publicAddress,
        // new List&#x3C;string> { transaction1, transaction2 }, AAFeeMode.Native(feeQuotesResultData));

        // check if pay gasless available
        var verifyingPaymasterGasless = JObject.Parse(feeQuotesResultData.data)["verifyingPaymasterGasless"];

        if (verifyingPaymasterGasless == null)
        {
            Debug.Log("gasless is not available");
        }
        else
        {
            Debug.Log("could send gasless");
        }
        // is verifyingPaymasterGasless is not null, you can send gasless
        // here is the code
        // var batchSendResultData =
        // await ParticleConnect.Instance.BatchSendTransactions((WalletType)this._walletType, _publicAddress,
        // new List&#x3C;string> { transaction1, transaction2 }, AAFeeMode.Gasless(feeQuotesResultData));

        // Now, let's check if you can send pay token
        // check if pay token available
        var tokenPaymaster = JObject.Parse(feeQuotesResultData.data)["tokenPaymaster"];
        Debug.Log($"TokenPaymaster = {tokenPaymaster}");

        if (tokenPaymaster == null)
        {
            Debug.Log("pay token is not available");
            return;
        }
        else
        {
        }

        JArray feeQuotes = (JArray)(tokenPaymaster["feeQuotes"]);

        var overFeeQuotes = feeQuotes
            .Where(jt =>
            {
                var fee = BigInteger.Parse(jt["fee"].Value&#x3C;string>());
                var balance = BigInteger.Parse((string)jt["balance"].Value&#x3C;string>());

                return balance >= fee;
            })
            .ToList();


        if (overFeeQuotes.Count == 0)
        {
            Debug.Log("no valid token for gas fee");
            return;
        }

        Debug.Log("prepare send pay token");

        // select the first feeQuote
        var feeQuote = overFeeQuotes[0];
        var tokenPaymasterAddress =
            JObject.Parse(feeQuotesResultData.data)["tokenPaymaster"]["tokenPaymasterAddress"].Value&#x3C;string>();

        // call BatchSendTransactions must use Account.publicAddress (Your EOA address) to send transaction
        var batchSendResultData =
            await ParticleConnect.Instance.BatchSendTransactions((WalletType)this._walletType, _publicAddress,
                new List&#x3C;string> { transaction1, transaction2 }, AAFeeMode.Token(feeQuote, tokenPaymasterAddress));
        if (batchSendResultData.isSuccess)
        {
            var result = batchSendResultData.data;
            Debug.Log($"result: {result}");
        }
        else
        {
            var errorData = JsonConvert.DeserializeObject&#x3C;NativeErrorData>(batchSendResultData.data);
            Debug.Log(errorData);
        }
    }
    catch (Exception e)
    {
        Debug.LogError($"An error occurred: {e.Message}");
    }
}
</code></pre>

## Dive Deeper

In this guide, we learned how to use the Particle AA Flutter SDK for getting smart account, sending transactions. If you want to learn more about different use cases, check out the [Particle A](../../../developers/connect-service/sdks/flutter.md)[A](../../../developers/account-abstraction/flutter.md) page.

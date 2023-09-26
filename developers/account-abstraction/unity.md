# Unity

Before using, please review [the prerequisites](../../getting-started/platform-specific-guides/unity/unity-sdk-prerequisites.md), All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### Initialize the SDK

1. **Before using the sdk you have to call init(Required)**&#x20;

```csharp
var dappApiKeys = new Dictionary<int, string>
{
    { 1, "YOUR_DAPP_API_KEY" },
    { 5, "YOUR_DAPP_API_KEY" },
    { 137, "YOUR_DAPP_API_KEY" },
    { 80001, "YOUR_DAPP_API_KEY" }
};

ParticleAAInteraction.Init(dappApiKeys);
```

**2. Drag prefab to your scene**

Drag the ParticleAA.prefab to your first scene(Required)&#x20;

### Is support chainInfo

check if support the chainInfo

```csharp
var chainInfo = ChainInfo.Ethereum;
var result = ParticleAAInteraction.IsSupportChainInfo(chainInfo);
```

### Is deploy AA wallet

```csharp
try
{
    var eoaAddress = ParticleAuthServiceInteraction.GetAddress();
    var nativeResultData = await ParticleAA.Instance.IsDepoly(eoaAddress);

    Debug.Log("result" + nativeResultData.data);
    if (nativeResultData.isSuccess)
    {
        var isDeploy = Convert.ToBoolean(nativeResultData.data);
        Debug.Log($"isDeploy {isDeploy}");
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
```

### Is AA mode enable

```csharp
var result = ParticleAAInteraction.IsAAModeEnable();
```

### Enable AA mode

```csharp
ParticleAAInteraction.EnableAAMode();
```

### Disable AA mode

```csharp
ParticleAAInteraction.DisableAAMode();
```

### Rpc get fee quotes

Always used with send Transaction, pick one feeQuote then send transaction in custom feeMode.

```csharp
try
{
    var eoaAddress = ParticleAuthServiceInteraction.GetAddress();
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { eoaAddress });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransacion(smartAccountAddress);
    var nativeResultData =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, new List<string> { transaction });

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
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
```

### SignAndSendTransaction with Auth Service

There are 3 ways to pay gas fees, native, token and gasless.

The following code describes how to pay using native

```csharp
try
{
    var eoaAddress = ParticleAuthServiceInteraction.GetAddress();
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { eoaAddress });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransacion(smartAccountAddress);

    // select native to pay gas fee, you need check if enough native for gas fee
    var feeQuotesResult =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, new List<string> { transaction });

    var verifyingPaymasterNative = JObject.Parse(feeQuotesResult.data)["verifyingPaymasterNative"];
    var feeQuote = verifyingPaymasterNative["feeQuote"];

    var fee = BigInteger.Parse((string)feeQuote["fee"]);
    var balance = BigInteger.Parse((string)feeQuote["balance"]);

    if (balance < fee)
    {
        Debug.Log("native balance if not enough for gas fee");
        return;
    }

    // pass result from rpcGetFeeQuotes to send pay with native
    var nativeResultData =
        await ParticleAuthService.Instance.SignAndSendTransaction(transaction,
            AAFeeMode.Native(JObject.Parse(feeQuotesResult.data)));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
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
```

### SignAndSendTransaction with Connect Service

There are 3 ways to pay gas fees, native, token and gasless.

The following code describes how to pay using token

```csharp
 try
{
    // confirm your eos address and walletType
    var walletType = WalletType.MetaMask
    var eoaAddress = "your eoa address";
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { eoaAddress });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransactionWithConnect(smartAccountAddress);

    // select token to pay gas fee, you need check if enough token for gas fee
    var feeQuotesResult =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, new List<string> { transaction });

    JArray feeQuotes = (JArray)(JObject.Parse(feeQuotesResult.data)["tokenPaymaster"]["feeQuotes"]);
    
    // filter token balance > fee 
    var overFeeQuotes = feeQuotes
        .Where(jt =>
        {
            var fee = BigInteger.Parse(jt["fee"].Value<string>());
            var balance = BigInteger.Parse((string)jt["balance"].Value<string>());

            return balance >= fee;
        })
        .ToList();


    if (overFeeQuotes.Count == 0)
    {
        Debug.Log("no valid token fro gas fee");
        return;
    }

    // select the first feeQuote
    var feeQuote = overFeeQuotes[0];
    var tokenPaymasterAddress =
        JObject.Parse(feeQuotesResult.data)["tokenPaymaster"]["tokenPaymasterAddress"].Value<string>();

    var nativeResultData =
        await ParticleConnect.Instance.SignAndSendTransaction(WalletType.MetaMask, eoaAddress, transaction,
            AAFeeMode.Token(feeQuote, tokenPaymasterAddress));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
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
```

### Batch send transaction with Auth Service

There are 3 ways to pay gas fees, native, token and gasless.

The following code describes how to pay using gasless

```csharp
try
{
    var eoaAddress = ParticleAuthServiceInteraction.GetAddress();
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { eoaAddress });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransacion(smartAccountAddress);

    var transactions = new List<string> { transaction, transaction };

    // select gasless to pay gas fee, check is gasless available
    var feeQuotesResult =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, new List<string> { transaction });

    var verifyingPaymasterGasless = JObject.Parse(feeQuotesResult.data)["verifyingPaymasterGasless"];

    if (verifyingPaymasterGasless.Type == JTokenType.Null)
    {
        print("gasless is not available");
        return;
    }

    // pass result from rpcGetFeeQuotes to send gasless
    var nativeResultData =
        await ParticleAuthService.Instance.BatchSendTransactions(transactions,
            AAFeeMode.Gasless(JObject.Parse(feeQuotesResult.data)));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
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
```

### Batch send transaction with Connect Service

There are 3 ways to pay gas fees, native, token and gasless.

The following code describes how to pay using native.

```csharp
try
{
    // confirm your eos address and walletType
    var walletType = WalletType.MetaMask
    var eoaAddress = "your eoa address";
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { eoaAddress });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransacion(smartAccountAddress);

    var transactions = new List<string> { transaction, transaction };
    // check if enough native for gas fee
    var feeQuotesResult =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, transactions);

    var verifyingPaymasterNative = JObject.Parse(feeQuotesResult.data)["verifyingPaymasterNative"];
    var feeQuote = verifyingPaymasterNative["feeQuote"];

    var fee = BigInteger.Parse((string)feeQuote["fee"]);
    var balance = BigInteger.Parse((string)feeQuote["balance"]);

    if (balance < fee)
    {
        Debug.Log("native balance if not enough for gas fee");
        return;
    }

    // pass result from rpcGetFeeQuotes to send pay with native
    var nativeResultData = await ParticleConnect.Instance.BatchSendTransactions(walletType,
        eoaAddress,
        transactions, AAFeeMode.Native(JObject.Parse(feeQuotesResult.data)));
    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
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
```

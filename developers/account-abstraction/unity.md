# Unity

Before using, please review [the prerequisites](../../getting-started/platform-specific-guides/unity/unity-sdk-prerequisites.md), All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### Initialize the SDK

**Drag prefab to your scene**

Drag the ParticleAA.prefab to your first scene(Required)

**Before using the SDK, you have to call init(Required)**

(Optional) The biconomyApiKeys comes from Biconomy, visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

```csharp
var biconomyApiKeys = new Dictionary<int, string>
{
    { 1, "your ethereum mainnet key" },
    { 5, "your ethereum goerli key" },
    { 137, "your polygon mainnet key" },
    { 80001, "your polygon mumbai key" }
};

// AccountName support BICONOMY_V1 | BICONOMY_V2 | SIMPLE | CYBERCONNECT
ParticleAAInteraction.Init(AAAccountName.BICONOMY_V1(), biconomyApiKeys);
```

**2. Drag prefab to your scene**

Drag the ParticleAA.prefab to your first scene(Required)

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

## Get smart account address

```csharp
public async void GetSmartAccount()
{
    try
    {
        var eoaAddress = "your EOA address";
        var smartAccountResult = await EvmService.GetSmartAccount(new[]
        {
            new SmartAccountObject(AAAccountName.BICONOMY_V1(), eoaAddress)
        });
        var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    }
    catch (Exception e)
    {
        Debug.LogError($"An error occurred: {e.Message}");
    }
}
```

## Send transaction

You should use ParticleAuth | ParticleConnect | ParticleAuthCore to send transaction, all of them have a function called `signAndSendTransaction` or `evmSendTransaction` , a parameter called `feeMode` is used with AA service.

`feeMode` support native, gasless and token, just as its name implies, it tells how to pay gas fee.

### SignAndSendTransaction with Connect Service

Show how to send transaction with particle-connect, use native token to pay gas fee.

```dart
try
{
    var eoaAddress = "your EOA address";
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { new SmartAccountObject(AAAccountName.BICONOMY_V1(), eoaAddress) });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransactionWithConnect(smartAccountAddress);

    // check if enough native for gas fee
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
        await ParticleConnect.Instance.SignAndSendTransaction(WalletType.MetaMask, eoaAddress, transaction,
            AAFeeMode.Native(JObject.Parse(feeQuotesResult.data)));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
        Debug.Log($"signature {nativeResultData.data}");
    }
    else
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
catch (Exception e)
{
    Debug.LogError($"An error occurred: {e.Message}");
}
```

Show  how to send transaction with particle-connect, gasless.

```dart
try
{
    var eoaAddress = "0x498c9b8379E2e16953a7b1FF94ea11893d09A3Ed";
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { new SmartAccountObject(AAAccountName.BICONOMY_V1(), eoaAddress) });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransactionWithConnect(smartAccountAddress);

    // check if gasless available
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
        await ParticleConnect.Instance.SignAndSendTransaction(WalletType.MetaMask, eoaAddress, transaction,
            AAFeeMode.Gasless(JObject.Parse(feeQuotesResult.data)));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
        Debug.Log($"signature {nativeResultData.data}");
    }
    else
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
catch (Exception e)
{
    Debug.LogError($"An error occurred: {e.Message}");
}
```

Show  how to send transaction with particle-connect, use token to pay gas fee.

```dart
try
{
    var eoaAddress = "";
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { new SmartAccountObject(AAAccountName.BICONOMY_V1(), eoaAddress) });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransactionWithConnect(smartAccountAddress);

    // select a feeQuote
    var feeQuotesResult =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, new List<string> { transaction });

    JArray feeQuotes = (JArray)(JObject.Parse(feeQuotesResult.data)["tokenPaymaster"]["feeQuotes"]);

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

    Debug.Log($"feeQuote {feeQuote}");
    Debug.Log($"tokenPaymasterAddress {tokenPaymasterAddress}");

    var nativeResultData =
        await ParticleConnect.Instance.SignAndSendTransaction(WalletType.MetaMask, eoaAddress, transaction,
            AAFeeMode.Token(feeQuote, tokenPaymasterAddress));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
        Debug.Log($"signature {nativeResultData.data}");
    }
    else
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
catch (Exception e)
{
    Debug.LogError($"An error occurred: {e.Message}");
}
```

{% hint style="info" %}
If you are using ParticleAuth to send transaction, the difference is which to call `signAndSendTransaction` method.

If you are uing ParticleAuthCore to send transaction, the difference is call evmSendTransaction method.
{% endhint %}

### Rpc get fee quotes

Use with send transaction, fill parameter `feeMode`.

```dart
try
{
    var eoaAddress = ParticleAuthServiceInteraction.GetAddress();
    // get your smart account by account name and version.
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { new SmartAccountObject(AAAccountName.BICONOMY_V1(), eoaAddress) });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransacion(smartAccountAddress);
    var nativeResultData =
        await ParticleAA.Instance.RpcGetFeeQuotes(eoaAddress, new List<string> { transaction });

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
        Debug.Log(nativeResultData.data);
    }
    else
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
catch (Exception e)
{
    Debug.LogError($"An error occurred: {e.Message}");
}
```

### Batch send transactions

Merge multiple transactions and send them together

```dart
try
{
    var eoaAddress = "0x498c9b8379E2e16953a7bEvmService1FF94ea11893d09A3Ed";
    var smartAccountResult = await EvmService.GetSmartAccount(new[] { new SmartAccountObject(AAAccountName.BICONOMY_V1(), eoaAddress) });
    var smartAccountAddress = (string)JObject.Parse(smartAccountResult)["result"][0]["smartAccountAddress"];
    var transaction = await TransactionHelper.GetEVMTransactionWithConnect(smartAccountAddress);

    // check if enough native for gas fee
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
        await ParticleConnect.Instance.SignAndSendTransaction(WalletType.MetaMask, eoaAddress, transaction,
            AAFeeMode.Native(JObject.Parse(feeQuotesResult.data)));

    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
        Debug.Log($"signature {nativeResultData.data}");
    }
    else
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
catch (Exception e)
{
    Debug.LogError($"An error occurred: {e.Message}");
}
```

{% hint style="info" %}
If you are using ParticleAuth/ParticleAuthCore to send transaction, the difference is which to call `batchSendTransactions` method.
{% endhint %}

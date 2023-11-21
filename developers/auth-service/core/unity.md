# Unity

Before using, please review [the prerequisites](../../../getting-started/platform-specific-guides/unity/unity-sdk-prerequisites.md), All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### Initialize the SDK

1. **Before using the sdk you have to call init(Required)**

```csharp
ParticleNetwork.Init(_chainInfo);
ParticleAuthCoreInteraction.Init();
```

2. &#x20;**Drag prefab to your scene**

Drag the ParticleAuthCore.prefab to your first scene(Required)

### Connect JWT

```csharp
public async void Connect()
{
    try
    {
        var jwt = "";
        var nativeResultData = await ParticleAuthCore.Instance.Connect(jwt);

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
}
```

### Disconnect

```csharp
public async void Disconnect()
{
    try
    {
        var nativeResultData = await ParticleAuthCore.Instance.Disconnect();
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
}
```

### IsConnected

```csharp
public async void IsConnected()
{
    try
    {
        var nativeResultData = await ParticleAuthCore.Instance.IsConnected();
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
}
```

### Swicth chain

```csharp
public async void SwitchChain()
{
    try
    {
        var nativeResultData = await ParticleAuthCore.Instance.SwitchChain(this._chainInfo);
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
}
```

### Change master password

```csharp
public async void ChangeMasterPassword()
{
    try
    {
        var nativeResultData =
            await ParticleAuthCore.Instance.ChangeMasterPassword();
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
}
```

### Has set payment password

```csharp
public void HasPaymentPassword()
{
    var result = ParticleAuthCoreInteraction.HasPaymentPassword();
    Debug.Log($"has master password {result}");
}
```

### Has set master password

```csharp
public void HasMasterPassword()
{
    var result = ParticleAuthCoreInteraction.HasMasterPassword();
    Debug.Log($"has master password {result}");
}
```

### Get user info

```csharp
public void GetUserInfo()
{
    var userInfo = ParticleAuthCoreInteraction.GetUserInfo();
    Debug.Log($"get user info {userInfo}");
}
```

### Open web wallet

```csharp
public void OpenWebWallet()
{
    var jsonString = "";
    ParticleAuthCoreInteraction.OpenWebWallet(jsonString);
}
```

### Open account and security

```csharp
public async void OpenAccountAndSecurity()
{
    try
    {
        var nativeResultData =
            await ParticleAuthCore.Instance.OpenAccountAndSecurity();

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
}
```

### EVM Sign

### Get address

```csharp
public void EvmGetAddress()
{
    var address = ParticleAuthCoreInteraction.EvmGetAddress();
    Debug.Log($"evm address: {address}");
}
```

### Personal sign and personal sign unique

```csharp
public async void EvmPersonalSign()
{
    try
    {
        var message = "Hello world";
        var nativeResultData =
            await ParticleAuthCore.Instance.EvmPersonalSign(message);
        // var nativeResultData =
            // await ParticleAuthCore.Instance.EvmPersonalSignUnique(message);

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
}
```

### Sign typed data and sign typed data unique

```csharp
public async void EvmSignTypedData()
{
    try
    {
        var txtAsset = Resources.Load<TextAsset>("TypedDataV4");
        string typedData = txtAsset.text;

        var chainId = ParticleNetwork.GetChainInfo().Id;
        JObject json = JObject.Parse(typedData);
        json["domain"]["chainId"] = chainId;
        string newTypedData = json.ToString();

        var nativeResultData =
            await ParticleAuthCore.Instance.EvmSignTypedData(newTypedData);
        // var nativeResultData =
            // await ParticleAuthCore.Instance.EvmSignTypedDataUnique(newTypedData);
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
}
```

### Send transaction

```csharp
public async void EvmSendTransaction()
{
    try
    {
        var address = ParticleAuthCoreInteraction.EvmGetAddress();
        var transaction = await TransactionHelper.GetEVMTransacion(address);

        var nativeResultData =
            await ParticleAuthCore.Instance.EvmSendTransaction(transaction);

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
}
```

### Solana Sign

### Get address

```csharp
public void SolanaGetAddress()
{
    var address = ParticleAuthCoreInteraction.SolanaGetAddress();
    Debug.LogError($"solana address: {address}");
}
```

### Sign message

```csharp
public async void SolanaSignMessage()
{
    try
    {
        var message = "Hello Particle!";

        var nativeResultData =
            await ParticleAuthCore.Instance.SolanaSignMessage(message);

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
}
```

### Sign transaction

```csharp
public async void SolanaSignTransaction()
{
    try
    {
        var address = ParticleAuthCoreInteraction.SolanaGetAddress();
        var transaction = await TransactionHelper.GetSolanaTransacion(address);

        var nativeResultData =
            await ParticleAuthCore.Instance.SolanaSignTransaction(transaction);

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
}
```

### Sign all transactions

```csharp
public async void SolanaSignAllTransactions()
{
    try
    {
        var address = ParticleAuthCoreInteraction.SolanaGetAddress();
        var transaction1 = await TransactionHelper.GetSolanaTransacion(address);
        var transaction2 = await TransactionHelper.GetSolanaTransacion(address);

        var nativeResultData =
            await ParticleAuthCore.Instance.SolanaSignAllTransactions(new[] { transaction1, transaction2 });

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
}
```

### Sign and send transaction

```csharp
public async void SolanaSignAndSendTransaction()
{
    try
    {
        var address = ParticleAuthCoreInteraction.SolanaGetAddress();
        var transaction = await TransactionHelper.GetSolanaTransacion(address);

        var nativeResultData =
            await ParticleAuthCore.Instance.SolanaSignAndSendTransaction(transaction);

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
}
```

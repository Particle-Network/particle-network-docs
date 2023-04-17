---
description: Power up your Unity games with Particle Network SDKs.
---

# Unity

Before using, please review [the prerequisites](../../../getting-started/platform-specific-guides/unity/unity-sdk-prerequisites.md), All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### &#x20;Initialize the SDK

1. **Before using the sdk you have to call init(Required)**&#x20;

```
ParticleNetwork.Init(new BscChain(BscChainId.Mainnet));
```

**2. Drag prefab to your scene**

Drag the ParticleAuthService.prefab to your first scene(Required)&#x20;

### Login

```csharp
public async void Login()
{
    // login email
    var nativeResultData = await ParticleAuthService.Instance.Login(LoginType.PHONE, null, SupportAuthType.ALL);
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
```

### Logout

```csharp
public async void Logout()
{
    var nativeResultData = await ParticleAuthService.Instance.Logout();
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
```

### Fast logout

```csharp
public async void FastLogout()
{
    var nativeResultData = await ParticleAuthService.Instance.FastLogout();
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
```

### IsLogin

Check is user login locally.

```csharp
ParticleAuthServiceInteraction.IsLogin();
```

### IsLoginAsync

Check is user login from server, recommended.

```csharp
var nativeResultData = await ParticleAuthService.Instance.IsLoginAsync();
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
```

### GetAddress

```
ParticleAuthServiceInteraction.GetAddress();
```

### SignMessage

```csharp
public async void SignMessage()
{
    var message = "Hello world";
    var nativeResultData =
        await ParticleAuthService.Instance.SignMessage(message);
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
```

### SignTransaction

```csharp
public async void SignTransaction()
{
    var transaction = "your transaction";
    var nativeResultData =
        await ParticleAuthService.Instance.SignTransaction(transaction);
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
```

### SignAllTransactions

```csharp
public async void SignAllTransactions()
{
    var transaction1 = "transaction1";
    var transaction2 ="transaction2";
    Debug.Log("transaction1 = " + transaction1);
    Debug.Log("transaction2 = " + transaction2);
    string[] transactions = new[] { transaction1, transaction2 };
    var nativeResultData =
        await ParticleAuthService.Instance.SignAllTransactions(transactions);
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
}cc
```

### SignAndSendTransaction

```csharp
public async void SignAndSendTransaction()
{
    var transaction = "transaction";
    var nativeResultData =
        await ParticleAuthService.Instance.SignAndSendTransaction(transaction);
    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
    }
    else
    {
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}c
```

### SignTypedData

```csharp
public async void SignTypedData()
{
    string typedData = "your typedata";
    var nativeResultData =
        await ParticleAuthService.Instance.SignTypedData(typedData,
            SignTypedDataVersion.V1);
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
```

### SetChainInfoASync

```csharp
public async void SetChainInfoASync()
{
    // call this method to change chain info and auto create public address if that is not existed.
    var nativeResultData =
        await ParticleAuthService.Instance.SetChainInfoAsync(_chainInfo);
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
```

### SetChainInfoSync

```csharp
public void SetChainInfoSync()
{
    // call this method to change chain info. You
    ParticleNetwork.SetChainInfo(_chainInfo);
}
```

### SetiOSModalPresentStyle

```csharp
public void SetiOSModalStyle()
{
    // call this method to set embedded safari fullscreen or formsheet, 
    // default value is formsheet 
    ParticleAuthServiceInteraction.SetiOSModalPresentStyle(iOSModalPresentStyle.FullScreen);
}
```

### SetiOSMediumScreen

```csharp
public void SetiOSMediumScreen()
{
    // support from iOS 15,
    // call this method to set embedded safari medium screen,
    // default value is false,
    // if you want to use medium screen, 
    // make sure not call SetiOSModalPresentStyle with FullScreen,
    // FullScreen conflicts with medium screen.
    ParticleAuthServiceInteraction.SetiOSMediumScreen(true);
}
```


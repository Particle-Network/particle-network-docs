# Social Login + Embeded Wallet

This guide will help you get started with Particle Unity SDK by setting up your environment, creating a wallet with social account, and sending a EVM transaction. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

## Installation

Before read the guide, you should read [Prerequisites](unity-sdk-prerequisites.md), understand how to install Particle Unity SDK.

**Now, let's start with unity.**

## Initialization in Unity

Set your chain info and wallet connect project id, wallet info.

If you don't want to use wallet connect, you should set `walletConnectProjectId` to a non empty string, like "x".

```csharp
void Start()
{
    ParticleNetwork.Init(ChainInfo.PolygonMumbai);
    var walletConnectProjectId = "Your wallet connect project id";
    ParticleConnectInteraction.Init(ChainInfo.PolygonMumbai,
        new DAppMetaData(walletConnectProjectId, "Guide Wallet", "Guide Icon", "Guide Url", "Guide description"));
    ParticleWalletGUI.ParticleWalletConnectInitialize(new WalletMetaData("Guide Wallet", "Guide Icon", "Guide Url", "Guide description",
        walletConnectProjectId));
}
```

## Triggering the login

Trigger login flow with email, phone number, google, facebook, twitter, JWT, etc.

You need to save the returned `Account` object for convenient use next time.

Here we extract the public address from returned data, and save it in memory.

```csharp
public async void ConnectGoogle()
{
    try
    {
        var config = new ConnectConfig(LoginType.GOOGLE, "", SupportAuthType.ALL, socialLoginPrompt:SocialLoginPrompt.SelectAccount);
        var nativeResultData =
            await ParticleConnect.Instance.Connect(WalletType.Particle, config);
        if (nativeResultData.isSuccess)
        {
            Debug.Log(nativeResultData.data);
            var accountJson = JObject.Parse(nativeResultData.data);

            this._publicAddress = accountJson["publicAddress"].ToString();
            this._walletType = WalletType.Particle;
            Debug.Log($"publicAddress: {_publicAddress}");
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

{% hint style="info" %}
WalletType.Particle is used for social login, `ConnectConfig` requires `LoginType` and other parameters.

If login type is JWT, you must pass your JWT as the second parameter.

```csharp
var config = new ConnectConfig(LoginType.JWT, "Your JWT", SupportAuthType.ALL);
```
{% endhint %}

We also support connect with other wallets, supported by wallet connect, and don't forget to set wallet connect project id when Initializing.

```csharp
public async void ConnectMetamask()
{
    try
    {
        var nativeResultData =
            await ParticleConnect.Instance.Connect(WalletType.MetaMask);
        if (nativeResultData.isSuccess)
        {
            Debug.Log(nativeResultData.data);
            var accountJson = JObject.Parse(nativeResultData.data);

            this._publicAddress = accountJson["publicAddress"].ToString();
            this._walletType = WalletType.MetaMask;
            Debug.Log($"publicAddress: {_publicAddress}");
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

## Triggering the sign

After connect, if you want to authenticate by signature call `SignMessage`.

```csharp
public async void SignMessage()
{
    if (this._publicAddress == null || this._walletType == null)
    {
        Debug.Log("did not connect any account");
        return;
    }

    try
    {
        var nativeResultData =
            await ParticleConnect.Instance.SignMessage((WalletType)this._walletType, _publicAddress,
                "Hello Particle");
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

If you want to send a EVM transaction, call `SignAndSendTransaction`.

You can create a transaction by `EvmService.CreateTransaction` method.

```csharp
public async void SendTransaction()
{
    if (this._publicAddress == null || this._walletType == null)
    {
        Debug.Log("did not connect any account");
        return;
    }

    try
    {
        var address = this._publicAddress;
        const string to = "0x0000000000000000000000000000000000000000";
        var transaction =
            await EvmService.CreateTransaction(address, "0x", BigInteger.Parse("10000000000000000"), to);
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

{% hint style="info" %}
`SignTypeData` does the same way with `SignMessage`
{% endhint %}

## Log the user out

Use the `disconnect` function of ParticleConnect to trigger the logout flow.&#x20;

```csharp
public async void Disconnect()
{
    if (this._publicAddress == null || this._walletType == null)
    {
        Debug.Log("did not connect any account");
        return;
    }

    this._publicAddress = null;
    this._walletType = null;
    
    try
    {
        var nativeResultData =
            await ParticleConnect.Instance.Disconnect(WalletType.Particle, this._publicAddress);
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
}
```

## Dive Deeper

In this guide, we learned how to use the Particle Unity SDK for social login, sending transactions, and signing message. If you want to learn more about different use cases, check out the [Particle Unity](../../../developers/account-abstraction/unity.md) page.

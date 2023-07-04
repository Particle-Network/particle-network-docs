---
description: SSO for Web3
---

# Unity

Before using, please review [the prerequisites](../../../getting-started/platform-specific-guides/unity/unity-sdk-prerequisites.md)

All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

## Github Demo/Packages

[https://github.com/Particle-Network/particle-unity](https://github.com/Particle-Network/particle-unity)

## Summary

![Particle Connect](https://static.particle.network/docs-images/particle-connect.jpeg)

<mark style="color:red;">**It is strongly discouraged to use private key or mnemonic import/generate function, if you use it, you need to secure the data yourself, Particle's SDK has no relationship with the imported/generated mnemonic or private key.**</mark>

## Quick Start

### Init

```csharp
// Get your wallet connect project id from https://walletconnect.com/
// it is required by wallet connect v2.
var metadata = new DAppMetaData(TestConfig.walletConnectProjectId,"Particle Connect",
                "https://connect.particle.network/icons/512.png",
                "https://connect.particle.network",
                "");
ParticleNetwork.Init(_chainInfo);
ParticleConnectInteraction.Init(_chainInfo, metadata);
List<ChainInfo> chainInfos = new List<ChainInfo>{new EthereumChain(EthereumChainId.Mainnet), new EthereumChain(EthereumChainId.Goerli), new EthereumChain(EthereumChainId.Sepolia)};
ParticleConnectInteraction.SetWalletConnectV2SupportChainInfos(chainInfos.ToArray());
```

{% hint style="info" %}
## Migrating to WalletConnect v2

Starting from version 0.14.0, WalletConnectV2 is supported.

```csharp
// set wallet connect support chain infos after init.
List<ChainInfo> chainInfos = new List<ChainInfo>{new EthereumChain(EthereumChainId.Mainnet), new EthereumChain(EthereumChainId.Goerli), new EthereumChain(EthereumChainId.Sepolia)};
ParticleConnectInteraction.SetWalletConnectV2SupportChainInfos(chainInfos.ToArray());
```
{% endhint %}

### Set chain info Sync

use before connect

```csharp
ParticleConnectInteraction.SetChainInfo(_chainInfo);
```

### Set chain info async

use after connect, because ParticleAuthService support both solana and evm, if switch to solana from evm, Auth Service will create a evm address if the user doesn't has a evm address.

```csharp
var nativeResultData = await ParticleConnect.Instance.SetChainInfoAsync(_chainInfo);
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
```

### Connect Wallet

<pre class="language-csharp"><code class="lang-csharp"><strong>var nativeResultData = await ParticleConnect.Instance.Connect(this._walletType);
</strong>if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject&#x3C;NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}

// if conenct with particle, you could pass a config parameter,
// like ParticleAuthService.login
// contains login type, account, support auth types and login from mode.
ConnectConfig config = new ConnectConfig(LoginType.PHONE, null, SupportAuthType.ALL, false);
var nativeResultData = await ParticleConnect.Instance.Connect(WalletType.Particle, config);
Debug.Log(nativeResultData.data);

if (nativeResultData.isSuccess)
{
    ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
    Debug.Log(nativeResultData.data);
}
else
{
    ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
    var errorData = JsonConvert.DeserializeObject&#x3C;NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
</code></pre>

### Disconnect Wallet

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;
var nativeResultData = await ParticleConnect.Instance.Disconnect(this._walletType, publicAddress);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Is Connected

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;
var isConnect = ParticleConnectInteraction.IsConnected(this._walletType, publicAddress);
```

### Sign And Send Transaction

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;
string transaction = await GetEVMTransacion();

var nativeResultData =
    await ParticleConnect.Instance.SignAndSendTransaction(this._walletType, publicAddress, transaction);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Sign Transaction

```csharp
string publicAddress = TestAccount.Solana.PublicAddress;
var transaction = await GetSolanaTransacion();
var nativeResultData =
    await ParticleConnect.Instance.SignTransaction(this._walletType, publicAddress, transaction);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Sign All Transactions

```csharp
var publicAddress = TestAccount.Solana.PublicAddress;
var transaction1 = await GetSolanaTransacion();
var transaction2 = await GetSolanaTransacion();
string[] transactions = new[] { transaction1, transaction2 };
var nativeResultData =
    await ParticleConnect.Instance.SignAllTransactions(this._walletType, publicAddress, transactions);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Sign Message

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;
var message = "Hello world";
var nativeResultData =
    await ParticleConnect.Instance.SignMessage(this._walletType, publicAddress, message);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Sign Typed Data

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;

var txtAsset = Resources.Load<TextAsset>("TypedDataV4");
string typedData = txtAsset.text;

var nativeResultData =
    await ParticleConnect.Instance.SignTypedData(this._walletType, publicAddress, typedData);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Login (Sign-In With Ethereum or Solana)

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;
var domain = "login.xyz";
var uri = "https://login.xyz/demo#login";
var nativeResultData =
    await ParticleConnect.Instance.Login(this._walletType, publicAddress, domain, uri);

if (nativeResultData.isSuccess)
{
    var message = (string)JObject.Parse(nativeResultData.data)["message"];
    var signature = (string)JObject.Parse(nativeResultData.data)["signature"];

    // get login message
    this.loginSourceMessage = message;
    // get signature
    this.loginSignature = signature;

    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Verify Locally

verify login return message and signature.

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;

var message = this.loginSourceMessage;
var signature = this.loginSignature;
var nativeResultData =
    await ParticleConnect.Instance.Verify(this._walletType, publicAddress, message, signature);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Import Private Key

```csharp
string privateKey = TestAccount.EVM.PrivateKey;

var nativeResultData =
    await ParticleConnect.Instance.ImportWalletFromPrivateKey(this._walletType, privateKey);
    
if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Import Mnemonic

```csharp
string mnemonic = TestAccount.EVM.Mnemonic;

var nativeResultData =
    await ParticleConnect.Instance.ImportWalletFromMnemonic(this._walletType, mnemonic);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Export Private Key

```csharp
string publicAddress = TestAccount.EVM.PublicAddress;

var nativeResultData =
    await ParticleConnect.Instance.ExportWalletPrivateKey(this._walletType, publicAddress);

if (nativeResultData.isSuccess)
{
    Debug.Log(nativeResultData.data);
}
else
{
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Add Ethereum Chain

use to add ethereum chain to wallet which comes from wallet connect, bridge underlayer wallet\_addEthereumChain, only support MetaMask.

```csharp
// add ethereum chain example
// define chain id, chain name, native currency, block explorer url and rpc url.
var nativeResultData =
    await ParticleConnect.Instance.AddEthereumChain(_walletType, publicAddress, (int)PolygonChainId.Mumbai, 
        "Polygon Mumbai", 
        new NativeCurrency("Polygon Mumbai", "Matic", 18), 
        "https://matic-mumbai.chainstacklabs.com", "https://mumbai.polygonscan.com");
        
// define chain id, other parameters will auto configure in SDK.
// var nativeResultData =
    // await ParticleConnect.Instance.AddEthereumChain(_walletType, publicAddress, (int)PolygonChainId.Mumbai);
Debug.Log(nativeResultData.data);

if (nativeResultData.isSuccess)
{
    Tips.Instance.Show($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
    Debug.Log("AddEthereumChain:" + nativeResultData.data);
    Tips.Instance.Show(nativeResultData.data);
}
else
{
    Tips.Instance.Show($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

### Switch Ethereum Chain

use to switch ethereum chain with wallet which comes from wallet connect, bridge underlayer wallet\_switchEthereumChain, only support MetaMask.

```csharp
var nativeResultData =
        await ParticleConnect.Instance.SwitchEthereumChain(_walletType, publicAddress, (int)EthereumChainId.Goerli);
Debug.Log(nativeResultData.data);

if (nativeResultData.isSuccess)
{
    Tips.Instance.Show($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
    Debug.Log("SwitchEthereumChain:" + nativeResultData.data);
    Tips.Instance.Show(nativeResultData.data);
}
else
{
    Tips.Instance.Show($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
    var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
    Debug.Log(errorData);
}
```

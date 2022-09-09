# Unity

Before using, please review [the prerequisites](../../dashboard/unity-sdk-prerequisites.md)

All unity sdk are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### Init

```csharp
var metadata = DAppMetadata.Create("Particle Connect",
                "https://connect.particle.network/icons/512.png",
                "https://connect.particle.network");
ParticleNetwork.Init(_chainInfo);
ParticleConnectInteraction.Init(_chainInfo, metadata);
```

### Connect Wallet

```csharp
var nativeResultData = await ParticleConnect.Instance.Connect(this._walletType);
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

### Login, Sign-In With Ethereum or Solana.

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

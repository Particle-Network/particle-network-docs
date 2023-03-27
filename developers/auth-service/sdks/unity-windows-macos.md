# Unity-Windows-macOS

### Resources <a href="#prerequisites" id="prerequisites"></a>

* [Particle Unity SDK Github](https://github.com/Particle-Network/particle-unity)

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Unity 2020.3.26f1 or later

Require unity package:&#x20;

[3D WebView for Windows and macOS (Web Browser)](https://assetstore.unity.com/packages/tools/gui/3d-webview-for-windows-and-macos-web-browser-154144#description), version 4.3.3

If you are familiar with web browser in windows platform, you can use other web browser instead.

### Installation

Install add desktop package to your project

### Auth Service

Try our [demo](https://github.com/Particle-Network/particle-unity) for more use cases.

### In unity editor

create a GameObject and attach ParticleSystem Script, then attach a CanvasWebViewPrefab to ParticleSystem Script

### Initialize&#x20;

call ParticleSystem.Instance.Init

It request 5 parameters:

* `config`: Json string, required project id, client key and appId, could have more parameters, for more details, click [here](web.md#set-auth-theme)
* `there`: Json string, could be empty, it works for custom web wallet ui, for more details, click [here](web.md#set-auth-theme)
* `language`: support en-US, zh-CN, zh-TW, ja-JP, ko-KR
* `chainName and chainId`: support EVM chains and Solana, take a look [ChainId And ChainName Configs](../../node-service/evm-chains-api/#structure)

```csharp
// here is a simple example, and we support more parameters
var config = new ParticleConfig();
config.ProjectId = "7fa3e77f-9d07-4417-8e45-01560fef0eab";
config.ClientKey = "cRX26iCgKipWu6scQi6R8qWaP903EF3YsL3bxIym";
config.AppId = "4ca94b0e-74b9-4a5b-aad3-72ee1ed84793";

var theme = new ParticleTheme();
theme.UiMode = "dark";
theme.DisplayWallet = true;
theme.DisplayCloseButton = true;
            
string language = "en";
string chainName = "Ethereum";
long chainId = 5;

/// Init method request 5 parameters
/// config: Json string, required project id, client key and appId, could have more parameters, for more details, click here

ParticleSystem.Instance.Init(config.ToString(), theme.ToString(), language, chainName, chainId);

```

### Login

```csharp
// preferredAuthType: support email, phone and jwt.
// account: required when jwt.
var loginResult = await ParticleSystem.Instance.Login(PreferredAuthType.email, "");
Debug.Log($"Login result {loginResult}");
```

### SignMessage

```csharp
// In evm, request plain text, like "hello world", in solana, request base58 string.
var signMessageResult = await ParticleSystem.Instance.SignMessage("hello world");
Debug.Log($"SignMessage result {signMessageResult}");
```

### SignAndSendTransaction

```csharp
// make a test transaction,
// you need to update it parameters before trying.
var transaction = ParticleSystem.Instance.MakeEvmTransaction("0x16380a03f21e5a5e339c15ba8ebe581d194e0db3", "0xA719d8C4C94C1a877289083150f8AB96AD0C6aa1", "0x",
    "0x123123");
var signMessageResult = await ParticleSystem.Instance.SignAndSendTransaction(transaction);
Debug.Log($"SignAndSendTransaction result {signMessageResult}");
```

### SignTypedData

```csharp
// only support evm
// pass your typedDataV4 here.
string typedDataV4 = "";
var signMessageResult = await ParticleSystem.Instance.SignTypedData(typedDataV4, SignTypedDataVersion.Default);
Debug.Log($"SignTypedData result {signMessageResult}");
```

### SignTransaction

```csharp
// only support solana
// pass your solana transaction here, request base58 string.
string transaction = "";
var signMessageResult = await ParticleSystem.Instance.SignTransaction(transaction);
Debug.Log($"SignTransaction result {signMessageResult}");
```

### SignAllTransactions

```csharp
// only support solana
// pass your solana transactions here, request base58 string list.
List<string> transactions = new List<string> { "" };
var signMessageResult = await ParticleSystem.Instance.SignAllTransactions(transactions);
Debug.Log($"SignAllTransactions result {signMessageResult}");
```


# React Native

<mark style="color:red;">**It is strongly discouraged to use private key or mnemonic import/generate function, if you use it, you need to secure the data yourself, Particle's SDK has no relationship with the imported/generated mnemonic or private key.**</mark>

### 1.Add the Connect Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install react-native-particle-connect
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-connect) to get the demo source code&#x20;

### 2.Configure Android project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Make sure that your project meets the following requirements:

* Targets API Level 23 (Marshmallow) or higher
* Uses Android 6.0 or higher
* Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)
* Java 11

Configure project information，Refer to [here](https://github.com/Particle-Network/particle-react-native/blob/master/particle-auth/example/android/app/build.gradle)

```gradle

android {
    ...
    defaultConfig {
        ......
        manifestPlaceholders["PN_PROJECT_ID"] = "your project id"
        manifestPlaceholders["PN_PROJECT_CLIENT_KEY"] = "your project client key"
        manifestPlaceholders["PN_APP_ID"] = "your app id"
    }
   
   //How you quote wallet sdk, you must set it
    dataBinding {
        enabled = true
    }

}

```

### 3.Configure iOS project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

3.1 After export iOS project, open `your_project_name.xcworkspace` under ios folder, here its name is `ParticleConnectExample.xcworkspace.`

![](<../../.gitbook/assets/image (3) (3).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is checked under Target Membership.

![](<../../.gitbook/assets/image (1) (4).png>)

3.3 Copy the following text into this file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>PROJECT_UUID</key>
	<string>YOUR_PROJECT_UUID</string>
	<key>PROJECT_CLIENT_KEY</key>
	<string>YOUR_PROJECT_CLIENT_KEY</string>
	<key>PROJECT_APP_UUID</key>
	<string>YOUR_PROJECT_APP_UUID</string>
</dict>
</plist>

```

3.4 Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard

3.5. Import the `particle-auth` module in your `AppDelegate.m` file.

{% tabs %}
{% tab title="Objective-C" %}
```swift
#import <react_native_particle_connect/react_native_particle_connect-Swift.h>
```
{% endtab %}
{% endtabs %}

3.6. Add the scheme URL handle in your app's `application(_:open:options:)` method

{% tabs %}
{% tab title="Objective-C" %}
```swift
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
  if ([ParticleConnectSchemeManager handleUrl:url] == YES) {
    return YES;
  } else {
    // other methods
  }
  return YES;
}
```
{% endtab %}
{% endtabs %}

3.7. Configure your app scheme URL, select your app from `TARGETS`,  under `Info` section, click + to add the `URL types`, and paste your scheme in `URL Schemes`

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../.gitbook/assets/image (1) (2) (1).png>)

3.8 In Xcode right-click your `Info.plist` file and choose "Open As Source Code".

Copy & Paste the XML snippet into the body of your file (`<dict>`...`</dict>`).

<pre><code>&#x3C;key>LSApplicationQueriesSchemes&#x3C;/key>
&#x3C;array>
<strong>    &#x3C;string>imtokenv2&#x3C;/string>
</strong>    &#x3C;string>metamask&#x3C;/string>
    &#x3C;string>phantom&#x3C;/string>
    &#x3C;string>bitkeep&#x3C;/string>
    &#x3C;string>trust&#x3C;/string>
    &#x3C;string>rainbow&#x3C;/string>
    &#x3C;string>gnosissafe&#x3C;/string>
    &#x3C;string>zerion&#x3C;/string>
    &#x3C;string>mathwallet&#x3C;/string>
    &#x3C;string>1inch&#x3C;/string>
    &#x3C;string>awallet&#x3C;/string>
    &#x3C;string>bitpie&#x3C;/string>
&#x3C;/array>
</code></pre>

3.9 Edit Podfile, there is a Podfile under ios folder.

```ruby
// From iOS native SDK 0.9.12, you should add more in podfile
If you use ParticleWalletGUI, you need add this one.
pod 'SkeletonView', :git => 'https://github.com/SunZhiC/SkeletonView.git', :branch => 'main'

// If you use PartcleWalletConnect or ConenctWalletConnectAdapter, you need add this one.
pod 'WalletConnectSwift', :git => 'https://github.com/SunZhiC/WalletConnectSwift', :branch => 'master'
```

### Initialize the SDK

### Usage

```javascript
import * as particleConnect from 'react-native-particle-connect';
```

### Initialize the SDK

**Before using the sdk you have to call init(Required)**&#x20;

```javascript
// Init particle connect SDK,
// metadata is your app info, will pass to wallet when wallet connect.
// you can use the values that we provided in example.
const chainInfo = ChainInfo.EthereumGoerli;
const env = Env.Dev;
const metadata = { name: "Particle Connect", icon: "https://connect.particle.network/icons/512.png", url: "https://connect.particle.network" }
const rpcUrl = { evm_url: null, solana_url: null };
particleConnect.init(chainInfo, env, metadata, rpcUrl);
```

### Connect

```javascript
// connect example
const result = await particleConnect.connect(walletType);
if (result.status) {
    console.log("connect success")
    const account = result.data;
    pnaccount = new PNAccount(account.icons, account.name, account.publicAddress, account.url);
    console.log("pnaccount = ", pnaccount);
} else {
    console.log("connect failure")
    const error = result.data;
    console.log(error);
}
// connect with particle, support pass more parameters
// set login type, account, support types and login form mode
const connectConfig = new ParticleConnectConfig(LoginType.Phone, "", [SupportAuthType.Facebook, SupportAuthType.Google, SupportAuthType.Apple])
const result = await particleConnect.connect(WalletType.Particle, connectConfig);
if (result.status) {
    console.log("connect success")
    const account = result.data;
    pnaccount = new PNAccount(account.icons, account.name, account.publicAddress, account.url);
    console.log("pnaccount = ", pnaccount);
} else {
    console.log("connect failure")
    const error = result.data;
    console.log(error);
}
```

### Disconnect

```javascript
const publicAddress = TestAccountEVM.publicAddress;
const result = await particleConnect.disconnect(walletType, publicAddress);
if (result.status) {
    console.log(result.data);
} else {
    const error = result.data;
    console.log(error);
}
```

### IsConnected

```javascript
const publicAddress = pnaccount.publicAddress;
const isConnected = await particleConnect.isConnected(walletType, publicAddress);
console.log(isConnected);
```

### Login (Sign-In With Ethereum or Solana)

```javascript
const publicAddress = pnaccount.publicAddress;
const domain = "login.xyz";
const uri = "https://login.xyz/demo#login";
const result = await particleConnect.login(walletType, publicAddress, domain, uri);
if (result.status) {
    const message = result.data.message;
    loginSourceMessage = message;
    const signature = result.data.signature;
    loginSignature = signature
    console.log("login message:", message);
    console.log("login signature:", signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Verify

```javascript
const publicAddress = pnaccount.publicAddress;
const message = loginSourceMessage;
const signature = loginSignature;
console.log("verify message:", message);
console.log("verify signature:", signature);
const result = await particleConnect.verify(walletType, publicAddress, message, signature);
if (result.status) {
    const flag = result.data;
    console.log(flag);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign message

```dart
const message = "Hello world!"
const publicAddress = pnaccount.publicAddress;
const result = await particleConnect.signMessage(walletType, publicAddress, message);
if (result.status) {
    const signedMessage = result.data;
    console.log(signedMessage);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign transaction

```dart
const transaction = ""
const publicAddress = pnaccount.publicAddress;
const result = await particleConnect.signTransaction(walletType, publicAddress, transaction);
if (result.status) {
    const signedTransaction = result.data;
    console.log(signedTransaction);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign all transactions

```dart
const transactions = ["", ""]
const publicAddress = pnaccount.publicAddress;
const result = await particleConnect.signAllTransactions(walletType, publicAddress, transactions);
if (result.status) {
    const signedTransactions = result.data;
    console.log(signedTransactions);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign and send transaction

```dart
const transaction = await Helper.getEthereumTransacion(pnaccount.publicAddress);
console.log(transaction);
const publicAddress = pnaccount.publicAddress;
const result = await particleConnect.signAndSendTransaction(walletType, publicAddress, transaction);
if (result.status) {
    const signature = result.data;
    console.log("signAndSendTransaction:", signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign typed data

```javascript
const typedData = "{        \"types\": {            \"EIP712Domain\": [                {                    \"name\": \"name\",                    \"type\": \"string\"                },                {                    \"name\": \"version\",                    \"type\": \"string\"                },                {                    \"name\": \"chainId\",                    \"type\": \"uint256\"                },                {                    \"name\": \"verifyingContract\",                    \"type\": \"address\"                }            ],            \"Person\": [                {                    \"name\": \"name\",                    \"type\": \"string\"                },                {                    \"name\": \"wallet\",                    \"type\": \"address\"                }            ],            \"Mail\": [                {                    \"name\": \"from\",                    \"type\": \"Person\"                },                {                    \"name\": \"to\",                    \"type\": \"Person\"                },                {                    \"name\": \"contents\",                    \"type\": \"string\"                }            ]        },        \"primaryType\": \"Mail\",        \"domain\": {            \"name\": \"Ether Mail\",            \"version\": \"1\",            \"chainId\": 5,            \"verifyingContract\": \"0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC\"        },        \"message\": {            \"from\": {                \"name\": \"Cow\",                \"wallet\": \"0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826\"            },            \"to\": {                \"name\": \"Bob\",                \"wallet\": \"0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB\"            },            \"contents\": \"Hello, Bob!\"        }}        ";
const publicAddress = pnaccount.publicAddress;
const result javjav= await particleConnect.signTypedData(walletType, publicAddress, typedData);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Import private key

```javascript
const privateKey = TestAccountEVM.privateKey;
const result = await particleConnect.importPrivateKey(WalletType.EvmPrivateKey, privateKey);
if (result.status) {
    const ajavccount = result.data;
    console.log(account);
} else {
    const error = result.data;
    console.log(error);
}
```

### Import mnemonic

```javascript
const mnemonic = TestAccountEVM.mnemonic;
const result = await particleConnect.importMnemonic(WalletType.EvmPrivateKey, mnemonic);
if (result.status) {
    const account = result.data;
    console.log(account);
} else {
    const error = result.data;
    console.log(error);
}
```

### Export private key

```javascript
const publicAddress = TestAccountEVM.publicAddress;
const result = await particleConnect.exportPrivateKey(WalletType.EvmPrivateKey, publicAddress);
if (result.status) {
    const privateKey = result.data;
    console.log(privateKey);
} else {
    const error = result.data;
    console.log(error);
}
```

### Get accounts

```javascript
const accounts = await particleConnect.getAccounts(walletType);
console.log(accounts);
```

### Set chain info async&#x20;

```javascript
const chainInfo = ChainInfo.EthereumGoerli;
const result = await particleConnect.setChainInfoAsync(chainInfo);
console.log(result);
```

### Set chain info

```javascript
const chainInfo = ChainInfo.EthereumGoerli;
const result = await particleConnect.setChainInfo(chainInfo);
console.log(result);
```

### Get chain info

```javascript
const chainInfo = await ParticleConnect.getChainInfo();
console.log(chainInfo);
```

### Add ethereum chain

for how to connect a wallet, please look at this [page](../faq.md) , the principle is the same.

```javascript
addEthereumChain = async () => {
    const publicAddress = TestAccountEVM.publicAddress;
    const chainId = 80001; // polygon testnet

    const result = await particleConnect.addEthereumChain(WalletType.MetaMask, publicAddress, chainId);

    if (result.status) {
        const data = result.data;
        console.log(data);
    } else {
        const error = result.data;
        console.log(error);
    }
}
```

### Switch ethereum chain&#x20;

```javascript
switchEthereumChain = async () => {
    const publicAddress = TestAccountEVM.publicAddress;
    const chainId = 137; // polygon mainnet

    const result = await particleConnect.switchEthereumChain(WalletType.MetaMask, publicAddress, chainId);

    if (result.status) {
        const data = result.data;
        console.log(data);
    } else {
        const error = result.data;
        console.log(error);
    }
}
```


# Flutter

### 1.Add the Connect Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_connect
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-connect) to get the demo source code&#x20;

### 2.Configure Android project







### 3.Configure iOS project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

3.1 After export iOS project, open Runner.xcworkspace under ios folder.

![](<../../.gitbook/assets/image (5) (1).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is checked under Target Membership.

![](<../../.gitbook/assets/image (1) (3).png>)

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

3.5. Import the `ParticleAuthService` module in your `AppDelegate.swift` file.

{% tabs %}
{% tab title="Swift" %}
```swift
import ParticleConnect
```
{% endtab %}
{% endtabs %}

3.6. Add the scheme URL handle in your app's `application(_:open:options:)` method

{% tabs %}
{% tab title="Swift" %}
```swift
override func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
    if ParticleConnect.handleUrl(url) {
        return true
    } else {
        return super.application(app, open: url, options: options)
    }
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
&#x3C;/array>
&#x3C;key>NSPhotoLibraryUsageDescription&#x3C;/key>
    &#x3C;string>We need access in order to open photos of barcodes&#x3C;/string>
&#x3C;key>NSCameraUsageDescription&#x3C;/key>
    &#x3C;string>We use the camera to scan barcodes&#x3C;/string>
</code></pre>



### Initialize the SDK

**Before using the sdk you have to call init(Required)**&#x20;

```dart
// Init particle connect SDK,
// dappInfo is your app info, will pass to wallet when wallet connect.
final dappInfo = DappMetaData("Particle Connect", "https://connect.particle.network/icons/512.png", "https://connect.particle.network");
ParticleConnect.init(currChainInfo, dappInfo, Env.dev);
```

### Connect

```dart
final result = await ParticleConnect.connect(WalletType.particle);
print("connect: $result");
```

### Disconnect

```dart
String result = await ParticleConnect.disconnect(WalletType.particle, getPublicAddress());
print("logout: $result");
```

### IsConnected

```dart
bool result = await ParticleConnect.isConnected(WalletType.particle, getPublicAddress());
print("isConnected: $result");
```

### Login (Sign-In With Ethereum or Solana)

```dart
String result = await ParticleConnect.login(WalletType.particle, getPublicAddress(), "login.xyz", "https://login.xyz/demo#login");
print("loginResult: $result");
```

### Verify

```dart
String result = await ParticleConnect.verify(WalletType.particle, getPublicAddress(), message, signature);
print("verifyResult: $result");
```

### Sign message

```dart
String result = await ParticleConnect.signMessage(WalletType.particle, getPublicAddress(), "Hello Particle");
print("result: $result");
```

### Sign transaction

```dart
String result = await ParticleConnect.signTransaction(WalletType.particle, getPublicAddress(), transaction);
print("result: $result");
```

### Sign all transactions

```dart
String result = await ParticleConnect.signAllTransactions(WalletType.particle, getPublicAddress(), transactions);
print("result: $result");
```

### Sign and send transaction

```dart
String result = await ParticleConnect.signAndSendTransaction(WalletType.particle, getPublicAddress(), transaction);
print("result: $result");
```

### Sign typed data

```dart
String result = await ParticleConnect.signTypedData(WalletType.particle, getPublicAddress(), typedData);
print("result: $result");
```

### Import private key

```dart
String result = await ParticleConnect.importPrivateKey(WalletType.particle, privateKey);
Map<String, dynamic> jsonResult = jsonDecode(result);
if (jsonResult["status"]  == 1 || jsonResult["status"]  == true) {
  publicAddress = jsonResult["data"]["publicAddress"];
  print("publicAddress:$publicAddress");
} else {
  print("${jsonResult["data"]}")
}
```

### Import mnemonic

```dart
String result = await ParticleConnect.importMnemonic(WalletType.particle, mnemonic);
Map<String, dynamic> jsonResult = jsonDecode(result);
if (jsonResult["status"]  == 1 || jsonResult["status"]  == true) {
  publicAddress = jsonResult["data"]["publicAddress"];
  print("publicAddress:$pubAddress");
} else {
  print("${jsonResult["data"]}");
}
```

### Export private key

```dart
String result = await ParticleConnect.exportPrivateKey(WalletType.particle, getPublicAddress());
Map<String, dynamic> jsonResult = jsonDecode(result);
if (jsonResult["status"]  == 1 || jsonResult["status"]  == true) {
  publicAddress = jsonResult["data"]["publicAddress"];
  print("pubAddress:$publicAddress");
} else {
  print("${jsonResult["data"]}");
}
```

### Add ethereum chain

only support metamask, other wallets don't support this method.

<pre class="language-dart"><code class="lang-dart">int chainId = 80001;
<strong>String result = await ParticleConnect.addEthereumChain(walletType, getPublicAddress(), chainId);
</strong>print(result);
</code></pre>

### Switch ethereum chain

only support metamask, other wallets don't support this method

```dart
int chainId = 5;
String result = await ParticleConnect.switchEthereumChain(walletType, getPublicAddress(), chainId);
print(result);
```

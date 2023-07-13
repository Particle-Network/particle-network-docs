# Flutter

<mark style="color:red;">**It is strongly discouraged to use private key or mnemonic import/generate function, if you use it, you need to secure the data yourself, Particle's SDK has no relationship with the imported/generated mnemonic or private key.**</mark>

### 1.Add the Connect Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_connect
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-connect) to get the demo source code&#x20;

### 2.Configure Android project

open ${your flutter project} /android/app/build.gradle

2.1 add project config and update the minSdkVersion to 23

```gradle
//Modify these configurations
defaultConfig {
    // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
    applicationId "com.example.particle_auth_test"
    // You can update the following values to match your application needs.
    // For more information, see: https://docs.flutter.dev/deployment/android#reviewing-the-build-configuration.
    minSdkVersion flutter.minSdkVersion
    targetSdkVersion flutter.targetSdkVersion
    versionCode flutterVersionCode.toInteger()
    versionName flutterVersionName
}

//After modification：
defaultConfig {
   // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
   applicationId "com.example.particle_auth_test"
   // You can update the following values to match your application needs.
   // For more information, see: https://docs.flutter.dev/deployment/android#reviewing-the-build-configuration.
   minSdkVersion 23 // required by particle sdk
   targetSdkVersion flutter.targetSdkVersion
   versionCode flutterVersionCode.toInteger()
   versionName flutterVersionName

   //get from https://dashboard.particle.network/
   manifestPlaceholders["PN_PROJECT_ID"] = "772f7499-1d2e-40f4-8e2c-7b6dd47db9de" //your project id
   manifestPlaceholders["PN_PROJECT_CLIENT_KEY"] = "ctWeIc2UBA6sYTKJknT9cu9LBikF00fbk1vmQjsV" //your project client key
   manifestPlaceholders["PN_APP_ID"] = "01a23ce8-d2e9-4b37-9eab-bf477279e53e" //your app id
}
```

2.2 update Java Version to 11 and add dataBinding config

```gradle
//Modify these configurations
compileOptions {
  sourceCompatibility JavaVersion.VERSION_1_8
  targetCompatibility JavaVersion.VERSION_1_8
}

kotlinOptions {
  jvmTarget = '1.8'
}

//After modification：
compileOptions {
  sourceCompatibility JavaVersion.VERSION_11
  targetCompatibility JavaVersion.VERSION_11
}

kotlinOptions {
  jvmTarget = JavaVersion.VERSION_11
}

dataBinding {
  enabled = true
}
```

Now,Android configuration is complete!

### 3.Configure iOS project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 14.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 14

3.1 After export iOS project, open Runner.xcworkspace under ios folder.

![](<../../../.gitbook/assets/image (5) (1).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is checked under Target Membership.

![](<../../../.gitbook/assets/image (1) (3).png>)

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

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)

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
    &#x3C;string>zerion&#x3C;/string>
    &#x3C;string>mathwallet&#x3C;/string>
    &#x3C;string>1inch&#x3C;/string>
    &#x3C;string>awallet&#x3C;/string>
    &#x3C;string>bitpie&#x3C;/string>
&#x3C;/array>
&#x3C;key>NSPhotoLibraryUsageDescription&#x3C;/key>
    &#x3C;string>We need access in order to open photos of barcodes&#x3C;/string>
&#x3C;key>NSCameraUsageDescription&#x3C;/key>
    &#x3C;string>We use the camera to scan barcodes&#x3C;/string>
</code></pre>

3.9 Edit Podfile, you should follow [Podfile required](ios.md#edit-podile) to edit Podfile.



### Initialize the SDK

**Before using the SDK you have to call init(Required)**&#x20;

```dart
// Get your project id and client from dashboard, 
// https://dashboard.particle.network
const projectId = ""; // your project id
const clientK = ""; // your client key 
ParticleInfo.set(projectId, clientK);
```

{% hint style="info" %}
## Migrating to WalletConnect v2

Starting from version 0.14.0, WalletConnectV2 is supported.

```dart
// Init particle connect SDK,
// dappInfo is your app info, will pass to wallet when wallet connect.
final dappInfo = DappMetaData(
        "your wallet connect project id",
        "Particle Connect",
        "https://connect.particle.network/icons/512.png",
        "https://connect.particle.network",
        "Particle Connect Flutter Demo");
ParticleConnect.init(currChainInfo, dappInfo, Env.dev);

// set wallet connect support chaininfos, if you dont call this method
// default value is current chaininfo.
List<ChainInfo> chainInfos = <ChainInfo>[EthereumChain.mainnet(), PolygonChain.mainnet()];
ParticleConnect.setWalletConnectV2SupportChainInfos(chainInfos);
```
{% endhint %}

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
String result = await ParticleConnect.importPrivateKey(WalletType.evmPrivateKey, privateKey);
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
String result = await ParticleConnect.importMnemonic(WalletType.evmPrivateKey, mnemonic);
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
String result = await ParticleConnect.exportPrivateKey(WalletType.evmPrivateKey, getPublicAddress());
Map<String, dynamic> jsonResult = jsonDecode(result);
if (jsonResult["status"]  == 1 || jsonResult["status"]  == true) {
  publicAddress = jsonResult["data"]["publicAddress"];
  print("pubAddress:$publicAddress");
} else {
  print("${jsonResult["data"]}");
}
```

### Add ethereum chain

from test, this method works well in metamask, it doesn't work well in other wallets.

```dart
int chainId = 80001;
String result = await ParticleConnect.addEthereumChain(walletType, getPublicAddress(), chainId);
print(result);
```

### Switch ethereum chain

from test, this method works well in metamask, it doesn't work well in other wallets.

```dart
int chainId = 5;
String result = await ParticleConnect.switchEthereumChain(walletType, getPublicAddress(), chainId);
print(result);
```

### Reconnect wallet connect wallet

only support iOS, usually use when open app, if current wallet is from wallet connect, call this method to reconnect, you can call this method anywhere anytime.

It is better to start a connection before sign.

```dart
ParticleConnect.reconnectIfNeeded(walletType, getPublicAddress());
```

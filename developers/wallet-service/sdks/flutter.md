# Flutter

### 1.Add the Wallet Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_wallet
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-wallet) to get the demo source code&#x20;

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
}o
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

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 14.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

3.1 After export iOS project, open Runner.xcworkspace under ios folder.

![](<../../../.gitbook/assets/image (5) (1).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is check under Target Membership.

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
import ParticleAuthService
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
@import ParticleAuthService;
```
{% endtab %}
{% endtabs %}

3.6. Add the scheme URL handle in your app's `application(_:open:options:)` method

{% tabs %}
{% tab title="Swift" %}
```swift
return ParticleAuthService.handleUrl(url)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
return [ParticleAuthService handleUrl:url];
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

3.9 Edit Podfile, you should follow [Podfile required](ios.md#edit-podfile) to edit Podfile.

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**&#x20;

{% hint style="info" %}
## Migrating to WalletConnect v2

Starting from version 0.14.0, WalletConnectV2 is supported.

```swift
ParticleWallet.setWalletConnectV2ProjectId(
        "your wallet connect v2 project id")

ParticleWallet.init(WalletMetaData(
        "Particle Connect",
        "https://connect.particle.network/icons/512.png",
        "https://connect.particle.network",
        "Particle Connect Flutter Demo"));
```
{% endhint %}

### Navigator wallet

```dart
ParticleWallet.navigatorWallet();
```

### Navigator token receive

```dart
ParticleWallet.navigatorTokenReceive();
```

### Navigator token send&#x20;

```dart
String tokenAddress = "8ci2fZwQSmArbeHkuA7M8h5rsRwwh4FMTXrZxb3KsDpb";
String toAddress = "1232fZwQSmArbeHkuA7M8h5rsRwwh4FMTXrZxb3KsDpb";
int amount = 1000000;
ParticleWallet.navigatorTokenSend(tokenAddress: tokenAddress, toAddress: toAddress, amount: amount);
```

### Navigator token transaction records

```dart
String tokenAddress = "8ci2fZwQSmArbeHkuA7M8h5rsRwwh4FMTXrZxb3KsDpb";
ParticleWallet.navigatorTokenTransactionRecords(tokenAddress: tokenAddress);
```

### Navigator NFT send

```dart
String mintAddress = "0xD000F000Aa1F8accbd5815056Ea32A54777b2Fc4";
String tokenId = "1412";
ParticleWallet.navigatorNFTSend(mintAddress, tokenId, receiveAddress: "");
```

### Navigator NFT details

```dart
String mint = "0xD000F000Aa1F8accbd5815056Ea32A54777b2Fc4";
String tokenId = "1412";
ParticleWallet.navigatorNFTDetails(mint, tokenId);
```

### Enable pay

```dart
bool enable = true;
ParticleWallet.enablePay(enable);
```

### Get enable pay

```dart
bool isEnable = await ParticleWallet.getEnablePay();
showToast("isEnable:$isEnable");
```

### Enable swap

```dart
bool enable = true;
ParticleWallet.enableSwap(enable);
```

### Get enable swap

```dart
bool isEnable = await ParticleWallet.getEnableSwap();
showToast("isEnable:$isEnable");
```

### Navigator buy crypto

```dart
final config = BuyCryptoConfig(walletAddress:"your wallet address", cryptoCoin:"USDT", fiatCoin:"USD", fiatAmt:1000, network:OpenBuyNetwork.ethereum);
ParticleWallet.navigatorBuyCrypto(config: config);
```

### Navigator swap

```dart
ParticleWallet.navigatorSwap();
```

### Navigator login list

```dart
daString result = await ParticleWallet.navigatorLoginList();
print("result:$result");
```

### Navigator dapp browser

```dart
const url = "https://opensea.io";
ParticleWallet.navigatorDappBrowser(url: url);
```

### Set Support chain

```dart
 List<ChainInfo> chainInfos = <ChainInfo>[];
 chainInfos.add(EthereumChain.mainnet());
 chainInfos.add(PolygonChain.mainnet());
 chainInfos.add(BSCChain.mainnet());
 ParticleWallet.supportChain(chainInfos);
```

### Switch wallet

```dart
WalletType walletType = WalletType.particle;
String publicAddress = "";
String result = await ParticleWallet.switchWallet(walletType, publicAddress);
print("result:$result");
```

### Show test network

```dart
bool enable = false;
ParticleWallet.showTestNetwork(enable);
```

### Show manage wallet

```dart
bool enable = true;
ParticleWallet.showManageWallet(enable);
```

### Set language

```dart
Language language = Language.system;
ParticleWallet.setLanguage(language);
```

### Custom UI

```dart
static void showLanguageSetting() {
  const isShow = false;
  ParticleWallet.showLanguageSetting(isShow);
}

static void showAppearanceSetting() {
  const isShow = false;
  ParticleWallet.showAppearanceSetting(isShow);
}


/// Set support dapp broswer in wallet page, default is true
static void supportDappBrowser() {
  ParticleWallet.supportDappBrowser(false);
}

/// Set support wallet connect as a wallet, default is true
static void supportWalletConnect() {
  ParticleWallet.supportWalletConnect(false);
}

static void navigatorDappBrowser() {
  const url = "https://opensea.io";
  ParticleWallet.navigatorDappBrowser(url: url);
}


static void setSupportAddToken() {
  const isShow = false;
  ParticleWallet.setSupportAddToken(isShow);
}

static void setDisplayTokenAddresses() {
  List<String> tokenAddresses = <String>[
    "0x303b35f48684bea50D0e7D1AcDdeaf78A7188798"
  ];

  ParticleWallet.setDisplayTokenAddresses(tokenAddresses);
}

static void setDisplayNFTContractAddresses() {
  List<String> nftContractAddresses = <String>[
    "0xD18e451c11A6852Fb92291Dc59bE35a59d143836"
  ];
  ParticleWallet.setDisplayNFTContractAddresses(nftContractAddresses);
}

static void setPriorityTokenAddresses() {
  List<String> tokenAddresses = <String>[
    "0x303b35f48684bea50D0e7D1AcDdeaf78A7188798"
  ];

  ParticleWallet.setPriorityTokenAddresses(tokenAddresses);
}

static void setPriorityNFTContractAddresses() {
  List<String> nftContractAddresses = <String>[
    "0xD18e451c11A6852Fb92291Dc59bE35a59d143836"
  ];
  ParticleWallet.setPriorityNFTContractAddresses(nftContractAddresses);
}

static void setFiatCoin() {
  const fiatCoin = FiatCoin.HKD;
  ParticleWallet.setFiatCoin(fiatCoin);
}

static void loadCustomUIJsonString() {
  // your custom json string.
  // example https://github.com/Particle-Network/particle-ios/blob/main/Demo/Demo/customUIConfig.json
  const json = "";
  ParticleWallet.loadCustomUIJsonString(json);
}

```

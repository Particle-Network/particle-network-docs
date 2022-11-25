# Flutter

### 1.Add the Wallet Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_wallet
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-wallet) to get the demo source code&#x20;

### 2.Configure Android project





### 3.Configure iOS project

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

3.1 After export iOS project, open Runner.xcworkspace under ios folder.

![](<../../.gitbook/assets/image (5) (1).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is check under Target Membership.

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
    &#x3C;string>We use the camera to scan barcodes&#x3C;/string></code></pre>



### Initialize the SDK

**Before using the SDK, you have to call init(Required)**&#x20;

```dart
ParticleWallet.init();
```

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

### Set interface style

```dart
UserInterfaceStyle interfaceStyle = UserInterfaceStyle.light;
ParticleWallet.setInterfaceStyle(interfaceStyle);
```

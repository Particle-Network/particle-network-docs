# Social Login + Embeded Wallet

This guild will help you get started with Particle Connect Flutter SDK by setting up your environment, creating a wallet with social account, and sending a EVM transaction. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

### iOS

* iOS 14 or higher
* Xcode 14.1 or higher
* CocoaPods 1.12.0 or higher

### Android







## Installation

Install ParticleConnect Flutter SDK using pub.

```
flutter pub add particle_connect
```

## Register your application

In order to use ParticleConnectService, you'll need to create a project in the [Particle Dashboard](../../dashboard/) and get your project id, client key and project app id.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

## Create and initialize the ParticleConnectService

### iOS

Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, Copy the following text into this file, then replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard.

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

Next step is set your scheme, select your app from TARGETS in Xcode, go to the "Info" section and click on the '+' button to add a new entry to the "URL types" field, under "URL Schemes", paste your specific scheme information.

Your scheme URL should be `"pn"` followed by your project app id.

For instance, if your project app id is `"63bfa427-cf5f-4742-9ff1-e8f5a1b9828f"`, your scheme URL should be `"pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f"`.

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)

Edit `Podfile`, specify Native SDK version and set `use_frameworks! :linkage => :static`

set \`BUILD\_LIBRARY\_FOR\_DISTRIBUTION\`&#x20;

Here is an example Podfile.

```ruby
# Uncomment this line to define a global platform for your project
platform :ios, '14.0'

# CocoaPods analytics sends network stats synchronously affecting flutter build latency.
ENV['COCOAPODS_DISABLE_STATS'] = 'true'

project 'Runner', {
  'Debug' => :debug,
  'Profile' => :release,
  'Release' => :release,
}

def flutter_root
  generated_xcode_build_settings_path = File.expand_path(File.join('..', 'Flutter', 'Generated.xcconfig'), __FILE__)
  unless File.exist?(generated_xcode_build_settings_path)
    raise "#{generated_xcode_build_settings_path} must exist. If you're running pod install manually, make sure flutter pub get is executed first"
  end

  File.foreach(generated_xcode_build_settings_path) do |line|
    matches = line.match(/FLUTTER_ROOT\=(.*)/)
    return matches[1].strip if matches
  end
  raise "FLUTTER_ROOT not found in #{generated_xcode_build_settings_path}. Try deleting Generated.xcconfig, then run flutter pub get"
end

require File.expand_path(File.join('packages', 'flutter_tools', 'bin', 'podhelper'), flutter_root)

flutter_ios_podfile_setup

target 'Runner' do
  use_frameworks! :linkage => :static
  use_modular_headers!
  
  pod 'ParticleNetworkBase', '1.3.5'
  pod 'ParticleAuthService', '1.3.5'
  pod 'ParticleWalletAPI', '1.3.5'

  pod 'ConnectCommon', '0.2.14'
  pod 'ConnectEVMAdapter', '0.2.14'
  pod 'ConnectSolanaAdapter', '0.2.14'
  pod 'ConnectWalletConnectAdapter', '0.2.14'
  pod 'ConnectPhantomAdapter', '0.2.14'
  pod 'ParticleConnect', '0.2.14'
  pod 'ParticleAuthAdapter', '0.2.14'

  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
  target 'RunnerTests' do
    inherit! :search_paths
  end
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
  end
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
    config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
      end
    end
end
```

If you want to run in simulator, you should edit `Architectures` under `Build Settings`, set to arm64.

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption><p>Edit Architechtures</p></figcaption></figure>

Edit AppDelegate.swift

```swift
import Flutter
import ParticleConnect
import UIKit

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    override func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        GeneratedPluginRegistrant.register(with: self)
        return super.application(application, didFinishLaunchingWithOptions: launchOptions)
    }

    override func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
        if ParticleConnect.handleUrl(url) {
            return true
        } else {
            return super.application(app, open: url, options: options)
        }
    }
}
```

### Android







### Initialization in flutter

Now, let's initialize ParticleConnect in your project, chainInfo signifies the chain (ethereum, polygon ...) you want to initialize ParticleConnect with.

```dart
void _init() {
    // Get your project id and client key from dashboard, https://dashboard.particle.network
    const projectId =
        "your project id"; 
    const clientK =
        "your client key"; 
    ParticleInfo.set(projectId, clientK);

    // you can set your app info here, use for wallet connect.
    final dappInfo = DappMetaData(
        "your wallet connect id",
        "Particle Connect",
        "https://connect.particle.network/icons/512.png",
        "https://connect.particle.network",
        "Particle Connect Flutter Demo");

    ParticleConnect.init(ChainInfo.PolygonMumbai, dappInfo, Env.dev);
  }
```

**All here, initialization is complete.**

## Triggering the login

Trigger login flow with email, phone number, google, facebook, twitter, JWT, etc.

You need to save the returned Account object for convenient use next time.

```dart
void _connectParticle() async {
    try {
      final config = ParticleConnectConfig(LoginType.google, "",
          [SupportAuthType.all], SocialLoginPrompt.none);
      final account =
          await ParticleConnect.connect(WalletType.particle, config: config);
      this.account = account;
      print("connect particle $account");
    } catch (error) {
      print("connect particle $error");
    }
  }
```

{% hint style="info" %}
If you want to connect with JWT, you should pass JWT as the second parameter.
{% endhint %}

```dart
final config = ParticleConnectConfig(LoginType.jwt, "your jwt",
          [SupportAuthType.all], SocialLoginPrompt.none);
```

{% hint style="info" %}
If you connect with particle, you can get a userInfo, it has token inside. Other wallets don't support userInfo.
{% endhint %}

```dart
final userInfo = ParticleAuth.getUserInfo();
```

Particle Connect Flutter SDK also support connect with other wallets, here is an example show how to connect with MetaMask wallet, other wallets are in the same way, just change the walletType.

```dart
void _connectMetaMask() async {
    try {
      final account = await ParticleConnect.connect(WalletType.metaMask);
      this.account = account;
      print("connect metaMask $account");
    } catch (error) {
      print("connect metaMask $error");
    }
  }
```

## Triggering the sign

After connect, if you want to authenticate by signature call `signMessage`.

```dart
void _signMessage() async {
  if (account == null) {
    print("didn't connect any account");
    return;
  }
  try {
    final walletType = WalletType.values.firstWhere((element) =>
        element.name.toLowerCase() == account!.walletType!.toLowerCase());
    final signature = await ParticleConnect.signMessage(
        walletType,
        account!.publicAddress,
        "0x${StringUtils.toHexString("Hello Particle")}");
    print("sign message $signature");
  } catch (error) {
    print("sign message $error");
  }
}
```

If you want to send a EVM transaction, call `signAndSendTransaction`.

You can create a transaction by `EvmService.createTransaction` method.

```dart
void _sendTransaction() async {
  if (account == null) {
    print("didn't connect any account");
    return;
  }
  try {
    final walletType = WalletType.values.firstWhere((element) =>
        element.name.toLowerCase() == account!.walletType!.toLowerCase());
    final transaction = await EvmService.createTransaction(
        account!.publicAddress,
        "0x",
        BigInt.from(1000000000000000),
        "0x0000000000000000000000000000000000000000");
    final signature = await ParticleConnect.signAndSendTransaction(
        walletType, account!.publicAddress, transaction);
    print("send transaction $signature");
  } catch (error) {
    print("send transaction $error");
  }
}
```

{% hint style="info" %}
`signTypeData` does the same way with `signMessage`
{% endhint %}

## Log the user out

Use the `disconnect` function of ParticleConnect to trigger the logout flow.&#x20;

```dart
void _disconnect() async {
  if (account == null) {
    print("didn't connect any account");
    return;
  }
  try {
    final walletType = WalletType.values.firstWhere((element) =>
        element.name.toLowerCase() == account!.walletType!.toLowerCase());

    final result =
        await ParticleConnect.disconnect(walletType, account!.publicAddress);
    print("disconnect $result");
    account = null;
  } catch (error) {
    print("disconnect $error");
  }
}
```

## Dive Deeper

In this guide, we learned how to use the Particle Connect Flutter SDK for social login, sending transactions, and signing message. If you want to learn more about different use cases, check out the [Particle Connect](../../../developers/connect-service/sdks/flutter.md) page.




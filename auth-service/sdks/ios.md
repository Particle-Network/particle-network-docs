# iOS

## Add Auth Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

### Create a Particle Project and App

Before you can add our Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Auth Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Auth Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Auth Service's CocoaPods distribution requires Xcode 13.3.1 and CocoaPods 1.10.0 or higher. Here's how to install the Auth Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```ruby
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app:

```ruby
pod 'ParticleAuthService'
```

3\. Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

```ruby
pod install --repo-update
```

```ruby
open your-project.xcworkspace
```

{% hint style="info" %}
If you would like to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

{% hint style="info" %}
### ARM64 Simulator support

For everyone with an M1 (Silicon) device who wishes to run their projects on a simulator, There are two solutions.

1. Set arm64 to exclude the architecture for any iOS Simulator SDK, then add the following to the Podfile:

```ruby
post_install do |installer|
  installer.pods_project.build_configurations.each do |config|
    config.build_settings["EXCLUDED_ARCHS[sdk=iphonesimulator*]"] = "arm64"
    config.build_settings["ARCHS[sdk=iphonesimulator*]"] = "x86_64"
    config.build_settings["ARCHS[sdk=iphoneos*]"] = "arm64"
  end
end
```

2\. Run Xcode with Rosetta.
{% endhint %}

### Initialize Auth Service in your app <a href="#initialize-firebase" id="initialize-firebase"></a>

The final step is to add an initialization code to your application. You may have already done this as part of adding the Auth Service to your app. If you are using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

1. Create a **ParticleNetwork-Info.plist** into the root of your Xcode project
2. Copy the following text into this file:

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

3\. Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard

4\. Import the `ParticleNetwork` module in your `UIApplicationDelegate`

{% tabs %}
{% tab title="Swift" %}
```swift
import ParticleNetworkBase 
import ParticleAuthService
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
@import ParticleNetworkBase;
@import ParticleAuthService;
```
{% endtab %}
{% endtabs %}

5\. Initialize the ParticleNetwork service, which is typically in your app's `application:didFinishLaunchingWithOptions:` method:

{% tabs %}
{% tab title="Swift" %}
```swift
// select a network from ChainInfo.
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.mainnet)
let devEnv = ParticleNetwork.DevEnvironment.debug
let config = ParticleNetworkConfiguration(chainInfo: chainInfo, devEnv: devEnv)
ParticleNetwork.initialize(config: config)

// also custom your evm network.
let chainInfo = ParticleNetwork.ChainInfo.customEvmNetwork(fullName: "Ethereum", network: "rinkeby", chainId: 4, explorePath: "https://rinkeby.etherscan.io/", symbol: "ETH")
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// select a network from ChainName enum.
ChainInfo *chainInfo = [ChainInfo ethereum:EthereumNetworkMainnet];
DevEnvironment devEnv = DevEnvironmentDebug;
ParticleNetworkConfiguration *config = [[ParticleNetworkConfiguration alloc] initWithChainInfo:chainInfo devEnv:devEnv];
[ParticleNetwork initializeWithConfig:config];

// also custom your evm network.
ChainName *chainName = [ChainName customEvmNetworkWithFullName:@"Ethereum" network:@"rinkeby" chainId:4 explorePath:@"https://rinkeby.etherscan.io/" symbol:@"ETH" isSupportEIP1159:YES];
```
{% endtab %}
{% endtabs %}

6\. Add the scheme URL handle in your app's `application(_:open:options:)` method

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

7\. Configure your app scheme URL, select your app target in the info section, click to add the URL type, and pass your scheme in URL Schemes

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../.gitbook/assets/image (1).png>)

{% hint style="info" %}
devEnv needs to be modified to be `DevEnvironment.production` for release.
{% endhint %}

#### Dynamically switch the chain info:

{% tabs %}
{% tab title="Swift" %}
```swift
// Async switch chain info, it will check if user has logged in this chain name.
// For example, if a user logged in with ethereum, then switch to bsc,
// it will switch to bsc directly, beacuse both bsc and ethereum are evm,
// but if switch to solana, beacuse user didn't log in solana before, it will 
// present a web browser for additional information automatically.
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.kovan)
ParticleAuthService.setChainInfo(chainInfo).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        print(error)
    case .success(let userInfo):
        print(userInfo)
    }
}.disposed(by: bag)

// Sync switch chain info. it will wont check if user has logged in.
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.kovan)
ParticleNetwork.setChainInfo(chainInfo)
```
{% endtab %}

{% tab title="Objective-C" %}

{% endtab %}
{% endtabs %}

## API Reference

### Login

To auth login with Particle, call `ParticleNetwork.login(...)`and `subscribe`. You can log in with your email or phone number by changing the `LoginType` parameter. Your wallet is created when you log in successfully for the first time.

{% tabs %}
{% tab title="Swift" %}
```swift
/// Login
/// - Parameters:
///   - type: Login type, support email, phone, google, apple, facebook and so on.
///   - account: When login type is email or phone, you could pass email address or phone number.
///   - supportAuthType: Controls whether third-party login buttons are displayed. default will show all third-party login buttons.
///   - loginFormMode: Controls whether show light UI in web, default is false. 
/// - Returns: User infomation single
ParticleAuthService.login(type: .email).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let userinfo):
        // login success, handle userinfo
    }
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
[ParticleAuthService loginWithType:LoginTypeEmail successHandler:^(UserInfo * userInfo) {
        NSLog(@"%@", userInfo);
        [self showLogin:NO];
    } failureHandler:^(NSError * error) {
        NSLog(@"%@", error);
    }];
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
After log-in success, you can obtain user info by calling `ParticleNetwork.getUserInfo()`
{% endhint %}

### Is User Login

{% tabs %}
{% tab title="Swift" %}
```swift
ParticleAuthService.isLogin()
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
[ParticleAuthService isLogin]
```
{% endtab %}
{% endtabs %}

### Logout

The SDK will delete users' account information in cache.

{% tabs %}
{% tab title="Swift" %}
```swift
ParticleAuthService.logout().subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let success):
        // logout success
    }
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
[ParticleAuthService logoutWithSuccessHandler:^(NSString * result) {
    NSLog(@"%@", result);
    [self showLogin:YES];
} failureHandler:^(NSError * error) {
    NSLog(@"%@", error);
}];
```
{% endtab %}
{% endtabs %}

### Get user info and address after login

{% tabs %}
{% tab title="Swift" %}
```swift
ParticleAuthService.getUserInfo()
ParticleAuthService.getAddress()
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
[ParticleAuthService getUserInfo];
[ParticleAuthService getAddress];
```
{% endtab %}
{% endtabs %}

### Custom modal present style

{% tabs %}
{% tab title="Swift" %}
```swift
// support fullScreen and formSheet
// default web broswer modal present style is formSheet
ParticleAuthService.setModalPresentStyle(.fullScreen)
```
{% endtab %}

{% tab title="Objective-C" %}

{% endtab %}
{% endtabs %}

### Signatures

Use the Particle SDK to sign a transaction or message. The SDK provides three methods for signing:

1. `signAndSendTransaction`: sign and send the transaction with Particle Node, then return the signature. This supports both Solana and EVM.
2. `signTransaction`: sign transaction, return signed message. This only supports Solana.
3. `signMessage`: sign message, return signed message. This supports both Solana and EVM.
4. `signTypedData`: sign typed data; this supports v1, v3, and v4 typed data. Return signed message; this only supports EVM.

{% tabs %}
{% tab title="Swift" %}
```swift
//transaction: request base58 string in solana, or hex string in evm
ParticleAuthService.signAndSendTransaction(transaction).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signature):
        // handle signature 
    }
}.disposed(by: bag)
        
//transaction: request base58 string in solana, not support evm
ParticleAuthService.signtransaction(transaction).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)

//transaction: request base58 string array in solana, not support evm
let transactions: [String] = []
ParticleAuthService.signAllTransactions(transactions).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let signature):
        print(signature)
    }
}.disposed(by: self.bag)

//sign string, any string in solana, request hex string in evm
ParticleAuthService.signMessage(message).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)

 
// sign typed data, request hex string in evm, not support solana.
// support v1, v3, v4 typed data
ParticleAuthService.signTypedData(message, version: .v1).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signed):
        // handle signed message
    }
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
//transaction: request base58 string in solana, or hex string in evm
[ParticleAuthService signAndSendTransaction:transaction successHandler:^(NSString * signature) {
    NSLog(@"%@", signature);
} failureHandler:^(NSError * error) {
    NSLog(@"%@", error);
}];

 //transaction: request base58 string in solana, not support evm     
[ParticleAuthService signTransaction:transaction successHandler:^(NSString * signedTransaction) {
    NSLog(@"%@", signedTransaction);
} failureHandler:^(NSError * error) {
    NSLog(@"%@", error);
}];

//sign string, any string in solana, request hex string in evm
[ParticleAuthService signMessage:message successHandler:^(NSString * signedMessage) {
    NSLog(@"%@", signedMessage);
} failureHandler:^(NSError * error) {
    NSLog(@"%@", error);
}];

// sign typed data, request hex string in evm, not support solana.
// support v1, v3, v4 typed data
[ParticleAuthService signMessage:message successHandler:^(NSString * signedMessage) {
    NSLog(@"%@", signedMessage);
} failureHandler:^(NSError * error) {
    NSLog(@"%@", error);
}];
```
{% endtab %}
{% endtabs %}

You can create a `transaction` with `TxData`. There's an easy way to do this with [Wallet Service](broken-reference).

### Error

`ParticleNetwork.Error` contains error details. The error will be logged in `debug` `DevEnvironment`.

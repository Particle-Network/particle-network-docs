# iOS

## Add Auth Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 14.1 or later
  * CocoaPods 1.11.0 or higher
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 14

### Create a Particle Project and App

Before you can add our Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](../../../getting-started/dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Auth Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Auth Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Auth Service's CocoaPods distribution requires Xcode 14.1 and CocoaPods 1.11.0 or higher. Here's how to install the Auth Service using CocoaPods:

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

### Edit Podfile

{% hint style="info" %}
#### It is required for every iOS project that integrates the Auth Service SDK.

```ruby
// paste there code into pod file
post_install do |installer|
installer.pods_project.targets.each do |target|
  target.build_configurations.each do |config|
  config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
    end
  end
 end
```
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

7\. Configure your app scheme URL, select your app from `TARGETS`,  under `Info` section, click + to add the `URL types`, and paste your scheme in `URL Schemes`

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)

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
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.mainnet)
ParticleAuthService.switchChain(chainInfo).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        print(error)
    case .success(let userInfo):
        print(userInfo)
    }
}.disposed(by: bag)

// Sync switch chain info. it will wont check if user has logged in.
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.mainnet)
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
///              When login type is jwt, you must pass json web token.
///   - supportAuthType: Controls whether third-party login buttons are displayed. default will show all third-party login buttons.
///   - loginFormMode: Controls whether show light UI in web, default is false. 
///   - socialLoginPrompt: Social login prompt.
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

/// Login with JWT
let account = "json web token"
ParticleAuthService.login(type: .jwt, account: account).subscribe { [weak self] result in
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

### Is user login

Check is user login locally

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

### Is login async

Check is user login from server side

```swift
ParticleAuthService.isLoginAsync().subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .success(let userInfo):
        print(userInfo)
    case .failure(let error):
        print(error)
    }
}.disposed(by: bag)
```

### Logout and FastLogout

The SDK will delete user's account information in cache.

{% tabs %}
{% tab title="Swift" %}
```swift
// This method presents a safari view controller, please keep the presented view controller alive,
// let safari view controller close automaticlly, then you will get the result.
ParticleAuthService.logout().subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let success):
        // logout success
    }
}.disposed(by: bag)

// This method do logout in a slient way.
ParticleAuthService.fastLogout().subscribe { [weak self] result in
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

### Get smart account

works on you enable [Account Abstraction](../../account-abstraction/ios.md)

```swift
ParticleAuthService.getSmartAccount()
```

### Custom modal present style

{% tabs %}
{% tab title="Swift" %}
```swift
// support fullScreen and formSheet
// default web broswer modal present style is formSheet
ParticleAuthService.setModalPresentStyle(.fullScreen)

// Set safari page show in medium screen, this method works from iOS 15
// default value is false.
// if you want to use medium screen, you must setModalPresentStyle fullScreen
ParticleAuthService.setMediumScreen(true)
```
{% endtab %}
{% endtabs %}

### Set appearance

{% tabs %}
{% tab title="Swift" %}
```swift
// set appearance, default value if follow system.
// such as set dark mode
ParticleNetwork.setAppearence(.dark)
```
{% endtab %}
{% endtabs %}

### Set web auth config

{% tabs %}
{% tab title="Swift" %}
```swift
// set display wallet when call sign and send transaction, and web page appearance.
ParticleAuthService.setWebAuthConfig(options: WebAuthConfig(isDisplayWallet: true, appearance: .unspecified))
```
{% endtab %}
{% endtabs %}

### Set language

```swift
// set English language, default value is English
ParticleNetwork.setLanguage(Language.en)
```

### Set FiatCoin

```swift
// set fiat coin symbol, default value is USD.
ParticleNetwork.FiatCoin(FiatCoin.USD)
```

### Signatures

Use the Particle SDK to sign a transaction or message. The SDK provides three methods for signing:

1. `signAndSendTransaction`: sign and send the transaction with Particle Node, then return the signature. This method supports both Solana and EVM.&#x20;
2. `signTransaction`: sign transaction, return signed message. This method only supports Solana.
3. `signMessage`: sign message, return signed message. This method supports both Solana and EVM.
4. `signMessageUnique`: sign message, return signed message, same message same signature. This method only support EVM.
5. `signTypedData`: sign typed data; this supports v1, v3, and v4 typed data. Return signed message; this method only supports EVM.

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

//sign string, any string in solana, hex string or any string in evm
ParticleAuthService.signMessage(message).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)

// sign string, hex string or any string in evm, same mssage same signature
ParticleAuthService.signMessageUnique(message).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)

 
// sign typed data, request hex string in evm, not support solana.
// support v1, v3, v4, v4Unique typed data
// v4Unique, same message same signature.
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
[ParticleAuthService signTypedData:message successHandler:^(NSString * signedMessage) {
    NSLog(@"%@", signedMessage);
} failureHandler:^(NSError * error) {
    NSLog(@"%@", error);
}];
```
{% endtab %}
{% endtabs %}

You can create a `transaction` with `TxData` and `FeeMarketEIP1559TxData` There's an easy way to do this with [Wallet Service](../../wallet-service/).

### Open web wallet

{% tabs %}
{% tab title="Swift" %}
```swift
// if user is login, it will open web wallet.
let styleJsonString = """
    {
      "supportAddToken": false,
      "supportChains": [{
          "id": 1,
          "name": "Ethereum"
        },
        {
          "id": 5,
          "name": "Ethereum"
        }
      ]
    }
"""
ParticleAuthService.openWebWallet(styleJsonString: styleJsonString) 
```
{% endtab %}
{% endtabs %}

### Open account and security

```swift
ParticleAuthService.openAccountAndSecurity()
```

### Set security account config

```swift
// set security account config, 
// promptSettingWhenSign default value is 1.
// promptMasterPasswordSettingWhenLogin default value is 0.
// 0 no prompt
// 1 first time show prompt
// 2 every time show prompt
// 3 force show prompt
ParticleAuthService.setSecurityAccountConfig(config: 
.init(promptSettingWhenSign: 1, promptMasterPasswordSettingWhenLogin: 2))
```

### CheckSum support

```swift
import ParticleNetworkBase
/// Ethereum address checksum, follow EIP55.
/// Don't use it with solana address.
/// - Returns: Checksum Address
func toChecksumAddress() -> String 

print("0x2648cfe97e33345300db8154670347b08643570b".toChecksumAddress())
```

### TRON network support

```swift
import ParticleNetworkBase

// convert tron base58 address to hex address
let tronAddressHex = TronFormatAddress.toHex("TDTduRew1o2ZqP9wiDPVEqdywMQdNC4hto")
print(tronAddressHex) // 0x2648cfe97e33345300db8154670347b08643570b

// convert hex address to tron base58 address
let tronAddressBase58 = TronFormatAddress.fromHex("0x2648cfe97e33345300db8154670347b08643570b")
print(tronAddressBase58) // TDTduRew1o2ZqP9wiDPVEqdywMQdNC4hto
```

### Particle Provider

```swift
// handle request from walle connect, could be used with ParticleWalletConnect
// support following methods:
// eth_sendTransaction, return signature or error
// eth_signTypedData, return signature or error
// personal_sign, return signature or error
// wallet_switchEthereumChain, return null or error
// eth_chainId, return chainId string like "0x5" or error
// eth_signTypedData_v1, return signature or error
// eth_signTypedData_v3, return signature or error
// eth_signTypedData_v4, return signature or error
public static func request(method: String, params: [Encodable]) -> Single<Data?>

// for example
ParticleProvider.request(method: method, params: params).subscribe { data in
   // handle data
} onFailure: { error in
    // handle error
}.disposed(by: bag)
```

### Error

`ParticleNetwork.Error` contains error details. The error will be logged in `debug` `DevEnvironment`.

## Particle Wallet Connect V2

Integrate your app as a wallet connect wallet. If you are using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

Add Particle Wallet Connect with CocoaPods

Starting from version 0.14.0, WalletConnectV2 is supported.

```ruby
pod 'ParticleWalletConnect', 0.14.0
```

## API Reference

### Initialize

```swift
// Initialize Particle Wallet Connect SDK, a wallet meta data
ParticleWalletConnect.initialize(
            WalletMetaData(name: "Particle Wallet",
            icon: URL(string: "https://connect.particle.network/icons/512.png")!,
            url: URL(string: "https://particle.network")!,
            description: "Particle Wallet Client"))
```

{% hint style="info" %}
## Migrating to WalletConnect v2

```swift
ParticleWalletConnect.setWalletConnectV2ProjectId("your wallet connect v2 project id")
```
{% endhint %}

### Connect

```swift
let pwc = ParticleWalletConnect()
// set delegate in your controller or view model.
pwc.delegate = self
// Connect to a wallet connect code
let wcCode = "wc:b47a7e47c4261edf2e6e8bf3860ff7795ec532fcfb53345e7337a129b6f74019@2?relay-protocol=irn&symKey=6a5310e7c98a3d358f8d1c11f205ef849f5b1f9430472363a2631edfbfb197c0"
do {
    try self.pwc.connect(code: wcCode)
} catch {
    print(error)
}
```

Handle connect result by implement ParticleWalletConnectDelegate protocol.

```swift
public protocol ParticleWalletConnectDelegate: AnyObject {
    /// Should connect dappMetaData, usually called after connect,
    /// you could save dappMetaData.topic for query session and manage session.
    /// - Parameters:
    ///   - dappMetaData: DappMetaData
    ///   - completion: Should return public address and chain id.
    func shouldConnectDapp(_ dappMetaData: DappMetaData, completion: @escaping (String, Int) -> Void)

    /// Handle request from dapp
    /// - Parameters:
    ///   - topic: Topic
    ///   - method: Method name
    ///   - params: Params
    ///   - completion: Result, encode to data
    func request(topic: String, method: String, params: [Encodable], completion: @escaping (WCResult<Data?>) -> Void)

    /// Did connect dapp
    /// - Parameter topic: Topic
    func didConnectDapp(_ topic: String)

    /// Did disconnect dapp
    /// - Parameter topic: Topic
    func didDisconnectDapp(_ topic: String)
} 
```

### Disconnect

```swift
// Disconnect wallet connect v2 session
Task {
    do {
        try await self.pwc.disconnect(topic: topic)
    } catch {
        print(error)
    }
}
```

### Get connected dapp&#x20;

```swift
/// Get dapp by topic
/// - Parameter topic: Topic
/// - Returns: Dapp meta data
func getDapp(by topic: String) -> DappMetaData?
```

### Get all connected dapps

```swift
 /// Get all wallet connect dapps. 
 /// - Parameter publicAddress: Public address, V2 need public address to filter dapp connections.
 /// - Returns: Dapp meta data array
 func getAllDapps(publicAddress: String) -> [DappMetaData]
```

### Update session

```swift
// updata wallet connect v2 public address and chain id.
do {
    try self.pwc.updateWalletConnect(dapp.topic, publicAddress: publicAddress, chainId: chainId)
} catch {
    print(error)
}
```

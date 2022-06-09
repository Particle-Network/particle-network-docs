# iOS

## Add Auth Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 12

### Create a Particle Project and App

Before you can add our Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://particle.network/#/login)

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
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

### Initialize Auth Service in your app <a href="#initialize-firebase" id="initialize-firebase"></a>

The final step is to add an initialization code to your application. You may have already done this as part of adding the Auth Service to your app. If you are using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

1. Create a  **ParticleNetwork-Info.plist** into the root of your Xcode project
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

3\. Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard

4\. Import the `ParticleNetwork` module in your `UIApplicationDelegate`

```swift
import ParticleNetwork     
```

5\. Initialize ParticleNetwork service, typically in your app's `application:didFinishLaunchingWithOptions:` method:

{% code title="swift" %}
```swift
let chainName = ParticleNetwork.ChainName.ethereum(.mainnet)
let devEnv = ParticleNetwork.DevEnvironment.debug
let config = ParticleNetworkConfiguration(chainName: chainName, devEnv: devEnv)
ParticleNetwork.initialize(config: config)
```
{% endcode %}

6\. Add the scheme URL handle in your app's `application(_:open:options:)` method

```swift
return ParticleNetwork.handleUrl(url)
```

7\. Config your app scheme URL, select your app target in the info section, click to add the URL type, and pass your scheme in URL Schemes

Your scheme URL should be "pn" + your project app uuid.

For example, if you project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](../../.gitbook/assets/image.png)

{% hint style="info" %}
You can dynamically switch the chainEnv by calling the `ParticleNetwork.setChainName()` method.&#x20;

devEnv needs to be modified to be `DevEnvironment.production` for release.
{% endhint %}

## API Reference

### Login

To auth login with Particle, call `ParticleNetwork.login(...)`and `subscribe`. You can log in with your email or phone number by changing the `LoginType` parameter. Your wallet is created when you log in successfully for the first time.

{% tabs %}
{% tab title="Swift" %}
```swift
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
{% endtabs %}

{% hint style="info" %}
After log-in success, you can obtain user info by calling `ParticleNetwork.getUserInfo()`
{% endhint %}

### Is User Logged In

Check that the user is logged in.

```swift
ParticleAuthService.isUserLoggedIn()
```

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
{% endtabs %}

### Get userinfo, publicKey, address after login.

{% tabs %}
{% tab title="Swift" %}
```swift
ParticleAuthService.getUserInfo()
ParticleAuthService.getPublicKey()
ParticleAuthService.getAddress()
```
{% endtab %}
{% endtabs %}

### Signatures

Use the Particle SDK to sign a transaction or message. The SDK provides three methods for signing:

1. `signAndSendTransaction`: sign and send the transaction with Particle Node, then return the signature. support both solana and evm.
2. `signTransaction`: sign transaction, return signed message, only support solana.
3. `signMessage`: sign message, return signed message, support both solana and evm.
4. `signTypedData`: sign typed data, support v1, v3, v4 typed data, return signed message, only support evm.

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
var message = ""
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
{% endtabs %}

### Error

`ParticleNetwork.Error` contains error details. The error will be logged in `debug` `DevEnvironment`.

# iOS

## Github Demo

[https://github.com/Particle-Network/particle-connect-ios](https://github.com/Particle-Network/particle-connect-ios)

## Summary

Modular Swift wallet adapters and components for EVM & Solana chains. Manage wallet and custom RPC request.

![Particle Connect](https://static.particle.network/docs-images/particle-connect.jpeg)

## Quick Start

### Add Connect Service to Your iOS Project

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

#### Create a Particle Project and App

Before you can add our Connect Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

#### Add the Connect Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Auth Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Auth Service's CocoaPods distribution requires Xcode 13.3.1 and CocoaPods 1.10.0 or higher. Here's how to install the Auth Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```ruby
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app:

```ruby
pod 'ParticleConnect'
pod 'ConnectWalletConnectAdapter'
pod 'ConnectEVMAdapter'
pod 'ConnectSolanaAdapter'
pod 'ConnectPhantomAdapter'
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

### Initialize Connect Service in your app <a href="#initialize-firebase" id="initialize-firebase"></a>

The final step is to add an initialization code to your application. You may have already done this as part of adding the Connect Service to your app. If you are using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

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
import ConnectCommon
import ConnectEVMAdapter
import ConnectPhantomAdapter
import ConnectSolanaAdapter
import ConnectWalletConnectAdapter
import ParticleNetworkBase
import ParticleConnect
```
{% endtab %}
{% endtabs %}

5\. Initialize the ParticleNetwork service, which is typically in your app's `application:didFinishLaunchingWithOptions:` method:

{% tabs %}
{% tab title="Swift" %}
```swift
 ParticleConnect.initialize(env: .debug, 
 chainInfo: .ethereum(.mainnet), 
 dAppData: DAppMetaData(name: "Test", 
 icon: URL(string: "https://static.particle.network/wallet-icons/Particle.png")!, 
 url: URL(string: "https://static.particle.network")!)) {
      [ParticleConnectAdapter(),
       WalletConnectAdapter(),
       MetaMaskConnectAdapter(),
       PhantomConnectAdapter(),
       EVMConnectAdapter(),
       SolanaConnectAdapter(),
       ImtokenConnectAdapter(),
       BitkeepConnectAdapter(),
       RainbowConnectAdapter()]
}
```
{% endtab %}
{% endtabs %}

6\. Add the scheme URL handle in your app's `application(_:open:options:)` method

{% tabs %}
{% tab title="Swift" %}
```swift
return ParticleConnect.handleUrl(url)
```
{% endtab %}
{% endtabs %}

7\. Configure your app scheme URL, select your app target in the info section, click to add the URL type, and pass your scheme in URL Schemes

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../.gitbook/assets/image (1).png>)

8\. Add LSApplicationQueriesSchemes to info.plist.

```
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>trust</string>
	<string>rainbow</string>
	<string>imtokenv2</string>
	<string>metamask</string>
	<string>phantom</string>
	<string>bitkeep</string>
</array>
```

####

### Switch chain.

```swift
ParticleConnect.setChain(chainInfo: .ethereum(.mainnet))
```

### Get all wallet adapters.

```swift
let adapters = ParticleConnect.getAdapters(chainType: .evm)
//or
let adapter = ParticleConnect.getAdapterByAddress(publicAddress: address)
```

### Get adapter accounts.

```swift
let accounts = adapter.getAccounts()
```

### Connect wallet.

(For `EVMConnectAdapter` or `SolanaConnectAdapter` will generate new wallet)

```swift
connectAdapter.connect().subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let account):
        print(account)
    }

```

### Disconnect wallet.

```swift
connectAdapter.disconnect(address).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let success):
        print(success)
    }

```

### Check whether the account is connected.

```swift
let result = connectAdapter.isConnected(address)
```

### Import wallet.

(Only `EVMConnectAdapter` and `SolanaConnectAdapter` support this method)

```swift
// import wallet with private key
connectAdapter.importWalletFromPrivateKey(privateKey).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let account):
        print(account)
    }
}

// import wallet with mnemonic(Split with space).
connectAdapter.importWalletFromMnemonic(mnemonic).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let account):
        print(account)
    }
}
```

### Export wallet.

(Only `EVMConnectAdapter` and `SolanaConnectAdapter` support this method)

```swift
connectAdapter.exportWalletPrivateKey(publicAddress: address).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let privateKey):
        print(privateKey)
    }
}
```

### Sign and send transaction.

```swift
// todo: check connected before sign
connectAdapter.signAndSendTransaction(publicAddress: address, transaction: transaction).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let signature):
        print(signature)
    }
}
```

### Sign transaction.

(Only Solana chain support this method)

```swift
connectAdapter.signTransaction(publicAddress: address, transaction: transaction).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let signed):
        print(signed)
    }
}
```

### Sign all transactions.

(Only Solana chain support this method)

```swift
connectAdapter.signAllTransactions(publicAddress: address, transactions: transactions).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let signed):
        print(signed)
    }
}
```

### Sign message.

(EVM call `personal_sign`)

```swift
connectAdapter.signMessage(publicAddress: address, message: message).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let signed):
        print(signed)
    }
}
```

### Sign typed data.

(Only EVM chains support this method)

```swift
connectAdapter.signTypedData(publicAddress: address, data: data).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
        case .failure(let error):
        print(error)
        case .success(let signed):
        print(signed)
    }
}
```

### Login

```swift
// We have full example in github demo.
let message = try! getSiweMessage()
self.siweMessage = message
print("message = \(message.description)")
adapter.login(config: message, publicAddress: getSender()).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        print(error)
        if let connectError = error as? ConnectError {
            self.resultLabel.text = "code = \(String(describing: connectError.code)), message = \(String(describing: connectError.message))"
        } else {
            self.resultLabel.text = error.localizedDescription
        }
    case .success(let (sourceMessage, signedMessage)):
        print("sourceMessage = \(sourceMessage), \n\nsignedMessage = \(signedMessage)")
        self.resultLabel.text = signedMessage
    }
}.disposed(by: bag)
```

### Verify locally

<pre class="language-swift"><code class="lang-swift">// We have full example in github demo.
<strong>guard let message = siweMessage else {
</strong>    return
}
let against = resultLabel.text ?? ""
adapter.verify(message: message, against: against).subscribe { [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        print(error)
        if let connectError = error as? ConnectError {
            self.resultLabel.text = "code = \(String(describing: connectError.code)), message = \(String(describing: connectError.message))"
        } else {
            self.resultLabel.text = error.localizedDescription
        }
    case .success(let flag):
        print(flag)
        self.resultLabel.text = flag ? "True" : "False"
    }
}.disposed(by: bag)</code></pre>

## Give Feedback

You can join our [Discord](https://discord.gg/2y44qr6CR2).
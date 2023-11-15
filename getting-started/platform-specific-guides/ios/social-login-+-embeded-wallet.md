# Social Login + Embeded Wallet

This guild will help you get started with Particle Auth iOS SDK by setting up your environment, creating a wallet with social account, and sending a EVM transaction. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* iOS 14 or higher
* Xcode 14.1 or higher
* CocoaPods 1.12.0 or higher

## Installation

Install ParticleAuthService SDK using Cocoapods.

```ruby
platform :ios, '14.0'
source 'https://github.com/CocoaPods/Specs.git'

target 'ParticleExample' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!
  pod 'ParticleNetworkBase', '1.3.5'
  pod 'ParticleAuthService', '1.3.5'
  pod 'ParticleWalletAPI', '1.3.5'
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
    config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
      end
    end
end

```

## Register your application

In order to use ParticleAuthService, you'll need to create a project in the [Particle Dashboard](../../dashboard/) and get your project id, client key and project app id.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

## Create and initialize the ParticleAuthService

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

Now, let's initialize ParticleAuthService in your project, chainInfo signifies the chain (ethereum, polygon ...) you want to initialize ParticleAuthService with.

```swift
import ParticleAuthService
import ParticleNetworkBase
import UIKit

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        particleInit()
        return true
    }

    func particleInit() {
        ParticleNetwork.initialize(config: .init(chainInfo: .ethereum(.mainnet), devEnv: .debug))
    }
    // handle login callback
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
        return ParticleAuthService.handleUrl(url)
    }
}
```

**All here, initialization is complete.**

## Triggering the login

Trigger login flow with email, phone number, google, facebook, twitter, JWT, etc.

ParticleAuthService.login requires LoginType as a required input

```swift
ParticleAuthService.login(type: .google).subscribe { result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let userInfo):
        print(userInfo)
    }
}.disposed(by: bag)
```

{% hint style="info" %}
If you are using authentication through JWT, you need pass jwt in login method.
{% endhint %}

```swift
ParticleAuthService.login(type: .jwt, account: "your jwt")
```

{% hint style="info" %}
If  you want to sign a message when login, you need set authorization parameter, then get signature from userInfo.
{% endhint %}

```swift
ParticleAuthService.login(type: .email, authorization: .init(message: "Hello Particle", isUnique: false))
```

## Triggering the sign

After login, if you want to authenticate by signature call `signMessage`.

```swift
ParticleAuthService.signMessage("Hello Particle!").subscribe { result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let signature):
        print(signature)
    }
}.disposed(by: bag)
```

Also you can try `signTypeData`.

If you want to send a EVM transaction, call `signAndSendTransaction`.

You can create a transaction by `ParticleWalletAPI.getEvmService().createTransaction` method.

```swift
let receiverAddress = "0x0000000000000000000000000000000000000000"
let senderAddress = ParticleAuthService.getAddress()
// the smallest unit
let amount = BInt(10000000000000).toHexString()
ParticleWalletAPI.getEvmService().createTransaction(from: senderAddress, to: receiverAddress, value: amount, data: "0x", gasFeeLevel: .high).flatMap { transaction in
    ParticleAuthService.signAndSendTransaction(transaction)
}.subscribe { result in
    switch result {
    case .success(let signature):
        print(signature)
    case .failure(let error):
        print(error)
    }

}.disposed(by: self.bag)
```

## Log the user out

Use the `fastLogout` function of ParticleAuthService to trigger the logout flow.&#x20;

```swift
ParticleAuthService.fastLogout().subscribe { result in
    switch result {
    case .failure(let error):
        print(error)
    case .success:
        print("success")
    }
}.disposed(by: bag)
```


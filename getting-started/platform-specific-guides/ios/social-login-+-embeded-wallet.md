# Social Login + Embeded Wallet

This guide will help you get started with Particle Connect iOS SDK by setting up your environment, creating a wallet with social account, and sending a EVM transaction. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* iOS 14 or higher
* Xcode 14.1 or higher
* CocoaPods 1.12.0 or higher

## Installation

Install ParticleConnectService SDK using Cocoapods.

```ruby
platform :ios, '14.0'
source 'https://github.com/CocoaPods/Specs.git'

target 'ParticleExample' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!
  pod 'ParticleNetworkBase', '1.3.5'
  pod 'ParticleAuthService', '1.3.5'
  pod 'ParticleWalletAPI', '1.3.5'

  pod 'ConnectCommon', '0.2.14'
  pod 'ParticleConnect', '0.2.14'
  pod 'ParticleAuthAdapter', '0.2.14'
  
  
  # The following SDKs are optional.
  # ConnectWalletConnectAdapter is WalletConnect SDK, support EVM.
  pod 'ConnectWalletConnectAdapter', '0.2.14'
  # ConnectEVMAdapter is EVM private SDK.
  pod 'ConnectEVMAdapter', '0.2.14'
  # ConnectSolanaAdapter is Solana Private SDK.
  pod 'ConnectSolanaAdapter', '0.2.14'
  # ConnectPhantomAdapter is Phantom Wallet SDK, support Solana.
  pod 'ConnectPhantomAdapter', '0.2.14'
  
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

In order to use ParticleConnectService, you'll need to create a project in the [Particle Dashboard](../../dashboard/) and get your project id, client key and project app id.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

## Create and initialize the ParticleConnectService

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

Now, let's initialize ParticleConnectService in your project, chainInfo signifies the chain (ethereum, polygon ...) you want to initialize ParticleConnectService with.

```swift
import ConnectCommon
import ConnectEVMAdapter
import ConnectSolanaAdapter
import ConnectPhantomAdapter
import ConnectWalletConnectAdapter
import ParticleConnect
import ParticleAuthAdapter
import UIKit
// if you want to use auth core service
// import AuthCoreAdapter

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.

        particleInit()

        return true
    }

    func particleInit() {
        let adapters: [ConnectAdapter] = [
            ParticleAuthAdapter(),
            MetaMaskConnectAdapter(),
            PhantomConnectAdapter(),
            WalletConnectAdapter(),
            RainbowConnectAdapter(),
            BitkeepConnectAdapter(),
            ImtokenConnectAdapter(),
            TrustConnectAdapter(),
            ZerionConnectAdapter(),
            MathConnectAdapter(),
            OmniConnectAdapter(),
            Inch1ConnectAdapter(),
            ZengoConnectAdapter(),
            AlphaConnectAdapter(),
            OKXConnectAdapter(), 
            SolanaConnectAdapter(),
            EVMConnectAdapter()
            // if you want to use auth core service
            // add AuthCoreAdapter()
        ]
        ParticleConnect.initialize(env: .debug, chainInfo: .polygon(.mumbai)) {
            adapters
        }
        ParticleConnect.setWalletConnectV2ProjectId("75ac08814504606fc06126541ace9df6")

        // Set wallet connect chains,
        // Note metamask only support one chain for each connection.
//        ParticleConnect.setWalletConnectV2SupportChainInfos([.ethereum(.mainnet), .ethereum(.goerli), .polygon(.mainnet), .polygon(.mumbai)])
    }

    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
        return ParticleConnect.handleUrl(url)
    }
}

```

{% hint style="info" %}
If you don't want to support private key connect and wallet connect, you can remove their pods in Podfile, then remove their adapters when initializing, keep ParticleAuthAdapter or AuthCoreAdapter
{% endhint %}

**All here, initialization is complete.**

## Triggering the login

Trigger login flow with email, phone number, google, facebook, twitter, JWT, etc.

Retrieve the adapter with the type 'particle' from all the adapters you have registered so far, call connect with a parameter ParticleAuthConfig.

You need to save the returned Account object for convenient use next time.

```swift
@IBAction func connectParticle() {
    let adapter = ParticleConnect.getAllAdapters().filter {
        $0.walletType == .particle
    }.first!

    let authConfig: ParticleAuthConfig = .init(loginType: .google)
    adapter.connect(authConfig).subscribe { [weak self] result in
        guard let self = self else { return }
        switch result {
        case .success(let account):
            self.account = account
            print(account)
        case .failure(let error):
            print(error)
        }
    }.disposed(by: self.bag)
}
```

{% hint style="info" %}
If you are using authentication through JWT, you need pass JWT in the ParticleAuthConfig object.
{% endhint %}

```swift
let authConfig: ParticleAuthConfig = .init(loginType: .jwt, phoneOrEmailAccount: "your jwt")
```

{% hint style="info" %}
If  you want to sign a message when connect, you need set authorization parameter, then get signature from userInfo.
{% endhint %}

```swift
let authConfig: ParticleAuthConfig = .init(loginType: .apple, authorization: .init(message: "Hello Particle!", isUnique: false))
```

```swift
// After connect a particle account
ParticleAuthService.getUserInfo()?.signature
// also you can get token from userInfo.
```

Particle Connect iOS SDK also support connect with other wallets, here is an example show how to connect with MetaMask wallet, other wallets are in the same way.

```swift
@IBAction func connectMetaMask() {
    let adapter = ParticleConnect.getAllAdapters().filter {
        // specify the wallet that you want.
        $0.walletType == .metaMask
    }.first!

    adapter.connect(ConnectConfig.none).subscribe { [weak self] result in
        guard let self = self else { return }
        switch result {
        case .success(let account):
            self.account = account
            print(account)
        case .failure(let error):
            print(error)
        }
    }.disposed(by: self.bag)
}
```

## Triggering the sign

After connect, if you want to authenticate by signature call `signMessage`.

```swift
@IBAction func signMessage() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }

    let adapter = ParticleConnect.getAllAdapters().filter {
        $0.walletType == account.walletType
    }.first!

    let publicAddress = account.publicAddress

    adapter.signMessage(publicAddress: publicAddress, message: "Hello Particle!").subscribe {
        [weak self] result in
            guard let self = self else { return }
            switch result {
            case .success(let signature):
                print(signature)
            case .failure(let error):
                print(error)
            }
    }.disposed(by: self.bag)
}
```

If you want to send a EVM transaction, call `signAndSendTransaction`.

You can create a transaction by `ParticleWalletAPI.getEvmService().createTransaction` method.

```swift
@IBAction func sendTransaciton() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }

    let adapter = ParticleConnect.getAllAdapters().filter {
        $0.walletType == account.walletType
    }.first!

    let publicAddress = account.publicAddress

    let receiverAddress = "0x0000000000000000000000000000000000000000"

    // the smallest unit
    let amount = BInt(10000000000000).toHexString()
    ParticleWalletAPI.getEvmService().createTransaction(from: publicAddress, to: receiverAddress, value: amount, data: "0x", gasFeeLevel: .high).flatMap { transaction in
        adapter.signAndSendTransaction(publicAddress: publicAddress, transaction: transaction)
    }.subscribe { result in
        switch result {
        case .success(let signature):
            print(signature)
        case .failure(let error):
            print(error)
        }

    }.disposed(by: self.bag)
}
```

{% hint style="info" %}
Then you can try `signTypeData in the same way.`
{% endhint %}

## Log the user out

Use the `disconnect` function of ParticleConnectService to trigger the logout flow.&#x20;

```swift
 @IBAction func disconnect() {
        guard let account = self.account else {
            print("you didn't connect any account")
            return
        }

        let adapter = ParticleConnect.getAllAdapters().filter {
            $0.walletType == account.walletType
        }.first!

        let publicAddress = account.publicAddress

        adapter.disconnect(publicAddress: publicAddress).subscribe {
            [weak self] result in
                guard let self = self else { return }
                switch result {
                case .success(let flag):
                    print(flag)
                    self.account = nil
                case .failure(let error):
                    print(error)
                }
        }.disposed(by: self.bag)
    }
```

## Dive Deeper

In this guide, we learned how to use the Particle Connect iOS SDK for social login, sending transactions, and signing message. If you want to learn more about different use cases, check out the [Particle Connect](../../../developers/connect-service/sdks/ios.md) page.

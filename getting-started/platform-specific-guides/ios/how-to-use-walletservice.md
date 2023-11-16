# How to use WalletService

This guild will help you get started with Particle Wallet iOS SDK by setting up your environment, based on the previous guides, should now understand how to create a Wallet,  our Particle Wallet iOS SDK offers common wallet pages, quick sending, viewing NFTs, viewing transaction history, and other features.

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
  pod 'ParticleAA', '1.3.5'
  pod 'ParticleWalletGUI', '1.3.5'
  pod 'ParticleWalletConnect', '1.3.5'

  pod 'ConnectCommon', '0.2.14'
  pod 'ConnectEVMAdapter', '0.2.14'
  pod 'ConnectSolanaAdapter', '0.2.14'
  pod 'ConnectWalletConnectAdapter', '0.2.14'
  pod 'ConnectPhantomAdapter', '0.2.14'
  pod 'ParticleConnect', '0.2.14'
  pod 'ParticleAuthAdapter', '0.2.14'
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

In order to use ParticleWalletService, you'll need to create a project in the [Particle Dashboard](../../dashboard/) and get your project id, client key and project app id.

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
import ConnectPhantomAdapter
import ConnectWalletConnectAdapter
import ParticleAA
import ParticleAuthAdapter
import ParticleConnect
import ParticleNetworkBase
import ParticleWalletConnect
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
        let adapters: [ConnectAdapter] = [
            MetaMaskConnectAdapter(),
            ParticleAuthAdapter(),
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
        ]
        ParticleConnect.initialize(env: .debug, chainInfo: .polygon(.mumbai)) {
            adapters
        }
        ParticleConnect.setWalletConnectV2ProjectId("your wallet connect project id")

        // set your biconomy api keys
        let biconomyApiKeys =
            [80001: "hYZIwIsf2.e18c790b-cafb-4c4e-a438-0289fc25dba1"]

        //
        AAService.initialize(name: .biconomy, version: .v1_0_0, biconomyApiKeys: biconomyApiKeys)
        let aaService = AAService()
        ParticleNetwork.setAAService(aaService)
        aaService.enableAAMode()
        // Set wallet connect chains,
        // Note metamask only support one chain for each connection.
//        ParticleConnect.setWalletConnectV2SupportChainInfos([.ethereum(.mainnet), .ethereum(.goerli), .polygon(.mainnet), .polygon(.mumbai)])

        ParticleWalletConnect.initialize(
            WalletMetaData(name: "Particle Wallet",
                           icon: URL(string: "https://connect.particle.network/icons/512.png")!,
                           url: URL(string: "https://particle.network")!,
                           description: "Particle Connect is a decentralized wallet connection protocol that makes it easy for users to connect their wallets to your DApp."))

        ParticleWalletConnect.setWalletConnectV2ProjectId("your wallet connect project id")
    }

    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
        return ParticleConnect.handleUrl(url)
    }
}
```

**All here, initialization is complete.**

You should connect a wallet first then do

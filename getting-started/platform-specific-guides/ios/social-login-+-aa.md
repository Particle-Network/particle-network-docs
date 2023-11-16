# Social Login + AA

This guild will help you get started with Particle Connect and Particle AA iOS SDK by setting up your environment, creating a wallet with social account, and sending a User Operation from it. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* iOS 14 or higher
* Xcode 14.1 or higher
* CocoaPods 1.12.0 or higher

## Installation

Install ParticleConnectService and ParticleAA SDK using Cocoapods.

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
import ConnectPhantomAdapter
import ConnectWalletConnectAdapter
import ParticleAA
import ParticleAuthAdapter
import ParticleConnect
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
        ParticleConnect.setWalletConnectV2ProjectId("75ac08814504606fc06126541ace9df6")

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
    }

    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
        return ParticleConnect.handleUrl(url)
    }
}
```

**All here, initialization is complete.**

## Triggering the login

Trigger login flow with email, phone number, google, facebook, twitter, JWT, etc.

Retrieve the adapter with the type 'particle' from all the adapters you have registered so far, call connect with a parameter ParticleAuthConfig.

You need to save the returned Account object for convenient use next time.

Due to initialize AA service before, you can get a smart account address from account object.

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
            print(account?.smartAccount?.smartAccountAddress)
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
            print(account?.smartAccount?.smartAccountAddress)
        case .failure(let error):
            print(error)
        }
    }.disposed(by: self.bag)
}
```

## Triggering the sign

After connect, if you want to send a EVM transaction, call `signAndSendTransaction`.

You can create a transaction by `ParticleWalletAPI.getEvmService().createTransaction` method.

```swift
@IBAction func sendTransaciton() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }
    guard let smartAccountAddress = account.smartAccount?.smartAccountAddress else {
        print("you didn't get a smart account address")
        return
    }

    let adapter = ParticleConnect.getAllAdapters().filter {
        $0.walletType == account.walletType
    }.first!

    let publicAddress = account.publicAddress

    let receiverAddress = "0x0000000000000000000000000000000000000000"

    // the smallest unit
    let amount = BInt(10000000000000).toHexString()

    ParticleWalletAPI.getEvmService().createTransaction(from: smartAccountAddress, to: receiverAddress, value: amount, data: "0x", gasFeeLevel: .high).flatMap { transaction in
        adapter.signAndSendTransaction(publicAddress: publicAddress, transaction: transaction, feeMode: .native, chainInfo: ParticleNetwork.getChainInfo())
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

Note the parameter `feeMode` in `signAndSendTransaction` method, it has three options, native, gasless and token, just as its name implies, it means how to pay gas fee.

Now I will show you how to pay token as gas fee.

Use `aaService.rpcGetFeeQuotes` get the wholeFeeQuote for your transactions, then find a available token to pay.

```swift
@IBAction func sendTransacitonPayToken() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }
    guard let smartAccountAddress = account.smartAccount?.smartAccountAddress else {
        print("you didn't get a smart account address")
        return
    }

    let adapter = ParticleConnect.getAllAdapters().filter {
        $0.walletType == account.walletType
    }.first!

    let publicAddress = account.publicAddress

    let receiverAddress = "0x0000000000000000000000000000000000000000"

    // the smallest unit
    let amount = BInt(10000000000000).toHexString()

    ParticleWalletAPI.getEvmService().createTransaction(from: smartAccountAddress, to: receiverAddress, value: amount, data: "0x", gasFeeLevel: .high).flatMap { transaction -> PrimitiveSequence<SingleTrait, (AA.WholeFeeQuote, String)> in
        let feeQuoteObservable = ParticleNetwork.getAAService()!.rpcGetFeeQuotes(eoaAddress: publicAddress, transactions: [transaction], chainInfo: ParticleNetwork.getChainInfo())

        return Single.zip(feeQuoteObservable, .just(transaction)) as PrimitiveSequence<SingleTrait, (AA.WholeFeeQuote, String)>
    }.flatMap { wholeFeeQuote, transaction -> Single<String> in
        guard let tokenPaymaster = wholeFeeQuote.token else {
            return .error(ParticleNetwork.ResponseError(code: nil, message: "pay token is unavailable"))
        }
        let tokenPaymasterAddress = tokenPaymaster.tokenPaymasterAddress
        let tokenFeeQuoters = tokenPaymaster.feeQuotes
        let feeQuotes = tokenFeeQuoters.map {
            AA.FeeQuote(json: $0, tokenPaymasterAddress: tokenPaymasterAddress)
        }
        // select a token, which isEnoughForPay is true
        let feeQuote = feeQuotes.first {
            $0.isEnoughForPay
        }

        if feeQuote == nil {
            return .error(ParticleNetwork.ResponseError(code: nil, message: "pay token is unavailable"))
        }

        return adapter.signAndSendTransaction(publicAddress: publicAddress, transaction: transaction, feeMode: .token(feeQuote!), chainInfo: ParticleNetwork.getChainInfo())
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

## Batch Send transactions

If you want to send multi transactions in one sign, you should try this way.

Combine transactions into a array, then call `aaService.quickSendTransactions` , the parameter \`feeMode\` is the same as before.

```swift
@IBAction func batchSendTransactions() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }
    guard let smartAccountAddress = account.smartAccount?.smartAccountAddress else {
        print("you didn't get a smart account address")
        return
    }

    let receiverAddress = "0x0000000000000000000000000000000000000000"
    // the smallest unit
    let amount = BInt(10000000000000).toHexString()

    // make two transactions
    let createTransactionObservable1 = ParticleWalletAPI.getEvmService().createTransaction(from: smartAccountAddress, to: receiverAddress, value: amount, data: "0x", gasFeeLevel: .high)
    let createTransactionObservable2 = ParticleWalletAPI.getEvmService().createTransaction(from: smartAccountAddress, to: receiverAddress, value: amount, data: "0x", gasFeeLevel: .high)

    Single.zip(createTransactionObservable1, createTransactionObservable2).flatMap { [weak self] transaction1, transaction2 -> Single<String> in
        guard let self = self else { return .error(ParticleNetwork.ResponseError(code: nil, message: "self is nil")) }
        return ParticleNetwork.getAAService()!.quickSendTransactions([transaction1, transaction2], feeMode: .native, messageSigner: self, wholeFeeQuote: nil, chainInfo: ParticleNetwork.getChainInfo())
    }.subscribe {
        result in
        switch result {
        case .success(let signature):
            print(signature)
        case .failure(let error):
            print(error)
        }
    }.disposed(by: self.bag)
}

extension ViewController: MessageSigner {
    func signMessage(_ message: String, chainInfo: ParticleNetworkBase.ParticleNetwork.ChainInfo?) -> RxSwift.Single<String> {
        guard let account = self.account else {
            print("you didn't connect any account")
            return .error(ParticleNetwork.ResponseError(code: nil, message: "you didn't connect any account"))
        }

        guard let smartAccountAddress = account.smartAccount?.smartAccountAddress else {
            print("you didn't get a smart account address")
            return .error(ParticleNetwork.ResponseError(code: nil, message: "you didn't get a smart account address"))
        }

        let adapter = ParticleConnect.getAllAdapters().filter {
            $0.walletType == account.walletType
        }.first!

        let publicAddress = account.publicAddress

        return adapter.signMessage(publicAddress: publicAddress, message: message)
    }

    func getEoaAddress() -> String {
        self.account?.publicAddress ?? ""
    }
}
```

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

In this guide, we learned how to use the Particle Auth iOS SDK for social login, sending transactions, and signing message. If you want to learn more about different use cases, check out the [Account Abstraction](../../../developers/account-abstraction/ios.md) page.

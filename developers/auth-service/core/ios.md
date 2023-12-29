# iOS

## Add Auth Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 14.1 or later
  * CocoaPods 1.12.1 or higher
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 14
* **Please note that the current version of our software does not support simulation testing. To perform testing, you will require an actual iPhone device.**

### Create a Particle Project and App

Before you can add our Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](../../../getting-started/dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Auth Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Auth Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Here's how to install the Auth Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```ruby
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app:

```ruby
pod 'ParticleAuthCore'
pod 'ParticleMPCCore'
pod 'Thresh'
```

3\. Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

```ruby
pod install --repo-update
```

```ruby
open your-project.xcworkspace
```

### Edit Podfile

{% hint style="info" %}
**It is required for every iOS project that integrates the Auth Service SDK.**

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

The final step is to add an initialization code to your application. You may have already done this as part of adding the Auth Service to your app.

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

3. Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard
4. Import the `ParticleNetwork` module in your `UIApplicationDelegate`

{% tabs %}
{% tab title="Swift" %}
```swift
import ParticleNetworkBase 
```
{% endtab %}
{% endtabs %}

5. Initialize the ParticleNetwork service, which is typically in your app's `application:didFinishLaunchingWithOptions:` method:

{% tabs %}
{% tab title="Swift" %}
<pre class="language-swift"><code class="lang-swift">// select a network from ChainInfo.
let chainInfo = ParticleNetwork.ChainInfo.ethereum
let devEnv = ParticleNetwork.DevEnvironment.debug
let config = ParticleNetworkConfiguration(chainInfo: <a data-footnote-ref href="#user-content-fn-1">chainInfo</a>, devEnv: devEnv)
ParticleNetwork.initialize(config: config)
</code></pre>
{% endtab %}
{% endtabs %}

6. Our SDK requires the 'Privacy - Face ID Usage Description' permission in order to function correctly. This is necessary because our SDK utilizes Face ID for secure user authentication. To request this permission, you must include the `NSFaceIDUsageDescription` key in your app's `Info.plist` file, accompanied by a string value explaining the reason for the request. The text you provide is presented to the user when your app first attempts to use Face ID. Here's an example:

```markup
<key>NSFaceIDUsageDescription</key>
<string>Your descriptive reason here...</string>
```

{% hint style="info" %}
### Together with Wallet Service

If you want to use with [Wallet Service](../../wallet-service/sdks/ios.md), you should add more pods in Podfile.

```ruby
pod 'AuthCoreAdapter'
```

Initialize [Particle Connect Service](../../connect-service/sdks/ios.md), add AuthCoreAdaper to your adapters.
{% endhint %}

## API Reference

### Get an AuthCore object

```swift
import ParticleAuthCore

let auth = Auth()
```

### Login

Login Particle with JWT.

{% tabs %}
{% tab title="Swift" %}
```swift
let userInfo = try await auth.connect(jwt: jwt)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Account Abstraction could use together with Auth Core Service, explore [Account Abstraction](../../account-abstraction/ios.md) for more detail, learn how to get a smart account address, how to send transaction under AA mode.
{% endhint %}

{% hint style="info" %}
Wallet Service could use together with Auth Core Service, explore [Wallet Service](../../wallet-service/sdks/ios.md) for more detail, learn how to open wallet page, how to open send page, how to open swap page etc.
{% endhint %}

### Get user info

```swift
let userInfo = auth.getUserInfo()
```

### Logout

The SDK will delete users' account information in cache.

{% tabs %}
{% tab title="Swift" %}
```swift
let result = try await auth.disconnect()
```
{% endtab %}
{% endtabs %}

### Check user login status

{% tabs %}
{% tab title="Swift" %}
```kotlin
let result = try await auth.isConnected()
```
{% endtab %}
{% endtabs %}

### Get Wallet Address

```swift
let evmAddress = auth.evm.getAddress()
let solanaAddress = auth.solana.getAddress()
```

### Switch ChainInfo

{% tabs %}
{% tab title="Swift" %}
```kotlin
let result = try await auth.switchChain(chainInfo: .bsc(.mainnet))
```
{% endtab %}
{% endtabs %}

### Signatures

Use Auth Core SDK to sign a transaction or message.

{% tabs %}
{% tab title="EVM" %}
```swift
// personal sign
let result = try await auth.evm.personalSign(messageHex)
    
// personal sign unique
let result = try await auth.evm.personalSignUnique(messageHex)

// sign typed data
let result = try await auth.evm.signTypedData(typedDataV4)

// sign typed data unique
let result = try await auth.evm.signTypedDataUnique(typedDataV4)

// send evm transaction
let result = try await auth.evm.sendTransaction(transaction)

// request public rpc
let method = "eth_getBalance"
let parameters = ["your evm public address", "latest"]
let result = try await auth.evm.request(method: method, parameters: parameters)
```
{% endtab %}

{% tab title="Solana" %}
```swift
// sign message
let result = try await auth.solana.signMessage(message)

// sign transaction
let result = try await auth.solana.signTransaction(transaction)

// sign all transactions
let result = try await auth.solana.signAllTransactions(transactions)

// sign and send transaction
let result = try await auth.solana.signAndSendTransaction(transaction)

// request public rpc
let method = "getBalance"
let parameters = ["your solana public address"]
let result = try await auth.solana.request(method: method, parameters: parameters)
```
{% endtab %}
{% endtabs %}

### Master Password

Wallet can set master password to protect assets.

```swift
// check user has master password or not
let result = try auth.hasMasterPassword()

// set or change master password
let result = try await auth.changeMasterPassword()
```

### Payment Password

If set a payment password, user should input password before sign message and transaction.

```swift
// check user has payment password or not
let result = try auth.hasPaymentPassword()
```

### Open account and security

Bind more login account, manage payment password etc.

```swift
try await auth.openAccountAndSecurity()
```

### Set security account config

```swift
// set security account config, 
// promptSettingWhenSign default value is 1.
// promptMasterPasswordSettingWhenLogin default value is 0.
// 0 no prompt
// 1 first time show prompt
// 2 every time show prompt
// 3 force show prompt and user must set payment password or master password
 ParticleNetwork.setSecurityAccountConfig(
    SecurityAccountConfig(
        promptSettingWhenSign = 1,
        promptMasterPasswordSettingWhenLogin = 2
    )
)
```

### Custom interface style

```swift
// this is the default setting
ParticleNetwork.setAppearence(.unspecified)
// dark 
ParticleNetwork.setAppearence(.dark)
// light
ParticleNetwork.setAppearence(.light)
```

### Language setting

```swift
// Support en, ja, ko, zh_hans and zh_hant.
ParticleNetwork.setLanguage(.ja)
        
let language = ParticleNetwork.getLanguage()
```

### Set custom UI json string

```swift
// Set custom ui is pass json string
let jsonString = "your custom ui json string, keys should be the same with customUIConfig.json in demo"
Auth.loadCustomUIJsonString(jsonString)
```

### Error

Try cast error into type `ParticleNetwork.Response`, you can check the information by printing its attributes.

[^1]: 

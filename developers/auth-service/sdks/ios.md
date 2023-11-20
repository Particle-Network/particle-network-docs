# iOS

## Adding the Auth Service SDK to Your App

Auth Service simplifies the integration of authentication system within your applications and supports installation with CocoaPods.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode version 14.1 or later.
  * CocoaPods version 1.12.0 or higher.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 14

### Create a Particle Project and App

Before you can add our Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](../../../getting-started/dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Add the Auth Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Follow these steps to install the Auth Service using [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

Here's how to install the Auth Service using CocoaPods:

1. **Create a Podfile**: If you do not already have a Podfile, you will need to create one. Navigate to the route of your project directory and run the following command in your terminal:

```ruby
pod init
```

2. **Add the Auth Service Pods**: To use the Auth Service in your app, you will need to add the respective pods to your Podfile: In your Podfile, add the following lines:

```ruby
pod 'ParticleAuthService'
```

3. **Install the Pods**: With the Auth Service pods added, you can now install the pods. Run the following command in your terminal:

```ruby
pod install --repo-update
```

4. **Open Your Project in Xcode**: After installing the pods, you need to open your .xcworkspace file to view the project in Xcode. Run the following command in your terminal:

```
open your-project.xcworkspace
```

{% hint style="info" %}
If you would like to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

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

### Initialize Auth Service in Your App

After adding the Auth Service to your app, the next step is to initialize it by following the steps described below. Please note, that if you are using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), initialization might have been done for you.

### Steps

**1. Create a `ParticleNetwork-Info.plist` File**

In the root of your Xcode project directory, create a file called `ParticleNetwork-Info.plist`.

**2. Add Content to Plist File**

Copy and paste the following text into the `ParticleNetwork-Info.plist` file:

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

Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the appropriate values generated from your Dashboard.

**3. Import ParticleNetwork Modules**

In your UIApplicationDelegate, import the ParticleNetworkBase and ParticleAuthService modules as such:

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

**4. Initialize the ParticleNetwork Service**

Typically, this is done in your app's `application:didFinishLaunchingWithOptions:` method.

Here is an example of how you can select a network from ChainInfo:

{% tabs %}
{% tab title="Swift" %}
```swift
// select a network from ChainInfo.
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.mainnet)
let devEnv = ParticleNetwork.DevEnvironment.debug
let config = ParticleNetworkConfiguration(chainInfo: chainInfo, devEnv: devEnv)
ParticleNetwork.initialize(config: config)
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
// select a network from ChainName enum.
ChainInfo *chainInfo = [ChainInfo ethereum:EthereumNetworkMainnet];
DevEnvironment devEnv = DevEnvironmentDebug;
ParticleNetworkConfiguration *config = [[ParticleNetworkConfiguration alloc] initWithChainInfo:chainInfo devEnv:devEnv];
[ParticleNetwork initializeWithConfig:config];
```
{% endtab %}
{% endtabs %}

#### 5. Add the Scheme URL Handle

In your app's `application(_:open:options:)` method, you need to return a call to the `handleUrl` method of the `ParticleAuthService`class:

{% tabs %}
{% tab title="Swift" %}
```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return ParticleAuthService.handleUrl(url)
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
    return [ParticleAuthService handleUrl:url];
}
```
{% endtab %}
{% endtabs %}

#### 6. Configuring Your App's Scheme URL:

To do this, please follow the steps below:

* Select your app from TARGETS in Xcode.
* Go to the "Info" section and click on the '+' button to add a new entry to the "URL types" field.
* Under "URL Schemes", paste your specific scheme information.

Your scheme URL should be `"pn"` followed by your project app id.

For instance, if your project app id is `"63bfa427-cf5f-4742-9ff1-e8f5a1b9828f"`, your scheme URL should be `"pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f"`.

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)

Congratulations! You have successfully initialized Auth Service in your app. Now you can make direct calls to the Auth Service SDK to manage user authentication. Remember to refer to the related documentation and API references to understand the scope and capability of the SDK.

{% hint style="info" %}
devEnv needs to be modified to be `DevEnvironment.production` for release.
{% endhint %}

### Dynamically Switching the Chain Info

The Auth Service SDK allows you to switch your blockchain network dynamically. The network can be altered either asynchronously or synchronously based on the function you decide to use. Follow the steps below:

#### Asynchronously

This function checks whether the user has previously logged into the desired chain name and automatically presents additional steps if needed. If the user has logged into the Ethereum chain and wants to switch to the BSC chain, the function switches directly since they're both EVM networks. If the user wants to change to a network they have not logged in before, i.e., Solana, the SDK will show a browser request for further login steps:

```swift
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
```

#### Synchronously

This function changes the network without checking for previous logins. It should be primarily used only if you are sure that the user has logged into the requested network previously:

```swift
let chainInfo = ParticleNetwork.ChainInfo.ethereum(.mainnet)
ParticleNetwork.setChainInfo(chainInfo)
```

By calling these methods based on the status of user activity in different chains can provide a smooth transition while switching chains.

## API Reference

Here is the documentation for several key API methods provided by the Auth Service SDK:

### Login

You can authenticate users in your app by using the `ParticleAuthService.login` function. This function supports different login types such as email, phone, Google, Apple, and Facebook. When the login is successful, a user wallet is created.

#### Prototype

```swift
ParticleAuthService.login(
    type: LoginType, 
    account: String? = nil, 
    supportAuthType: [SupportAuthType] = [SupportAuthType.all], 
    socialLoginPrompt: SocialLoginPrompt? = nil, 
    authorization: LoginAuthorization? = nil
)
```

#### Parameters

* `type`: Specifies the login type (e.g., email, phone, jwt, google, apple, facebook).
* `account`: Optional parameter for email, phone, or jwt login methods. You should pass in the user's email address, phone number, or jwt token here.
* `supportAuthType`: Controls whether third-party login buttons are displayed. By default, all third-party login buttons are shown.
* `socialLoginPrompt`: Social login prompt.
* `authorization`: Optional parameter for LoginAuthorization object.

#### Returns

The function returns a user information object (`userinfo`) if the login is successful.

#### Example usage

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

{% hint style="info" %}
After log-in success, you can obtain user info by calling `ParticleNetwork.getUserInfo()`
{% endhint %}

### Is User Login

You can check whether a user is logged in or not using the `ParticleAuthService.isLogin` function.

#### Prototype

```swift
ParticleAuthService.isLogin()
```

#### Returns

The function returns a boolean indicating whether a user is logged in (`true`) or not (`false`).

### Is User Login Async

#### Prototype

```swift
ParticleAuthService.isLoginAsync()
```

This function asynchronously checks from the server if a user is logged in. This ensures your local login status is up-to-date with the server status.

#### Example usage

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

#### Logout

The `logout` function enables users to log out from their account. The function deletes user account information from cache.

**Prototype**

```swift
ParticleAuthService.logout()
```

**Returns**

The function returns a success message if the logout process is completed successfully.

**Example usage**

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

#### FastLogout

The `fastLogout` function is similar to the `logout` method but performs the logout operation silently without presenting the user with a safari view controller.

**Prototype**

```swift
ParticleAuthService.fastLogout()
```

**Returns**

The function returns a success message if the fast logout process is completed successfully.

**Example usage**

```swift
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

### Get User Info and Address After Login

These functions enable you to retrieve user information and address after a successful login.

**Example**

```swift
ParticleAuthService.getUserInfo()
ParticleAuthService.getAddress()
```

**Returns**

`getUserInfo` returns a user information object. `getAddress` returns a string containing the user's address.

### Get Smart Account

Enables retrieval of the user's smart account. This function only works if you enable [Account Abstraction](../../account-abstraction/ios.md).

**Example**

```swift
ParticleAuthService.getSmartAccount()
```

**Returns**

The function returns a smart account object.

### Custom Modal Present Style

This function allows you to set the modal presentation style for the web browser.

**Example**

```swift
ParticleAuthService.setModalPresentStyle(.fullScreen)
ParticleAuthService.setMediumScreen(true)
```

**Details**

The default presentation style for the web browser is `.formSheet`. You can change it to `.fullScreen` by using the `setModalPresentStyle` function.

Starting from iOS 15, you can set the web browser to display in medium screen by using the `setMediumScreen` function. Before using it, make sure you have set the modal presentation style to `fullScreen`.

### Set and Get Appearance

You can configure the User Interface appearance of your app with the `setAppearance` and `getAppearance` functions. The default value is set to follow the system's appearance.

**Example**

```swift
ParticleNetwork.setAppearence(.dark)
let appearance = ParticleNetwork.getAppearence()
```

### Set Web Auth Config

You can configure the behavior and appearance of the authentication webpage with the `setWebAuthConfig` function. For instance, you can make the wallet visible when signing and sending transactions.

**Example**

```swift
ParticleAuthService.setWebAuthConfig(options: 
    WebAuthConfig(isDisplayWallet: true, appearance: .unspecified))
```

### Set and Get Language

You can configure the language of your app with the `setLanguage` and `getLanguage` functions. The default language is English.

**Example**

```swift
ParticleNetwork.setLanguage(.en)
let language = ParticleNetwork.getLanguage()
```

### Set and Get FiatCoin

You can set the fiat coin symbol with the `setFiatCoin` function. The default fiat coin is USD. You can retrieve the current fiat coin with the `getFiatCoin` function.

**Example**

```swift
ParticleNetwork.setFiatCoin(.USD)
let fiatCoin = ParticleNetwork.getFiatCoin()
```

### Signatures

The Particle SDK provides several methods for signing transactions or messages.

#### 1. Sign and Send Transaction

This method signs and sends a transaction to the Particle Node and then returns the txHash.

**Prototype**

```swift
ParticleAuthService.signAndSendTransaction(transaction)
```

**Parameter**

`transaction` (String): The transaction to sign and to send. The request is a base58 string in Solana or a hex string in EVM.

**Example**

```swift
ParticleAuthService.signAndSendTransaction(transaction).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let txHash):
        print(txHash)
    }
}.disposed(by: bag)
```

#### 2. Sign Transaction

This method signs a transaction then returns the signatrue.

**Prototype**

```swift
ParticleAuthService.signTransaction(transaction)
```

**Parameter**

`transaction` (String): The transaction to sign. This request is a base58 string in Solana and currently does not support EVM.

**Example**

```swift
ParticleAuthService.signTransaction(transaction).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let txHash):
        print(txHash)
    }
}.disposed(by: bag)
```

#### 3. Sign Message

This method signs a message and then returns signature.

**Prototype**

```swift
ParticleAuthService.signMessage(message)
```

**Parameter**

`message` (String): The message to sign. For Solana, it can be any string while for EVM, it should be a hex string.

**Example**

```swift
ParticleAuthService.signMessage(message).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let signature):
        print(signature)
    }
}.disposed(by: bag)
```

#### 4. Sign Message Unique

This method signs a message and then returns the signature, with the feature that the same message will always generate the same signature.

**Prototype**

```swift
ParticleAuthService.signMessageUnique(message)
```

**Parameter**

`message` (String): The message to sign. For EVM, this can be a hex string or any other string.

**Example**

```swift
ParticleAuthService.signMessageUnique(message).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let signature):
        print(signature)
    }
}.disposed(by: bag)
```

#### 5. Sign Typed Data

This method signs typed data and then returns the signature. It supports v1, v3 and v4 typed data.

**Prototype**

```swift
ParticleAuthService.signTypedData(message, version: .v4)
```

**Parameters**

`message` (String): The typed data to sign. It should be a hex string and this method does not support Solana at present.

`version` (EVMSignTypedDataVersion): The version of typed data. It could be v1, v3, v4 or v4Unique.

**Example**

```swift
ParticleAuthService.signTypedData(message, version: .v4).subscribe { [weak self] result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let signature):
        print(signature)
    }
}.disposed(by: bag)
```

You can create a `transaction` with `TxData` and `FeeMarketEIP1559TxData` There's an easy way to do this with [Wallet Service](../../wallet-service/).

### Open Web Wallet

You can open the web wallet with the `openWebWallet` function:

#### Parameters

`styleJsonString`: A JSON format string specifying the display details for the web wallet.

```swift
let styleJsonString = """
{
  "supportAddToken": false,
  "supportChains": [{"id": 1,"name": "Ethereum"},{"id": 5,"name": "Ethereum"}]
}
"""
ParticleAuthService.openWebWallet(styleJsonString: styleJsonString)
```

### Open Account and Security

Open the user's account & security page with `openAccountAndSecurity` function:

**Example**

```swift
ParticleAuthService.openAccountAndSecurity()
```

### Set and Get Security Account Config

These methods allow you to configure and store security settings.

**Parameters**

`promptSettingWhenSign` (String): Control prompt payment password when sign, 0 no prompt, 1 first time show prompt, 2 every time show prompt, 3 force show prompt

`promptMasterPasswordSettingWhenLogin` (String): Control prompt master password when login, 0 no prompt, 1 first time show prompt, 2 every time show prompt, 3 force show prompt

**Example**

```swift
// Set the Security Account Config
ParticleNetwork.setSecurityAccountConfig(config: 
    SecurityAccountConfig.init(promptSettingWhenSign: 1, promptMasterPasswordSettingWhenLogin: 2))

// Get the Security Account Config
let securityAccountConfig = ParticleNetwork.getSecurityAccountConfig()
```

### CheckSum support

To get an Ethereum address checksum transitioning from a normal address, use the `toChecksumAddress` function.

#### Example

```swift
"0x2648cfe97e33345300db8154670347b08643570b".toChecksumAddress()
```

### TRON network support

The SDK allows you to convert TRON base58 addresses to hex addresses, and vice versa.

#### Example

```swift
let tronAddressHex = TronFormatAddress.toHex("TDTduRew1o2ZqP9wiDPVEqdywMQdNC4hto")
print(tronAddressHex) // 0x2648cfe97e33345300db8154670347b08643570b

let tronAddressBase58 = TronFormatAddress.fromHex("0x2648cfe97e33345300db8154670347b08643570b")
print(tronAddressBase58) // TDTduRew1o2ZqP9wiDPVEqdywMQdNC4hto
```

### Particle Provider

The SDK provides a Particle Provider that handles wallet connect requests. The Particle Provider supports the following methods:

* `eth_sendTransaction`: Return the signature or an error.
* `eth_signTypedData`: Return the signature or an error.
* `personal_sign`: Return the signature or an error.
* `wallet_switchEthereumChain`: Return null or an error.
* `eth_chainId`: Return chainId string like "0x5" or an error.
* `eth_signTypedData_v1`: Return the signature or an error.
* `eth_signTypedData_v3`: Return the signature or an error.
* `eth_signTypedData_v4`: Return the signature or an error.

#### Prototype

```swift
ParticleProvider.request(method: String, params: [Encodable]) -> Single<Data?>
```

#### Parameters

`method` (String): The method to be requested. `params` (\[Encodable]): Array of parameters to pass to the method.

#### Example

```swift
ParticleProvider.request(method: method, params: params).subscribe { data in
   // handle returned data
} onFailure: { error in
    // handle error
}.disposed(by: bag)
```

### Error

Any error that occurs will be of type `ParticleNetwork.Error` and will contain details about the error that occurred. The errors will be logged in debug DevEnvironment.

## Particle Wallet Connect

The Particle Wallet Connect SDK allows you to integrate your app as a Wallet Connect wallet. Below is the documentation for integrating and using the Particle Wallet Connect SDK.

If you are using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

#### Adding Particle Wallet Connect with CocoaPods

To add the Particle Wallet Connect SDK to your project, you can use the following Podfile entry:

```ruby
pod 'ParticleWalletConnect', '1.2.2'
```

Make sure to run `pod install` after updating your Podfile to add the Particle Wallet Connect V2 SDK to your project.

### API Reference

#### Initialize

To initialize the Particle Wallet Connect SDK, you can use the `initialize` method and provide the wallet metadata.

**Example**

```swift
ParticleWalletConnect.initialize(WalletMetaData)
```

#### Migrating to WalletConnect v2

You can set the WalletConnect V2 project ID using the following method:

**Example**

```swift
ParticleWalletConnect.setWalletConnectV2ProjectId("your wallet connect v2 project id")
```

#### Connect and Handling Connections

You can use the Particle Wallet Connect SDK to connect to a wallet and handle the connection results by implementing the `ParticleWalletConnectDelegate` protocol.

**Delegate Methods**

* `shouldConnectDapp`: Request to connect to the Dapp with metadata.
* `request`: Handle requests from the Dapp.
* `didConnectDapp`: Called after successfully connecting to a Dapp.
* `didDisconnectDapp`: Called after disconnecting from a Dapp.

#### Disconnect

You can disconnect from a Wallet Connect V2 session using the following method:

**Example**

```swift
Task {
    do {
        try await self.pwc.disconnect(topic: topic)
    } catch {
        print(error)
    }
}
```

#### Get Connected Dapp

You can get the Dapp metadata by topic and get all connected Dapps using the following methods:

**Prototypes**

```swift
func getDapp(by topic: String) -> DappMetaData?
func getAllDapps(publicAddress: String) -> [DappMetaData]
```

#### Update Session

The Wallet Connect V2 public address and chain ID can be updated using the following method:

**Example**

```swift
do {
    try self.pwc.updateWalletConnect(dapp.topic, publicAddress: publicAddress, chainId: chainId)
} catch {
    print(error)
}
```

These methods allow you to integrate your app as a Wallet Connect wallet and handle session management effectively.

# iOS

## Add Auth Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1 or later
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 12

### Create a Particle Project and App

Before you can add Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Broken link](broken-reference "mention") to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://particle.network/#login)

### Add the Auth Service SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Auth Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) .

Auth Service's CocoaPods distribution requires Xcode 13.3.1 and CocoaPods 1.10.0 or higher.Here's how to install Auth Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app.

```
pod 'ParticleAuthService'
```

3\. Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

```
pod install --repo-update
```

```
open your-project.xcworkspace
```

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

### Initialize Auth Service in your app <a href="#initialize-firebase" id="initialize-firebase"></a>

The final step is to add initialization code to your application. You may have already done this as part of adding Auth Service to your app. If you're using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

1. Import the `ParticleNetwork` module in your `UIApplicationDelegate.`

```
import ParticleNetwork     
```

2\. Initialize ParticleNetwork service, typically in your app's `application:didFinishLaunchingWithOptions:` method:

{% code title="swift" %}
```swift
        let projectUuid: String = "your project uuid"
        let projectClientKey: String = "your project client key"
        let projectAppUuid: String = "your project app uuid"
        let chainName = ChainName.solana
        let chainEnv = ChainEnvironment.devnet
        let devEnv = DevEnvironment.debug
        
        ParticleNetwork.initialize(projectUuid: projectUuid,
                                   projectClientKey: projectClientKey,
                                   projectAppUuid: projectAppUuid,
                                   chainName: chainName,
                                   chainEnv: chainEnv,
                                   devEnv: devEnv)
```
{% endcode %}

3\. Add scheme url handle in your app's `application(_:open:options:)` method

```
return ParticleNetwork.handleUrl(url)
```

4\. Config your app scheme url, select your app target, in the info section, click add URL Type, past your scheme in URL Schemes.&#x20;

your scheme url should be "pn" + your project app id.&#x20;

for example, if you project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", you scheme url is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](../../.gitbook/assets/image.png)

{% hint style="info" %}
Replace <mark style="color:red;">**your project uuid**</mark>, <mark style="color:red;">**your project client key**</mark>, <mark style="color:red;">**your project app uuid**</mark> <mark style="color:red;"></mark><mark style="color:red;"></mark> with the new values created in your Dashboard**.**

You can dynamically switch the chainEnv by calling the `ParticleNetwork.setChainEnv()` method. ``&#x20;

devEnv needs to be modified to be `DevEnvironment.production` for release.
{% endhint %}

## API Reference



### Login

To auth login with Particle, call `ParticleNetwork.login(...)`and `subscribe`. You can log in with your email or phone number by changing the `LoginType` parameter. Your wallet is created when you log in successfully for the first time.

{% tabs %}
{% tab title="Swift" %}
```kotlin
ParticleNetwork.login(type: .email).subscribe { [weak self] result in
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

{% tab title="Java" %}
```java
ParticleNetwork.login(activity, LoginType.EMAIL, new WebServiceCallback<LoginOutput>() {
    @Override
    public void success(@NonNull LoginOutput output) {
        //login success, return user info in output
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle err
    }
});
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
After log-in success, you can obtain user info by calling `ParticleNetwork.getUserInfo()`
{% endhint %}

### Logout

The SDK will delete users' account information in cache.

{% tabs %}
{% tab title="Swift" %}
```kotlin
ParticleNetwork.logout().subscribe { [weak self] result in
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
```java
ParticleNetwork.logout(this, new WebServiceCallback<WebOutput>() {
    @Override
    public void success(@NonNull WebOutput output) {
        //logout success
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});
```
{% endtab %}
{% endtabs %}

### Signatures

Use the Particle SDK to sign a transaction or message. The SDK provides three methods for signing:

1. `signAndSendTransaction`: sign and send the transaction with Particle Node, then return the signature.
2. `signTransaction`: sign transaction, return signed message.
3. `signMessage`: sign message, return signed message.

{% tabs %}
{% tab title="Swift" %}
```kotlin
//transaction: base58 string
ParticleNetwork.signAndSendTransaction(transaction).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signature):
        // handle signature 
    }
}.disposed(by: bag)
        
//transaction: base58 string
ParticleNetwork.signtransaction(transaction).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)

//sign any string
ParticleNetwork.signMessage(message).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)
```
{% endtab %}

{% tab title="Objective-C" %}
```java
//transaction: base58 string
ParticleNetwork.signAndSendTransaction(transaction).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signature):
        // handle signature 
    }
}.disposed(by: bag)
        
//transaction: base58 string
ParticleNetwork.signtransaction(transaction).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)

//sign any string
ParticleNetwork.signMessage(message).subscribe {  [weak self] result in
    guard let self = self else { return }
    switch result {
    case .failure(let error):
        // handle error
    case .success(let signedMessage):
        // handle signed message
    }
}.disposed(by: bag)
```
{% endtab %}
{% endtabs %}

### Error

`ParticleNetwork.Error` contains error details, error will be logged in `debug` `DevEnvironment`

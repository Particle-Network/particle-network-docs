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
pod 'ParticleNetwork'
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

{% hint style="info" %}
Replace <mark style="color:red;">**your project uuid**</mark>, <mark style="color:red;">**your project client key**</mark>, <mark style="color:orange;">**your project app uuid**</mark> with the new values created in your Dashboard**.**

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
                print(error)
            case .success(let userinfo):
                print(userinfo)
                
                self.showLogin(false)
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
{% tab title="Kotlin" %}
```kotlin
ParticleNetwork.logout(activity, object : WebServiceCallback<WebOutput> {
    override fun success(output: WebOutput) {
        //logout success
    }

    override fun failure(errMsg: WebServiceError) {
        //handle error
    }
})
```
{% endtab %}

{% tab title="Java" %}
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
{% tab title="Kotlin" %}
```kotlin
//transaction: base58 string
ParticleNetwork.signAndSendTransaction(activity, transaction, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign and send transaction success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})


//transaction: base58 string
ParticleNetwork.signTransaction(activity, transaction, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign transaction success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})


//sign any string
ParticleNetwork.signMessage(activity, message, object : WebServiceCallback<SignOutput>{
    override fun success(output: SignOutput) {
        //sign success
    }

    override fun failure(errMsg: WebServiceError) {
        // handle error
    }
})
```
{% endtab %}

{% tab title="Java" %}
```java
//transaction: base58 string
ParticleNetwork.signAndSendTransaction(activity, transaction, new WebServiceCallback<SignOutput>() {
    
    @Override
    public void success(@NonNull SignOutput output) {
        //sign and send transaction success
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});


//transaction: base58 string
ParticleNetwork.signTransaction(activity, transaction, new WebServiceCallback<SignOutput>() {
    
    @Override
    public void success(@NonNull SignOutput output) {
        //sign transaction success
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});


//sign any string
ParticleNetwork.signMessage(activity, message, new WebServiceCallback<SignOutput>() {
    
    @Override
    public void success(@NonNull SignOutput output) {
        //sign success
    }
    
    @Override
    public void failure(@NonNull WebServiceError errMsg) {
        //handle error
    }
});
```
{% endtab %}
{% endtabs %}

### Error

`WebServiceError` contains error details. You can check the information by printing the  `message` attribute.

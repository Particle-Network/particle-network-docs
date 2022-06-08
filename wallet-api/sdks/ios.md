# iOS

## Add Wallet API to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 12

### Create a Particle Project and App

Before you can add Auth Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://particle.network/#/login)

### Add the **Wallet** API SDK to Your App <a href="#add-sdks" id="add-sdks"></a>

Auth Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) .

Auth Service's CocoaPods distribution requires Xcode 13.3.1 and CocoaPods 1.10.0 or higher.Here's how to install Auth Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app.

```
pod 'ParticleWalletAPI'
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

### Initialize Wallet API in your app <a href="#initialize-firebase" id="initialize-firebase"></a>

The final step is to add initialization code to your application. You may have already done this as part of adding Auth Service to your app. If you're using a [quickstart sample project](https://github.com/Particle-Network/particle-ios), this has been done for you.

1. Create a  **ParticleNetwork-Info.plist** into the root of your Xcode project.
2. Copy the following  text into this file

```
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

3\. Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard.

4\. Import the `ParticleNetwork` module in your `UIApplicationDelegate.`

```
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

{% hint style="info" %}
You can dynamically switch the chainEnv by calling the `ParticleNetwork.`setChainName`()` method.&#x20;

devEnv needs to be modified to be `DevEnvironment.production` for release.
{% endhint %}

## API Reference

### GetSolanaService

{% tabs %}
{% tab title="Swift" %}
```kotlin
let service = ParticleWalletAPI.getSolanaService()
```
{% endtab %}

{% tab title="Objective-C" %}
```java
```
{% endtab %}
{% endtabs %}

### GetEvmService

```
let service = ParticleWalletAPI.getEvmService()
```

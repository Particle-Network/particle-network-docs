# iOS

## Add Wallet Service to Your iOS Project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1 or later
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 12

### Create a Particle Project and App

Before you can add Wallet Service to your iOS app, you need to create a Particle project to connect to your iOS app. Visit [Broken link](broken-reference "mention") to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://particle.network/#login)

### Add the Wallet Service SDK to your app <a href="#add-sdks" id="add-sdks"></a>



Wallet Service supports installation with [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) .

Wallet Service's CocoaPods distribution requires Xcode 13.3.1 and CocoaPods 1.10.0 or higher.Here's how to install Wallet Service using CocoaPods:

1. Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

```
pod init
```

2\. To your Podfile, add the Auth Service pods that you want to use in your app.

```
pod 'ParticleNetworkGUI'
```

3\. Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

```
pod install --repo-update
```

```
open your-project.xcworkspace
```

The Wallet Service can only be used after a successful log-in with Auth Service.

{% hint style="info" %}
Wallet Service depends on Auth Service, you must import [Auth Service](../../auth-service/sdks/android.md).&#x20;
{% endhint %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

## API Reference

### Open Wallet

{% tabs %}
{% tab title="Swift" %}
```kotlin
PNRouter.navigator(routhPath: .wallet)
```
{% endtab %}

{% tab title="Objective-C" %}
```java
PNRouter.build(RouterPath.Wallet).navigation();
```
{% endtab %}
{% endtabs %}



### Open Send Token

{% tabs %}
{% tab title="Swift" %}
```kotlin
//open send spl token
let tokenSendConfig = TokenSendConfig(tokenAddress: nil, toAddress: nil, amount: nil)
PNRouter.navigatorTokenSend(tokenSendConfig: tokenSendConfig)

//open send default token by chain name
PNRouter.navigator(routhPath: .tokenSend)
```
{% endtab %}

{% tab title="Java" %}
```java
//open send spl token
WalletSendParams params = new WalletSendParams(tokenAddress, toAddress, toAmount);
PNRouter.build(RouterPath.TokenSend, params).navigation();

//open send default token by chain name
PNRouter.build(RouterPath.TokenSend).navigation();
```
{% endtab %}
{% endtabs %}

`WalletSendParam` has three parameters:

1. `tokenAddress`: token mint address
2. `toAddress`: receiver address (optional)
3. `toAmount`: send token amount, token minimum unit (optional)&#x20;

### Open Receive Token

Display your address QRCode.

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
PNRouter.build(RouterPath.TokenReceive).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
PNRouter.build(RouterPath.TokenReceive).navigation();
```
{% endtab %}
{% endtabs %}

### Open Transaction Records

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
//open spl token transaction records
val params = TokenTransactionRecordsParams(tokenAddress)
PNRouter.build(RouterPath.TokenTransactionRecords, params).navigation()

//open default token transaction records by chain name
PNRouter.build(RouterPath.TokenTransactionRecords).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
//open spl token transaction records
TokenTransactionRecordsParams params = new TokenTransactionRecordsParams(tokenAddress);
PNRouter.build(RouterPath.TokenTransactionRecords, params).navigation();

//open default token transaction records by chain name
PNRouter.build(RouterPath.TokenTransactionRecords).navigation();
```
{% endtab %}
{% endtabs %}


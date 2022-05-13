# Android

## Add Wallet Service to your Android project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Make sure that your project meets these requirements:

* Targets API level 23 (Marshmallow) or higher
* Uses Android 6.0 or higher
* Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)

### Create Particle project and app

Before you can add Wallet Service to your Android app, you need to create a Particle project to connect to your Android app. Visit [Broken link](broken-reference "mention") to learn more about Particle projects and app.

[👉 Sign up/log in and create your project now](https://particle.network/#login)

### Add Wallet Service SDK to your app <a href="#add-sdks" id="add-sdks"></a>

Declare them in your module (app-level) Gradle file (usually app/build.gradle).

{% tabs %}
{% tab title="Kotlin" %}
```kts
repositories {
    google()
    mavenCentral()
    //...
}

dependencies {
    // Particle Auth Service
    implementation("particle.network:auth-service${latest_version}")
    // Particle Wallet Service
    implementation("particle.network:wallet-service${latest_version}")
    //...
}
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
repositories {
    google()
    mavenCentral()
    //...
}

dependencies {
    // Particle Auth Service
    implementation 'particle.network:auth-service${latest_version}'
    // Particle Wallet Service
    implementation 'particle.network:wallet-service${latest_version}'
    //...
}
```
{% endtab %}
{% endtabs %}

The Wallet Service can only be used after successfully logging in with the Auth Service.

{% hint style="info" %}
Wallet Service depends on Auth Service, you must import [Auth Service](../../auth-service/sdks/android.md).&#x20;
{% endhint %}

{% hint style="info" %}
If you want to receive release updates, subscribe to our [GitHub repository](https://github.com/Particle-Network).
{% endhint %}

## API Reference

### Open Wallet

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
PNRouter.build(RouterPath.Wallet).navigation()
```
{% endtab %}

{% tab title="Java" %}
```java
PNRouter.build(RouterPath.Wallet).navigation();
```
{% endtab %}
{% endtabs %}

`navigation()`can add param `callback` to listen result.

### Open Send Token

{% tabs %}
{% tab title="Kotiln" %}
```kotlin
//open send spl token
val params = WalletSendParams(tokenAddress, toAddress?, toAmount?)
PNRouter.build(RouterPath.TokenSend, params).navigation()

//open send default token by chain name
PNRouter.build(RouterPath.TokenSend).navigation()
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

`WalletSendParam` has three params:

1. `tokenAddress`: token mint address.
2. `toAddress`: (optional) receiver address.
3. `toAmount`: (optional) send token amount, token minimum unit.

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


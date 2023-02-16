# React Native

### 1.Add the Auth Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install react-native-particle-auth
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-auth) to get the demo source code&#x20;

### 2.Configure Android project

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Make sure that your project meets the following requirements:

* Targets API Level 23 (Marshmallow) or higher
* Uses Android 6.0 or higher
* Uses [Jetpack (AndroidX)](https://developer.android.com/jetpack/androidx/migrate?authuser=0)
* Java 11

Configure project information，Refer to [here](https://github.com/Particle-Network/particle-react-native/blob/master/particle-auth/example/android/app/build.gradle)

```gradle
android {
    ...
    defaultConfig {
        ......
        manifestPlaceholders["PN_PROJECT_ID"] = "your project id"
        manifestPlaceholders["PN_PROJECT_CLIENT_KEY"] = "your project client key"
        manifestPlaceholders["PN_APP_ID"] = "your app id"
    }
   
   //How you quote wallet sdk, you must set it
    dataBinding {
        enabled = true
    }

}
```

### 3.Configure iOS project

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 13.3.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 13

3.1 After export iOS project, open `your_project_name.xcworkspace` under ios folder, here its name is `ParticleAuthExample.xcworkspace.`

![](<../../.gitbook/assets/image (1) (1).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is checked under Target Membership.

<img src="../../.gitbook/assets/image (6).png" alt="" data-size="original">

3.3 Copy the following text into this file:

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

3.4 Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard

3.5. Import the `ParticleAuthService` module in your `AppDelegate.swift` file.

{% tabs %}
{% tab title="Objective-C" %}
```objectivec
#import <react_native_particle_auth/react_native_particle_auth-Swift.h>
```
{% endtab %}
{% endtabs %}

3.6. Add the scheme URL handle in your app's `application(_:open:options:)` method

{% tabs %}
{% tab title="Objective-C" %}
```objectivec
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
  if ([ParticleAuthSchemeManager handleUrl:url] == YES) {
    return YES;
  } else {
    // other methods
  }
  return YES;
}
```
{% endtab %}
{% endtabs %}

3.7. Configure your app scheme URL, select your app from `TARGETS`,  under `Info` section, click + to add the `URL types`, and paste your scheme in `URL Schemes`

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../.gitbook/assets/image (1) (2) (1).png>)

### Initialize the SDK

### Usage

You can get the demo source code from [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-auth)

```javascript
import * as particleAuth from "react-native-particle-auth";
```

### Initialize the SDK

**Before using the SDK you have to call init(Required)**&#x20;

```javascript
const chainInfo = ChainInfo.EthereumGoerli;
const env = Env.Production;
particleAuth.init(chainInfo, env);
```

### Login

```javascript
const type = LoginType.Phone;
const supportAuthType = [SupportAuthType.All];
const result = await particleAuth.login(type, "", supportAuthType, undefined);
if (result.status) {
    const userInfo = result.data;
    console.log(userInfo);
} else {
    const error = result.data;
    console.log(error);
}
```

### Logout

```javascript
const result = await particleAuth.logout();
if (result.status) {
    console.log(result.data);
} else {
    const error = result.data;
    console.log(error);
}
```

### Is user logged in

```javascript
const result = await particleAuth.isLogin();
jsconsole.log(result);
```

### Get address

```javascript
const address = await particleAuth.getAddress();
console.log(address)
```

### Get user info

```javascript
const result = await particleAuth.getUserInfo();
const userInfo = JSON.parse(result);
console.log(userInfo);
```

### Sign message

```dart
const message = "Hello world!"
const result = await particleAuth.signMessage(message);
if (result.status) {
    const signedMessage = result.data;
    console.log(signedMessage);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign transaction

```dart
const chainInfo = await particleAuth.getChainInfo();
if (chainInfo.chain_name.toLowerCase() != "solana") {
    console.log("signTransaction only supports solana")
    return
}
const sender = await particleAuth.getAddress();
console.log("sender: ", sender);
const transaction = await Helper.getSolanaTransaction(sender);
console.log("transaction:", transaction);
const result = await particleAuth.signTransaction(transaction);
if (result.status) {
    const signedTransaction = result.data;
    console.log(signedTransaction);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign all transactions

```dart
const chainInfo = await particleAuth.getChainInfo();
if (chainInfo.chain_name.toLowerCase() != "solana") {
    console.log("signAllTransactions only supports solana")
    return
}
const sender = await particleAuth.getAddress();
const transaction1 = await Helper.getSolanaTransaction(sender);
const transaction2 = await Helper.getSplTokenTransaction(sender);
const transactions = [transaction1, transaction2];
const result = await particleAuth.signAllTransactions(transactions);
if (result.status) {
    const signedTransactions = result.data;
    console.log(signedTransactions);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign and send transaction

```dart
const sender = await particleAuth.getAddress();
const chainInfo = await particleAuth.getChainInfo();
let transaction = "";
if (chainInfo.chain_name.toLowerCase() == "solana") { 
    transaction = await Helper.getSolanaTransaction(sender);
} else {
    transaction = await Helper.getEthereumTransacion(sender);
}
console.log(transaction);
const result = await particleAuth.signAndSendTransaction(transaction);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign typed data

```dart
const chainInfo = await particleAuth.getChainInfo();
if (chainInfo.chain_name.toLowerCase() == "solana") {
    console.log("signTypedData only supports evm")
    return
}
const typedData = "[    {    \"type\":\"string\",    \"name\":\"Message\",    \"value\":\"Hi, Alice!\"    },    {    \"type\":\"uint32\",    \"name\":\"A nunmber\",    \"value\":\"1337\"    }]";

const version = "v1";

// particleAuth support typed data version v1, v3, v4
const result = await particleAuth.signTypedData(typedData, version);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Set chain info async

Login is required, if you want to switch to another chain, call this method after login.

```dart
const chainInfo = ChainInfo.SolanaDevnet;
const result = await particleAuth.setChainInfoAsync(chainInfo);
console.log(result);
```

### Set chain info sync

if you want to switch to another chain, call this method before login.

```dart
const chainInfo = ChainInfo.EthereumGoerli;
const result = await particleAuth.setChainInfo(chainInfo);
console.log(result);
```

### Get chain info

```javascript
const result = await particleAuth.getChainInfo();
console.log(result);
```

### Set display wallet

```javascript
const isDisplay = true;
particleAuth.setDisplayWallet(isDisplay);
```

### Open web wallet

```javascript
particleAuth.openWebWallet();
```

### Open account and security page

```javascript
const result = await particleAuth.openAccountAndSecurity();
if (result.status) {
    const data = result.data;
    console.log(data);
} else {
    const error = result.data;
    console.log(error);
}
```

### Set iOS modal present style and medium screen

```javascript
setModalPresentStyle = async () => {
    const style = iOSModalPresentStyle.FormSheet;
    particleAuth.setModalPresentStyle(style)
}

// request iOS 15 or later
setMediumScreen = async () => {
    const isMedium = true;
    particleAuth.setMediumScreen(isMedium);
}
```

### Set language

```javascript
setLanguage = async () => {
    const language = Language.JA;
    particleAuth.setLanguage(language);
}
```


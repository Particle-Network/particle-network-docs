# React Native

### 1.Add the AuthCore Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install react-native-particle-auth-core
// install react-native-particle-auth to initialize SDK and set custom config.
npm install react-native-particle-auth
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-auth-core) to get the demo source code&#x20;



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
  * Xcode 14.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 14
* **Please note that the current version of our software does not support simulation testing. To perform testing, you will require an actual iPhone device.**

3.1 After export iOS project, open `your_project_name.xcworkspace` under ios folder, here its name is `ParticleAuthExample.xcworkspace.`

![](<../../../.gitbook/assets/image (1) (1) (2).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is checked under Target Membership.

<img src="../../../.gitbook/assets/image (6) (1).png" alt="" data-size="original">

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

3.7 Edit Podfile, you should follow [Podfile required](../sdks/ios.md#edit-podfile) to edit Podfile.

### Usage

You can get the demo source code from [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-auth-core)

```typescript
import * as particleAuthCore from "react-native-particle-auth-core";
import * as particleAuth from "react-native-particle-auth";
```

### Initialize the SDK

**Before using the SDK you have to call init(Required)**&#x20;

```typescript
// Get your project id and client from dashboard,  
// https://dashboard.particle.network/
ParticleInfo.projectId = ''; // your project id
ParticleInfo.clientKey = ''; // your client key 

const chainInfo = Ethereum;
const env = Env.Production;
particleAuth.init(chainInfo, env);
```

### Connect JWT

```typescript
const jwt = "your jwt";
const result = await particleAuthCore.connect(jwt);
if (result.status) {
    const userInfo = result.data;
    console.log(userInfo);
} else {
    const error = result.data;
    console.log(error);
}
```

### Disconnect

```typescript
const result = await particleAuthCore.disconnect();
if (result.status) {
    console.log(result.data);
} else {
    const error = result.data;
    console.log(error);
}
```

### IsConnected

```javascript
const result = await particleAuthCore.isConnected();
console.log(result);
```

### Swicth chain

```typescript
const chainInfo: ChainInfo = PolygonMumbai;
const result = await particleAuthCore.switchChain(chainInfo);
console.log(result);
```

### Change master password

```typescript
const result = await particleAuthCore.changeMasterPassword();
if (result.status) {
    console.log(result.data);
} else {
    const error = result.data;
    console.log(error);
}
```

### Has set payment password

```typescript
const hasPaymentPassword = await particleAuthCore.hasPaymentPassword();
console.log('hasPaymentPassword', hasPaymentPassword);
```

### Has set master password

```javascript
const hasMasterPassword = await particleAuthCore.hasMasterPassword();
console.log('hasMasterPassword', hasMasterPassword);
```

### Get user info

```typescript
const result = await particleAuthCore.getUserInfo();
const userInfo = JSON.parse(result);
console.log(userInfo);
```

### Open web wallet

```typescript
openWebWallet = async () => {
    //https://docs.particle.network/developers/wallet-service/sdks/web
    let webConfig = {
        supportAddToken: false,
        supportChains: [
            {
                id: 1,
                name: 'Ethereum',
            },
            {
                id: 5,
                name: 'Ethereum',
            },
        ],
    };
    const webConfigJSON = JSON.stringify(webConfig);
    particleAuthCore.openWebWallet(webConfigJSON);
};
```

### Open account and security

```typescript
const result = await particleAuthCore.openAccountAndSecurity();
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### EVM Sign

### Get address

```typescript
const address = await evm.getAddress();
console.log('evm address ', address);
```

### Personal sign and personal sign unique

```dart
// message requires hexadecimal string or human readable string.
const message = 'Hello world!'; 

const result = await evm.personalSign(message);
const result = await evm.personalSignUnique(message);

if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign typed data and sign typed data unique

```typescript
// typedData requires hexadecimal string or human readable json string.
const typedData: string = 'your typed data v4'

const result = await evm.signTypedData(typedData);
const result = await evm.signTypedDataUnique(typedData);

if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Send transaction

```dart
// transaction requires hexadecimal string
const transaction = 'your transaction';

const result = await evm.sendTransaction(transaction);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Solana Sign

### Get address

```typescript
const address = await solana.getAddress();
console.log('solana address ', address);
```

### Sign message

```dart
// message requires human readable string.
const message = 'Hello world!';
const result = await solana.signMessage(message);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign  transaction

```dart
const sender = await solana.getAddress();
console.log('sender: ', sender);
const transaction = await Helper.getSolanaTransaction(sender);
// transaction requires base58 string.
const result = await solana.signTransaction(transaction);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign  all transactions

```dart
const sender = await solana.getAddress();
console.log('sender: ', sender);
const transaction1 = await Helper.getSolanaTransaction(sender);
const transaction2 = await Helper.getSplTokenTransaction(sender);
// transaction requires base58 string.
const transactions = [transaction1, transaction2];

const result = await solana.signAllTransactions(transactions);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

### Sign and send transaction

```dart
const sender = await solana.getAddress();
console.log('sender: ', sender);
const transaction = await Helper.getSolanaTransaction(sender);
// transaction requires base58 string.
const result = await solana.signAndSendTransaction(transaction);
if (result.status) {
    const signature = result.data;
    console.log(signature);
} else {
    const error = result.data;
    console.log(error);
}
```

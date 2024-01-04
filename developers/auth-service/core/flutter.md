# Flutter

### 1.Add the Auth Core Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_auth_core
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-auth-core) to get the demo source code

### 2.Configure Android project

2.1 open ${your flutter project} /android/app/build.gradle

2.1.1 add project config and update the minSdkVersion to 23

```gradle
//Modify these configurations
defaultConfig {
    // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
    applicationId "com.example.particle_auth_test"
    // You can update the following values to match your application needs.
    // For more information, see: https://docs.flutter.dev/deployment/android#reviewing-the-build-configuration.
    minSdkVersion flutter.minSdkVersion
    targetSdkVersion flutter.targetSdkVersion
    versionCode flutterVersionCode.toInteger()
    versionName flutterVersionName
}

//After modification：
defaultConfig {
   // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
   applicationId "com.example.particle_auth_test"
   // You can update the following values to match your application needs.
   // For more information, see: https://docs.flutter.dev/deployment/android#reviewing-the-build-configuration.
   minSdkVersion 23 // required by particle sdk
   targetSdkVersion flutter.targetSdkVersion
   versionCode flutterVersionCode.toInteger()
   versionName flutterVersionName

   //get from https://dashboard.particle.network/
   manifestPlaceholders["PN_PROJECT_ID"] = "772f7499-1d2e-40f4-8e2c-7b6dd47db9de" //your project id
   manifestPlaceholders["PN_PROJECT_CLIENT_KEY"] = "ctWeIc2UBA6sYTKJknT9cu9LBikF00fbk1vmQjsV" //your project client key
   manifestPlaceholders["PN_APP_ID"] = "01a23ce8-d2e9-4b37-9eab-bf477279e53e" //your app id
}
```

2.1.2 update Java Version to 11 and add dataBinding config

```gradle
//Modify these configurations
compileOptions {
  sourceCompatibility JavaVersion.VERSION_1_8
  targetCompatibility JavaVersion.VERSION_1_8
}

kotlinOptions {
  jvmTarget = '1.8'
}

//After modification：
compileOptions {
  sourceCompatibility JavaVersion.VERSION_11
  targetCompatibility JavaVersion.VERSION_11
}

kotlinOptions {
  jvmTarget = JavaVersion.VERSION_11
}

dataBinding {
  enabled = true
}
```

2.2 edit ${your flutter project} /android/build.gradle

<pre class="language-gradle"><code class="lang-gradle">buildscript {
    ...
    repositories {
        google()
        mavenCentral()
        maven { setUrl("https://jitpack.io") }  //add this line 
    }

<strong>    dependencies {
</strong>        ...
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
        maven { setUrl("https://jitpack.io") }  //add this line 
    }
}
...

</code></pre>

Now,Android configuration is complete!

### 3.Configure iOS project

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install the following:
  * Xcode 14.1 or later.
* Make sure that your project meets the following requirements:
  * Your project must target these platform versions or later:
    * iOS 14

3.1 After export iOS project, open Runner.xcworkspace under ios folder.

![](<../../../.gitbook/assets/image (5) (1).png>)

3.2 Create a **ParticleNetwork-Info.plist** into the root of your Xcode project, and make sure the file is checked under Target Membership.

![](<../../../.gitbook/assets/image (1) (3).png>)

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

3.5 Edit Podfile, you should follow [Podfile required](../sdks/ios.md#edit-podfile) to edit Podfile.

3.6 More important to note, our SDK is static XCFramework, when introduced, you must specify the use of static libraries, `use_frameworks! :linkage => :static`

```
target 'Runner' do
  use_frameworks! :linkage => :static
  
  ...other
end
```

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**

```dart
// Get your project id and client key from dashboard, https://dashboard.particle.network
const projectId = "";
const clientK = ""; 
if (projectId.isEmpty || clientK.isEmpty) {
  throw const FormatException(
      'You need set project info, get your project id and client key from dashboard, https://dashboard.particle.network');
}
ParticleInfo.set(projectId, clientK);
ParticleAuth.init(currChainInfo, env);
ParticleAuthCore.init();
```

### Connect

If loginType is email or phone, you can pass email address or phone number to `account`.

If loginType is JWT, you must pass JWT token to `account`.

If loginType is email or phone, below parameter is optional, other wise, they are useless.

The `supportAuthTypes` controls which types of social login buttons are displayed at the bottom of the login page.

The `loginPageConfig` controls the title, description, and image displayed on the login page.

```dart
final userInfo = await ParticleAuthCore.connect(loginType,
          account: account,
          supportAuthTypes: supportAuthTypes,
          loginPageConfig: LoginPageConfig(
              imagePath:
                  "your icon url", // icon url or base64 string
              projectName: "Flutter Example",
              description: "Welcome to login",
              imageType: "url")); // url | base64
```

{% hint style="info" %}
Connect with JWT

```dart
const jwt = ""; // paste your jwt
final userInfo = await ParticleAuthCore.connect(LoginType.jwt, account: jwt);
```
{% endhint %}

{% hint style="info" %}
Connect with Google&#x20;

```dart
final userInfo = await ParticleAuthCore.connect(LoginType.google);
```
{% endhint %}

### Is Connected

```dart
final isConnected = await ParticleAuthCore.isConnected();
```

### Disconnect

```dart
final result = await ParticleAuthCore.disconnect();
```

### Get user info

```dart
final userInfo = await ParticleAuthCore.getUserInfo();
```

### Get EVM address

```dart
final address = await Evm.getAddress();
```

### EVM Sign message and sign message unique

In EVM chain requires a hexadecimal string

If your message is already a hexadecimal string, don't need to convert again, if your message is a human readable string, like "Hello Particle!", you should convert it to hexadecimal string.

```dart
final messageHex = "0x${StringUtils.toHexString("Hello Particle!")}";
String signature = await Evm.personalSign(messageHex);
String signature = await Evm.personalSignUnique(messageHex);
```

### EVM Sign typed data and sign typed data unique

If your typed data is already a hexadecimal string, don't need to convert again, if your typed data is a human readable string, is a json string, you should convert it to hexadecimal string.

```dart
String typedData = await getTypedDataV4();
String typedDataHex = "0x${StringUtils.toHexString(typedData)}";
String signature = await Evm.signTypedData(typedDataHex);
String signature = await Evm.signTypedDataUnique(typedDataHex);
```

### EVM Send transaction

The transaction requires a hexadecimal string.

Please explore our GitHub. In the `example/transaction_mock.dart` file, you can learn how to mock a test transaction.

```dart
final transaction = "Your transaction";
String signature = await Evm.sendTransaction(transaction);
```

### Get Solana address

```dart
final address = await Solana.getAddress();
```

### Solana sign message, sign transaction and sign all transactions

Both message and transaction should be a base58 string.

```dart
final signature = await Solana.signMessage(message);

final transaction = await TransactionMock.mockSolanaTransaction(address);
final signature = await Solana.signTransaction(transaction);

List<String> transactions = <String>[];
List<String> signatures = await Solana.signAllTransactions(transactions);
```

### Solana sign and send transaction

Transaction should be a base58 string.

```dart
final transaction = await TransactionMock.mockSolanaTransaction(address);
final signature = await Solana.signAndSendTransaction(transaction);
```

### Switch chain

```dart
bool isSuccess = await ParticleAuthCore.switchChain(ChainInfo.PolygonMumbai);
```

### Set security account config

```dart
// set security account config, 
// the first parameter is promptSettingWhenSign, default value is 1.
// the second parameter is promptMasterPasswordSettingWhenLogin, default value is 0.
// 0 no prompt
// 1 first time show prompt
// 2 every time show prompt
final config = SecurityAccountConfig(1, 2);
ParticleAuth.setSecurityAccountConfig(config);
```

### Open account and security page

If user is expired, should return error, If user is expired, should return error, otherwise return nothing.

```dart
try {
  String result = await ParticleAuthCore.openAccountAndSecurity();
  print("openAccountAndSecurity: $result");
} catch (error) {
  print("openAccountAndSecurity: $error");
  showToast("openAccountAndSecurity: $error");
}
```

### Blind sign enable

This switch will work if the following conditions are met:

1\. your account is connected with JWT

2\. your account does not set payment password

3\. SecurityAccountConfig.promptSettingWhenSign is 0, you can call ParticleAuth.setSecurityAccountConfig to update its value.

```dart
static void setBlindEnable(bool enable) async {
  ParticleAuthCore.setBlindEnable(enable);
}

static void getBlindEnable() async {
  final result = ParticleAuthCore.getBlindEnable();
  print("getBlindEnable: $result");
  showToast("getBlindEnable: $result");
}
```

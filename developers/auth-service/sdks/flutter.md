# Flutter

### 1.Add the Auth Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_auth
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-auth) to get the demo source code

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

3.5. Import the `ParticleAuthService` module in your `AppDelegate.swift` file.

{% tabs %}
{% tab title="Swift" %}
```swift
import ParticleAuthService
```
{% endtab %}
{% endtabs %}

3.6. Add the scheme URL handle in your app's `application(_:open:options:)` method

{% tabs %}
{% tab title="Swift" %}
```swift
override func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
    if ParticleAuthService.handleUrl(url) {
        return true
    } else {
        return super.application(app, open: url, options: options)
    }
}
```
{% endtab %}
{% endtabs %}

3.7. Configure your app scheme URL, select your app from `TARGETS`, under `Info` section, click + to add the `URL types`, and paste your scheme in `URL Schemes`

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)

3.8 Edit Podfile, you should follow [Podfile required](ios.md#edit-podfile) to edit Podfile.

3.9 More important to note, our SDK is static xcframework, when introduced, you must specify the use of static libraries, `use_frameworks! :linkage => :static`

```
target 'Runner' do
  use_frameworks! :linkage => :static
  
  ...other
end
```

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**

```dart
ParticleAuth.init(Ethereum.mainnet(), env);
```

### Login

```dart
List<SupportAuthType> supportAuthType = <SupportAuthType>[];
supportAuthType.add(SupportAuthType.all);
//message:evm->hex sign message, solana is base58 string.
//uniq:unique sign,only support evm.
final authorization = LoginAuthorization(messageHex, true);

//authorization is optional
final userInfo = await ParticleAuth.login(
    LoginType.phone,
    "", 
    supportAuthType,
    socialLoginPrompt,
    authorization
);
```

### Is Login Async

Is user login, check from server, recommended.

if you can get the userInfo, which means user is login, otherwise current user is invalid.

```dart
try {
    final userInfo = await ParticleAuth.isLoginAsync();
    print("isLoginAsync: $userInfo");
  } catch (error) {
    print("isLoginAsync: $error");
  }
```

### Is Login

Is user login, check local data.

```dart
bool result = await ParticleAuth.isLogin();
```

### Logout

```dart
String result = await ParticleAuth.logout();
```

### Fast logout

```dart
String result = await ParticleAuth.fastLogout()
```

### Get address

```dart
final address = await ParticleAuth.getAddress();
```

### Get userInfo

```dart
final userInfo = await ParticleAuth.getUserInfo();
```

### Sign message

In EVM chain requires a hexadecimal string, in Solana chain requires a human readable string.

```dart
final messageHex = "0x${StringUtils.toHexString("Hello Particle")}";
String signature = await ParticleAuth.signMessage(messageHex);
```

### Sign message unique

In EVM chain requires a hexadecimal string, not support Solana.

```dart
final messageHex = "0x${StringUtils.toHexString("Hello Particle")}";
String signature = await ParticleAuth.signMessageUnique(messageHex);
```

### Sign transaction

Only support Solana chain, in Solana chain requires a base58 string.

Please explore our GitHub. In the `example/transaction_mock.dart` file, you can learn how to mock a test transaction.

```dart
final transaction = "Your transaction";
String pubAddress = await ParticleAuth.getAddress();
```

### Sign all transactions

Only support Solana chain, in Solana chain requires a base58 string.

Please explore our GitHub. In the `example/transaction_mock.dart` file, you can learn how to mock a test transaction.

```dart
List<String> transactions = <String>[];
List<String> signatures =
            await ParticleAuth.signAllTransactions(transactions);
```

### Sign and send transaction

In EVM chain requires a hexadecimal string, in Solana chain requires a base58 string.

Please explore our GitHub. In the `example/transaction_mock.dart` file, you can learn how to mock a test transaction.

```dart
final transaction = "Your transaction";
String signature = await ParticleAuth.signAndSendTransaction(transction);
```

### Sign typed data

support v1, v3, v4, v4Unique.

You can get the demo source code from [here](https://github.com/Particle-Network/particle-flutter/blob/master/particle-auth/example/lib/auth\_demo/auth\_logic.dart)

In EVM chain requires a hexadecimal string, not support Solana.

```dart
// your typed data is a json string
String typedDataHex = "0x${StringUtils.toHexString(typedData)}";
final signature = await ParticleAuth.signTypedData(
          typedDataHex, SignTypedDataVersion.v4);
```

### Set chain info sync

```dart
bool isSuccess = await ParticleAuth.setChainInfo(ChainInfo.PolygonMumbai);
print("setChainInfoAsync: $isSuccess");
```

### Set chain info async

```dart
bool isSuccess = await ParticleAuth.setChainInfoAsync(ChainInfo.SolanaDevnet);
print("setChainInfoSync: $isSuccess");
```

### Get chain info

```dart
final chainInfo = await ParticleAuth.getChainInfo();
```

### Set security account config

```dart
// set security account config, 
// the first parameter is promptSettingWhenSign, default value is 1.
// the second parameter is promptMasterPasswordSettingWhenLogin, default value is 0.
// 0 no prompt
// 1 first time show prompt
// 2 every time show prompt
// 3 force show prompt
final config = SecurityAccountConfig(1, 2);
ParticleAuth.setSecurityAccountConfig(config);
```

### Open account and security page

If user is expired, should return error, If user is expired, should return error, otherwise return nothing.

```dart
try {
  String result = await ParticleAuth.openAccountAndSecurity();
  print("openAccountAndSecurity: $result");
} catch (error) {
  print("openAccountAndSecurity: $error");
  showToast("openAccountAndSecurity: $error");
}
```

### Set iOS modal present style

```dart
ParticleAuth.setModalPresentStyle(IOSModalPresentStyle.fullScreen);
```

### Set iOS medium screen

Set iOS medium screen, true is medium screen, false is large screen, default value if false.

Only support iOS 15 or higher.

If you want to set medium screen, don't call setModalPresentStyle with IOSModalPresentStyle.fullScreen

```dart
ParticleAuth.setMediumScreen(true);
```

### Set language

```dart
const language = Language.ja;
ParticleAuth.setLanguage(language);
```

### Set appearance

```dart
ParticleAuth.setAppearance(Appearance.light);
```

### Set fiat coin

```dart
ParticleAuth.setFiatCoin(FiatCoin.KRW);
```

### Set web auth config

```dart
const displayWallet = false;
ParticleAuth.setWebAuthConfig(displayWallet, Appearance.light);
```

### Open web wallet

```dart
//https://docs.particle.network/developers/wallet-service/sdks/web
String webConfig =
    '''
     {
        "supportAddToken": false,
        "supportChains": [{
            "id": 1,
            "name": "Ethereum"
          },
          {
            "id": 5,
            "name": "Ethereum"
          }
        ]
      }
    '''; 
ParticleAuth.openWebWallet(webConfig);
```

### Has master password, payment password, security account

```dart
// get hasMasterPassword, hasPaymentPassword and hasSecurityAccount from local user info.

final hasMasterPassword = await ParticleAuth.hasMasterPassword();
final hasPaymentPassword = await ParticleAuth.hasPaymentPassword();
final hasSecurityAccount = await ParticleAuth.hasSecurityAccount();
final securityAccount = await ParticleAuth.getSecurityAccount();
```

## EVM Service

### Write Contract

Get a write contract transaction

```dart
String publicAddress = "your public address";
String contractAddress = "your contract address";
// this is your contract method name, like balanceOf, mint.
String methodName = "mint"; 
// this is the method params, 
// all parameters should be convert to hex string.
List<Object> params = <Object>["0x3"]; 

// abi json string, you can get it from your contract developer.
// such as
// [{\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"quantity\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"mint\",\"outputs\":[],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]
const abiJsonString = "";

final transaction = EvmService.writeContract(publicAddress, contractAddress, methodName, params, abiJsonString, true);
```

### Read contract

Read conrtact data from blockchain

```dart
String publicAddress = "your public address";
String contractAddress = "your contract address";
// this is your contract method name, like balanceOf.
String methodName ="balanceOf"; 
// this is the method params, 
// all parameters should be convert to hex string.
List<Object> parameters = <Object>[publicAddress];

// abi json string, you can get it from your contract developer.
// such as
// [{\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"quantity\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"mint\",\"outputs\":[],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]
const abiJsonString = "";

final result = await EvmService.readContract(publicAddress, contractAddress, methodName, parameters, abiJsonString);Cr
```

### Create transaction

```dart
// if you want to send native, data should be 0x
// we mock send some token, get data from EvmService.erc20Transfer

String from = "your public address";
String receiver = "receiver address"
String contractAddress = "contract address"
BigInt amount = BigInt.from(1000000);
String to = contractAddress;
final data = await EvmService.erc20Transfer(contractAddress, receiver, amount);

// then create transaction
final transaction = await EvmService.createTransaction(
    from, data, BigInt.from(0), to,
    gasFeeLevel: GasFeeLevel.high);
```

### Estimate gas

Return estimated gas

```dart
final gasLimit = await EvmService.ethEstimateGas(from, to, value, data);
```

### Get suggested gas fees

Return gas fee json object.

```dart
final gasFees = await EvmService.suggestedGasFees();
```

### Get tokens and NFTs

Return all tokens, NFTs and native amount at this address.

```dart
final result = await EvmService.getTokensAndNFTs(address);
```

### Get tokens

Return all tokens and native amount at this address.

```dart
final result = await EvmService.getTokens(publicAddress);
```

### Get NFTs

Return all NFTs at this address.

```dart
final result = await EvmService.getNFTs(publicAddress);
```

### Get token by token addresses

Return the balance of the token specified below this address

```dart
final result = await EvmService.getTokenByTokenAddresses(publicAddress, tokenAddresses);
```

### Get transactions by address

Return all transaction at this address

```dart
final result = await EvmService.getTransactionsByAddress(publicAddress);
```

### Get price

Return token price, if you want the native token price, use `native` as its address.

```dart
List<String> currencies = <String>['usd'];
List<String> tokenAddresses = <String>['native'];
tokenAddresses.add('0x001B3B4d0F3714Ca98ba10F6042DaEbF0B1B7b6F');
tokenAddresses.add('0x326C977E6efc84E512bB9C30f76E30c160eD06FB');
final result = await EvmService.getPrice(tokenAddresses, currencies);
```

### Get smart account

Make a `SmartAccountConfig` object, then call `EvmService.getSmartAccount` the result value should be a smart account.

```dart
try {
    final eoaAddress = await ParticleAuth.getAddress();
    SmartAccountConfig config = SmartAccountConfig(
        AccountName.BICONOMY, VersionNumber.V1_0_0(), eoaAddress);
    List<dynamic> response =
        await EvmService.getSmartAccount(<SmartAccountConfig>[config]);

    var smartAccountJson = response.firstOrNull;
    if (smartAccountJson != null) {
      print(smartAccountJson);
      final smartAccount = smartAccountJson as Map<String, dynamic>;

      final smartAccountAddress =
          smartAccount["smartAccountAddress"] as String;

      print("getSmartAccount: $smartAccountAddress");
      showToast("getSmartAccount: $smartAccountAddress");
    } else {
      print('List is empty');
    }
  } catch (error) {
    print("getSmartAccount: $error");
    showToast("getSmartAccount: $error");
  }
```

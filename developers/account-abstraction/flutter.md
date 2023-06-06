# Flutter

### 1.Add the Biconomy Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_biconomy
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-biconomy) to get the demo source code&#x20;

### 2.Add Particle Auth or ParticleConnect to your project

biconomy service can't use individually.



### Initialize the SDK

**Before using the SDK, you have to call init(Required)**&#x20;

```dart
const version = BiconomyVersion.v1_0_0;
Map<int, String> dappKeys = {
  1: "your ethereum mainnet key",
  5: "your ethereum goerli key",
  137: "your polygon mainnet key",
  80001: "your polygon testnet key"
};
ParticleBiconomy.init(version, dappKeys);
```

### Is support chainInfo

check if support the chainInfo

```dart
var result = await ParticleBiconomy.isSupportChainInfo(EthereumChain.mainnet());
```

### Is deploy AA wallet

```dart
const eoaAddress = "";
var result = await ParticleBiconomy.isDeploy(eoaAddress);

final status = jsonDecode(result)["status"];
final data = jsonDecode(result)["data"];
if (status == true || status == 1) {
  var isDelpoy = jsonDecode(result)["data"];
  print(isDelpoy);
} else {
  final error = RpcError.fromJson(data);
  print(error);
}
```

### Is biconomy mode enable

```dart
var result = await ParticleBiconomy.isBiconomyModeEnable();
print(result);
```

### Enable biconomy mode

```dart
ParticleBiconomy.enableBiconomyMode();
```

### Disable biconomy mode

```dart
ParticleBiconomy.disableBiconomyMode();
```

### Rpc get fee quotes,&#x20;

Always used with send Transaciton, pick one feeQuote then send transaction in custom feeMode.

```dart
final publicAddress = await ParticleAuth.getAddress();
final transaction = await TransactionMock.mockEvmSendNative(publicAddress);

List<String> transactions = <String>[transaction];
var result =
    await ParticleBiconomy.rpcGetFeeQuotes(publicAddress, transactions);
print(result);
```

### SignAndSendTransaction with AuthService

```dart
final publicAddress = await ParticleAuth.getAddress();
final transaction = await TransactionMock.mockEvmSendNative(publicAddress);

// send transaction in auto mode, auto means use native to pay gas fee.
final signature = await ParticleAuth.signAndSendTransaction(transaction,
    feeMode: BiconomyFeeMode.auto());
    
// send transaction in gasless mode, gasless means user dont need to pay gas fee. 
final signature = await ParticleAuth.signAndSendTransaction(transaction,
        feeMode: BiconomyFeeMode.gasless());
        
// send transaction in custom mode, custom means user pick one token or native to pay gas fee. 
List<String> transactions = <String>[transaction];
var result = await ParticleBiconomy.rpcGetFeeQuotes(publicAddress, transactions);
// pick one quote 
var feeQuote = result[0];
final signature = await ParticleAuth.signAndSendTransaction(transaction,
        feeMode: BiconomyFeeMode.custom(feeQuote));
```

### Batch send transaction with AuthService

```dart
static void batchSendTransactions() async {
  final publicAddress = await ParticleAuth.getAddress();
  final transaction = await TransactionMock.mockEvmSendNative(publicAddress);
  // batch your transacitons into a list
  List<String> transactions = <String>[transaction, transaction];
  // support auto, gasless and custom feeMode.
  final signature = await ParticleAuth.batchSendTransactions(transactions, feeMode: BiconomyFeeMode.auto());
  showToast("signature $signature");
}
```

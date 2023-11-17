# Flutter

### 1. Add the Particle AA Service SDK to Your Flutter App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

With Flutter:

```dart
flutter pub add particle_aa
```

click [here](https://github.com/Particle-Network/particle-flutter/tree/master/particle-biconomy) to get the demo source code

### 2. Add Particle Auth or Particle Connect to your project

Particle AA service can't use individually.

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**

(Optional) The biconomyApiKeys comes from Biconomy, visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

```dart
Biconomy Api keys 
Map<int, String> biconomyApiKeys = {
  1: "your ethereum mainnet key",
  5: "your ethereum goerli key",
  137: "your polygon mainnet key",
  80001: "your polygon testnet key"
};
// Support BICONOMY | CYBERCONNECT | SIMPLE
ParticleAA.init(AccountName.BICONOMY, VersionNumber.V1_0_0(), biconomyApiKeys);
```

### Is deploy AA wallet

Check is a smart account address has been deployed by eoa address.

```dart
static void isDeploy() async {
  try {
    final eoaAddress = await ParticleAuth.getAddress();
    var isDeploy = await ParticleAA.isDeploy(eoaAddress);
    print("isDeploy: $isDeploy");
  } catch (error) {
    print("isDeploy: $error");
  }
}
```

### Is AA mode enable

```dart
var result = await ParticleAA.isAAModeEnable();
print(result);
```

### Enable AA mode

```dart
ParticleAA.enableAAMode();
```

### Disable AA mode

```dart
ParticleAA.disableAAMode();
```

## Send transaction

You should use particle-auth/particle-connect to send transaction, both of them have a function called `signAndSendTransaction` , a parameter called `feeMode` is used with AA service.

`feeMode` support native, gasless and token, just as its name implies, it tells how to pay gas fee.

### SignAndSendTransaction with Connect Service

Show how to send transaction with particle-connect, use native token to pay gas fee.

```dart
static void signAndSendTransactionWithNative() async {
    if (account == null) {
      print("not connect");
      return;
    }
    try {
      final transaction =
          await TransactionMock.mockEvmSendNative(account!.publicAddress);

      // check if enough native for gas fee
      var result = await ParticleAA.rpcGetFeeQuotes(
          account!.publicAddress, [transaction]);
      var verifyingPaymasterNative = result["verifyingPaymasterNative"];
      var feeQuote = verifyingPaymasterNative["feeQuote"];
      var fee = BigInt.parse(feeQuote["fee"], radix: 10);
      var balance = BigInt.parse(feeQuote["balance"], radix: 10);

      if (balance < fee) {
        print("native balance if not enough for gas fee");
        return;
      }

      // pass result from rpcGetFeeQuotes to send pay with native

      final signature = await ParticleConnect.signAndSendTransaction(
          WalletType.metaMask, account!.publicAddress, transaction,
          feeMode: AAFeeMode.native(result));
      print("signature $signature");
    } catch (error) {
      print("signAndSendTransactionWithNative: $error");
    }
}
```

Show  how to send transaction with particle-connect, gasless.

```dart
static void signAndSendTransactionWithGasless() async {
    if (account == null) {
      print("not connect");
      return;
    }
    try {
      final transaction =
          await TransactionMock.mockEvmSendNative(account!.publicAddress);

      // check if gasless available
      var result = await ParticleAA.rpcGetFeeQuotes(
          account!.publicAddress, [transaction]);
      var verifyingPaymasterGasless = result["verifyingPaymasterGasless"];
      if (verifyingPaymasterGasless == null) {
        print("gasless is not available");
        return;
      }

      // pass result from rpcGetFeeQuotes to send gasless

      final signature = await ParticleConnect.signAndSendTransaction(
          WalletType.metaMask, account!.publicAddress, transaction,
          feeMode: AAFeeMode.gasless(result));
      print("signature $signature");
    } catch (error) {
      print("signAndSendTransactionWithGasless: $error");
    }
}
```

Show  how to send transaction with particle-connect, use token to pay gas fee.

```dart
static void signAndSendTransactionWithToken() async {
  if (account == null) {
    print("not connect");
    return;
  }
  try {
    final transaction =
        await TransactionMock.mockEvmSendNative(account!.publicAddress);

    List<String> transactions = <String>[transaction];

    var result = await ParticleAA.rpcGetFeeQuotes(
        account!.publicAddress, transactions);

    List<dynamic> feeQuotes = result["tokenPaymaster"]["feeQuotes"];

    var overFeeQuotes = feeQuotes.where((element) {
      var fee = BigInt.parse(element["fee"], radix: 10);
      var balance = BigInt.parse(element["balance"], radix: 10);
      return balance >= fee;
    }).toList();

    if (overFeeQuotes.isEmpty) {
      print("no valid token fro gas fee");
      return;
    }

    var feeQuote = overFeeQuotes[0];
    String tokenPaymasterAddress =
        result["tokenPaymaster"]["tokenPaymasterAddress"];

    print("feeQuote $feeQuote");
    print("tokenPaymasterAddress $tokenPaymasterAddress");

    final signature = await ParticleConnect.signAndSendTransaction(
        WalletType.metaMask, account!.publicAddress, transaction,
        feeMode: AAFeeMode.token(feeQuote, tokenPaymasterAddress));
    print("signature $signature");
  } catch (error) {
    print("signAndSendTransactionWithToken: $error");
  }
}
```

{% hint style="info" %}
If you are using particle-auth to send transaction, the difference is which to call `signAndSendTransaction` method.
{% endhint %}

### Rpc get fee quotes

Use with send transaction, fill parameter `feeMode`.

```dart
static void rpcGetFeeQuotes() async {
  if (account == null) {
    print("not connect");
    return;
  }
  try {
    final transaction =
        await TransactionMock.mockEvmSendNative(account!.publicAddress);
    List<String> transactions = <String>[transaction];
    var result = await ParticleAA.rpcGetFeeQuotes(
        account!.publicAddress, transactions);
    print("rpcGetFeeQuotes: $result");
  } catch (error) {
    print("rpcGetFeeQuotes: $error");
  }
}
```

### Batch send transactions

Merge multiple transactions and send them together

```dart
static void batchSendTransactions() async {
  if (account == null) {
    print("not connect");
    return;
  }

  try {
    final transaction =
        await TransactionMock.mockEvmSendNative(account!.publicAddress);

    List<String> transactions = <String>[transaction, transaction];

    // check if enough native for gas fee
    var result = await ParticleAA.rpcGetFeeQuotes(
        account!.publicAddress, transactions);
    var verifyingPaymasterNative = result["verifyingPaymasterNative"];
    var feeQuote = verifyingPaymasterNative["feeQuote"];
    var fee = BigInt.parse(feeQuote["fee"], radix: 10);
    var balance = BigInt.parse(feeQuote["balance"], radix: 10);

    if (balance < fee) {
      print("native balance if not enough for gas fee");
      return;
    }

    final signature = await ParticleConnect.batchSendTransactions(
        WalletType.metaMask, account!.publicAddress, transactions,
        feeMode: AAFeeMode.native(result));
    print("signature $signature");
    showToast("signature $signature");
  } catch (error) {
    print("batchSendTransactions: $error");
    showToast("batchSendTransactions: $error");
  }
}
```

{% hint style="info" %}
If you are using particle-auth to send transaction, the difference is which to call `batchSendTransactions` method.
{% endhint %}

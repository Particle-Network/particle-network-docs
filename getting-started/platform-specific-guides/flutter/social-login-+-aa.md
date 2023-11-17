# Social Login + AA

This guide will help you get started with Particle Connect and Particle AA Flutter SDK by setting up your environment, creating a wallet with social account, and sending a User Operation from it. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

Before read the guide, you should read [last guide](social-login-+-embeded-wallet.md), understand how to connect, AA service should be used with Connect Service.

**Now, let's start with last guide example.**

## Installation

Install ParticleConnect Flutter SDK using pub.

```
flutter pub add particle_aa
```

### Initialization aa in flutter

```dart
void _initAA() {
  ParticleAA.init(
      AccountName.BICONOMY, VersionNumber.V1_0_0(), <int, String>{});
  ParticleAA.enableAAMode();
}
```

After get account, you can get smart account address, then each time you send transaction, you should use smart account address to create transaction.

```dart
void _getSmartAccountAddress() async {
  if (account == null) {
    print("didn't connect any account");
    return;
  }
  try {
    List<dynamic> response =
        await EvmService.getSmartAccount(<SmartAccountConfig>[
      SmartAccountConfig(AccountName.BICONOMY, VersionNumber.V1_0_0(),
          account!.publicAddress)
    ]);
    var smartAccountJson = response.firstOrNull;
    if (smartAccountJson != null) {
      final smartAccount = smartAccountJson as Map<String, dynamic>;

      final smartAccountAddress =
          smartAccount["smartAccountAddress"] as String;
      this.smartAccountAddress = smartAccountAddress;
      print("getSmartAccount: $smartAccountAddress");
    } else {
      print('List is empty');
    }
  } catch (error) {
    print("get smart account address $error");
  }
}
```

Let's send a transaction in AA, it use the same method `signAndSendTransaction`.

Show how to send transaction in AA mode and use native token to pay gas fee.

```dart
void _sendTransaction() async {
    if (account == null) {
      print("didn't connect any account");
      return;
    }
    try {
      final walletType = WalletType.values.firstWhere((element) =>
          element.name.toLowerCase() == account!.walletType!.toLowerCase());
      final transaction = await EvmService.createTransaction(
          account!.publicAddress,
          "0x",
          BigInt.from(1000000000000000),
          "0x0000000000000000000000000000000000000000");
      var wholeFeeQuote = await ParticleAA.rpcGetFeeQuotes(
          account!.publicAddress, [transaction]);
      var verifyingPaymasterNative = wholeFeeQuote["verifyingPaymasterNative"];
      var feeQuote = verifyingPaymasterNative["feeQuote"];
      var fee = BigInt.parse(feeQuote["fee"], radix: 10);
      var balance = BigInt.parse(feeQuote["balance"], radix: 10);

      if (balance < fee) {
        print("native balance if not enough for gas fee");
        return;
      }
      final signature = await ParticleConnect.signAndSendTransaction(
          walletType, account!.publicAddress, transaction,
          feeMode: AAFeeMode.native(wholeFeeQuote));
      print("send transaction $signature");
    } catch (error) {
      print("send transaction $error");
    }
  }
```

{% hint style="info" %}
`AAFeeMode` also support gasless and token to pay gas fee.
{% endhint %}

## Dive Deeper

In this guide, we learned how to use the Particle AA Flutter SDK for getting smart account, sending transactions. If you want to learn more about different use cases, check out the [Particle A](../../../developers/connect-service/sdks/flutter.md)[A](../../../developers/account-abstraction/flutter.md) page.

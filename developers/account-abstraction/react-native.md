# React Native

### 1. Add the Account Abstraction Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install @particle-network/rn-aa
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-biconomy) to get the demo source code

### 2. Add Particle Auth or Particle Connect to your project

Account Abstraction service can't use individually.

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**

(Optional) The biconomyApiKeys comes from Biconomy, visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

```typescript
import * as particleAA from '@particle-network/rn-aa';
import { AAVersion } from '@particle-network/rn-auth';

const biconomyApiKeys = {
      1: 'your ethereum mainnet key',
      5: 'your ethereum goerli key',
      137: 'your polygon mainnet key',
      80001: 'your polygon testnet key'
}
// Support BICONOMY_V1 | BICONOMY_V2 | CYBERCONNECT | SIMPLE
particleAA.init(AccountName.BICONOMY_V1(), biconomyApiKeys);
```

### Is deploy AA wallet

```dart
const eoaAddress = "";
const result = await particleAA.isDeploy(eoaAddress);

if (result.status) {
    const isDeploy = result.data;
    console.log('isDeploy result', isDeploy);
} else {
    const error = result.data;
    console.log('isDeploy result', error);
}
```

### Is AA mode enable

```dart
const result = await particleAA.isAAModeEnable();
```

### Enable AA mode

```dart
particleAA.enableAAMode();
```

### Disable AA mode

```dart
particleAA.disableAAMode();
```

### Send transaction

Before send transaction, you should get your smart account address first.

```typescript
const smartAccountParam = {
      name: this.accountName.name,
      version: this.accountName.version,
      ownerAddress: eoaAddress,
};
const result: SmartAccountInfo[] = await EvmService.getSmartAccount([smartAccountParam]);
const smartAccountAddress = result[0]?.smartAccountAddress;
```

An example send a transaction with particleAuth in AA mode.

Generally, there are 5 steps to send a transaction or multi transactions in AA mode.

If you are using particleConnect, call particleConnect.sendAndSendTransaction method to sign.

If you are using particleAuthCore, call evm.sendTransaction method to sign.

In step 4, you can select gasless, native or token to pay gas fee.

```dart
const eoaAddress = await particleAuth.getAddress();
// 1. get your smart account address from eoaAdddress
const smartAccountAddress = await this.getSmartAccountAddress(eoaAddress);
if (smartAccountAddress == undefined) {
  return;
}
const receiver = TestAccountEVM.receiverAddress;
const amount = TestAccountEVM.amount;
// 2. create transaction or transactions
const transaction = await Helper.getEthereumTransacion(
  smartAccountAddress,
  receiver,
  BigNumber(amount)
);

// 3. get whole fee quote
const wholeFeeQuote = (await particleAA.rpcGetFeeQuotes(eoaAddress, [
  transaction,
])) as WholeFeeQuote;

console.log('wholeFeeQuote', wholeFeeQuote);

// 4.1 use native to pay gas, 
const feeQuote = wholeFeeQuote.verifyingPaymasterNative.feeQuote;
const fee = BigNumber(feeQuote.fee);
const balance = BigNumber(feeQuote.balance);
// check is native balance is enough
if (balance.isLessThan(fee)) {
  console.log('native balance if not enough for gas fee');
  return;
}

// 4.2 use token to pay gas
const feeQuotes = wholeFeeQuote.tokenPaymaster.feeQuotes as any[];
const validFeeQuotes = feeQuotes.filter((item) => {
  const fee = BigNumber(item.fee);
  const balance = BigNumber(item.balance);
  if (balance.isLessThan(fee)) {
    return false;
  } else {
    return true;
  }
});

if (validFeeQuotes.length == 0) {
  console.log('no valid token for gas fee');
  return;
}

const feeQuote = validFeeQuotes[0];
const tokenPaymasterAddress = wholeFeeQuote.tokenPaymaster
  .tokenPaymasterAddress as string;
  
// 4.3 use gasless
const verifyingPaymasterGasless = wholeFeeQuote.verifyingPaymasterGasless;
if (verifyingPaymasterGasless == undefined) {
  console.log('gasless is not available');
  return;
}

// 5.1 use native to pay gas fee, pass back wholeFeeQuote
const result = await particleAuth.signAndSendTransaction(
  transaction,
  AAFeeMode.native(wholeFeeQuote)
);

// 5.2 use token to pay gas fee, pass back selected feeQuote and tokenPaymasterAddress
const result = await particleAuth.signAndSendTransaction(
  transaction,
  AAFeeMode.token(feeQuote, tokenPaymasterAddress)
);

// 5.3 use gasless, pass back wholeFeeQuote
const result = await particleAuth.signAndSendTransaction(
  transaction,
  AAFeeMode.gasless(wholeFeeQuote)
);


if (result.status) {
  const signature = result.data;
  console.log('signAndSendTransactionWithNative result', signature);
} else {
  const error = result.data as CommonError;
  console.log('signAndSendTransactionWithNative result', error);
}
```

### Batch send transactions

Batch send transactions is similar with send one transaction.

The difference is call batchSendTransactions method.

```typescript
const result = await particleAuth.batchSendTransactions(
    transactions,
    AAFeeMode.native(wholeFeeQuote)
);
```

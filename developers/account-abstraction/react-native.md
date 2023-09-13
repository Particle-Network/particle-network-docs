# React Native

### 1. Add the Account Abstraction Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install @particle-network/rn-aa
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-biconomy) to get the demo source code&#x20;

### 2. Add Particle Auth or Particle Connect to your project

Account Abstraction service can't use individually.

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**&#x20;

```typescript
import * as particleAA from '@particle-network/rn-aa';
import { AAVersion } from '@particle-network/rn-auth';

const dappAppKeys = {
      1: 'your ethereum mainnet key',
      5: 'your ethereum goerli key',
      137: 'your polygon mainnet key',
      80001: 'your polygon testnet key'
}
particleAA.init(AAVersion.v1_0_0, dappAppKeys);
```

### Is support chainInfo

check if support the chainInfo

```dart
const result = await particleAA.isSupportChainInfo(ChainInfo.BSCMainnet);
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

### Rpc get fee quotes

Always used with send Transaction, pick one feeQuote then send transaction in custom feeMode.

```dart
const eoaAddress = await particleAuth.getAddress();
console.log('eoaAddress', eoaAddress);
const receiver = TestAccountEVM.receiverAddress;
const amount = TestAccountEVM.amount;
const transaction = await Helper.getEthereumTransacion(eoaAddress, receiver, amount);

console.log('transaction', transaction);
const result = await particleAA.rpcGetFeeQuotes(eoaAddress, [transaction]);
```

### SignAndSendTransaction with Auth Service

```dart
const eoaAddress = await particleAuth.getAddress();
const receiver = TestAccountEVM.receiverAddress;
const amount = TestAccountEVM.amount;
const transaction = await Helper.getEthereumTransacion(eoaAddress, receiver, amount);

// send transaction in auto mode, auto means use native to pay gas fee.
const result = await particleAuth.signAndSendTransaction(transaction, AAFeeMode.auto())
    
// send transaction in gasless mode, gasless means user dont need to pay gas fee. 
const result = await particleAuth.signAndSendTransaction(transaction, AAFeeMode.gasless())
        
// send transaction in custom mode, custom means user pick one token or native to pay gas fee. 
const feeQutotes = await particleAA.rpcGetFeeQuotes(eoaAddress, [transaction]);
// pick one quote 
const result = await particleAuth.signAndSendTransaction(transaction, AAFeeMode.custom(feeQutotes[0]))

if (result.status) {
    const signature = result.data;
    console.log('signAndSendTransaction result', signature);
} else {
    const error = result.data;
    console.log('signAndSendTransaction result', error);
}
```

### SignAndSendTransaction with Connect Service

```dart
// confirm your eoa public address and current wallet type
const publicAddress = '';
const walletType = WalletType.MetaMask;

// send transaction in auto mode, auto means use native to pay gas fee.
const result = await particleConnect.signAndSendTransaction(this.walletType, this.publicAddress, transaction, 
    
// send transaction in gasless mode, gasless means user dont need to pay gas fee. 
const result = await particleConnect.signAndSendTransaction(this.walletType, this.publicAddress, transaction, AAFeeMode.gasless())

// send transaction in custom mode, custom means user pick one token or native to pay gas fee. 
const feeQutotes = await particleAA.rpcGetFeeQuotes(eoaAddress, [transaction]);
const result = await particleConnect.signAndSendTransaction(this.walletType, this.publicAddress, transaction, AAFeeMode.custom(feeQutotes[0]))


if (result.status) {
    const signature = result.data;
    console.log('signature', signature);
} else {
    const error = result.data;
    console.log('result error', error);
}
```

### Batch send transaction with Auth Service

```dart
const eoaAddress = await particleAuth.getAddress();
const receiver = TestAccountEVM.receiverAddress;
const amount = TestAccountEVM.amount;
const transaction = await Helper.getEthereumTransacion(eoaAddress, receiver, amount);

// batch your transacitons into a list
const transactions = [transaction, transaction];

// support auto, gasless and custom feeMode.
const result = await particleAuth.batchSendTransactions(transactions, AAFeeMode.auto);
if (result.status) {
    const signature = result.data;
    console.log('signAndSendTransaction result', signature);
} else {
    const error = result.data;
    console.log('signAndSendTransaction result', error);
}
```

### Batch send transaction with Connect Service

```javascript
// confirm your eoa public address and current wallet type
const publicAddress = '';
const walletType = WalletType.MetaMask;
// get your transaction array
const transactions = [transaction, transaction];

const result = await particleConnect.batchSendTransactions(this.walletType, this.publicAddress, transactions, AAFeeMode.auto());
if (result.status) {
    const signature = result.data;
    console.log('signature', signature);
} else {
    const error = result.data;
    console.log('result error', error);
}
```

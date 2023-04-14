---
description: Power up your Cocos games with Particle Network SDKs.
---

# Cocos

Before using, please review [the prerequisites](../../../getting-started/platform-specific-guides/cocos/cocos-sdk-prerequisites.md#prerequisites), All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### Usage

```javascript
import * as particleAuth from '../../Core/particleAuth';
```

### Initialize the SDK

**Before using the sdk you have to call init(Required)**&#x20;

```typescript
const chainInfo = EvmService.currentChainInfo;
const env = Env.Dev;
particleAuth.init(chainInfo, env);
```

### Web3 provider

you can use our SDK as a web3 provider, The web3 provider requires the web3.js library and does not support Solana.

```javascript
private web3 = createWeb3();

async web3_getAccounts() {
    try {
        const accounts = await this.web3.eth.getAccounts();
        console.log('web3.eth.getAccounts', accounts);
    } catch (error) {
        console.log('web3.eth.getAccounts', error);
    }
};

async web3_getBalance() {
    try {
        const accounts = await this.web3.eth.getAccounts();
        const balance = await this.web3.eth.getBalance(accounts[0]);
        console.log('web3.eth.getBalance', balance);
    } catch (error) {
        console.log('web3.eth.getBalance', error);
    }
};

async web3_getChainId() {
    try {
        const chainId = await this.web3.eth.getChainId();
        console.log('web3.eth.getChainId', chainId);
    } catch (error) {
        console.log('web3.eth.getChainId', error);
    }
};

async web3_personalSign() {
    try {
        // for persion_sign
        // don't use web3.eth.personal.sign
        const result = await this.web3.currentProvider.request({
            method: 'personal_sign',
            params: ['hello world'],
        });

        console.log('web3.eth.personal.sign', result);
    } catch (error) {
        console.log('web3.eth.personal.sign', error);
    }
};

async web3_signTypedData_v1() {
    try {
        const accounts = await this.web3.eth.getAccounts();
        const result = await this.web3.currentProvider.request({
            method: 'eth_signTypedData_v1',
            params: [
                [your typed data v1],
                accounts[0],
            ],
        });
        console.log('web3 eth_signTypedData_v1', result);
    } catch (error) {
        console.log('web3 eth_signTypedData_v1', error);
    }
};

async web3_signTypedData_v3() {
    try {
        const accounts = await this.web3.eth.getAccounts();
        const result = await this.web3.currentProvider.request({
            method: 'eth_signTypedData_v3',
            params: [
                accounts[0],
                {your typed data v3});
        console.log('web3 eth_signTypedData_v3', result);
    } catch (error) {
        console.log('web3 eth_signTypedData_v3', error);
    }
};

async web3_signTypedData_v4() {
    try {
        const accounts = await this.web3.eth.getAccounts();
        const result = await this.web3.currentProvider.request({
            method: 'eth_signTypedData_v4',
            params: [
                accounts[0],
                {your typed data v4});
        console.log('web3 eth_signTypedData_v4', result);
    } catch (error) {
        console.log('web3 eth_signTypedData_v4', error);
    }
};

async web3_sendTransaction() {
    try {
        const accounts = await this.web3.eth.getAccounts();
        const result = await this.web3.eth.sendTransaction({
            from: accounts[0],
            to: "0x39b2DeB155Ee6a5a23E172bE11744329e95Af6df",
            value: '1000000',
            data: '0x',
        });
        console.log('web3.eth.sendTransaction', JSON.stringify(result));
    } catch (error) {
        console.log('web3.eth.sendTransaction', error);
    }
};

async web3_wallet_switchEthereumChain() {
    const chainId = EvmService.currentChainInfo.chain_id;
    try {
        const result = await this.web3.currentProvider.request({
            method: 'wallet_switchEthereumChain',
            params: [{ chainId: "0x" + chainId.toString(16) }],
        });
        console.log('web3 wallet_switchEthereumChain', result);
    } catch (error) {
        console.log('web3 wallet_switchEthereumChain', error);
    }
};
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

### Fast logout

logout silently

```javascript
const result = await particleAuth.fastLogout();
if (result.status) {
    console.log(result.data);
} else {
    const error = result.data;
    console.log(error);
}
```

### Is Login

```javascript
const result = await particleAuth.isLogin();
console.log(result);
```

### Is Login Async

```typescript
const result = await particleAuth.isLoginAsync();
if (result.status) {
    const userInfo = result.data;
    console.log(userInfo);
} else {
    const error = result.data;
    console.log(error);
}
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

support both EVM and Solana.

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

support Solana only.

```dart
const chainInfo = await particleAuth.getChainInfo();
if (chainInfo.chain_name.toLowerCase() != "solana") {
    console.log("signTransaction only supports solana")
    return
}
const sender = await particleAuth.getAddress();
const to = "";
const amount = 1000;

console.log("sender: ", sender);
const transaction = await Helper.getSolanaTransaction(sender, to, amount);
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

support Solana only.

```dart
const chainInfo = await particleAuth.getChainInfo();
if (chainInfo.chain_name.toLowerCase() != "solana") {
    console.log("signAllTransactions only supports solana")
    return
}
const sender = await particleAuth.getAddress();
const to = "";
const amount = 1000;
const transaction1 = await Helper.getSolanaTransaction(sender, to, amount);
const transaction2 = await Helper.getSplTokenTransaction(sender, to, amount);
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

support both EVM and Solana.

```dart
const sender = await particleAuth.getAddress();
const chainInfo = EvmService.currentChainInfo;
const to = "";
const amount = 1000;
let transaction = "";
if (chainInfo.chain_name.toLowerCase() == "solana") { 
    transaction = await Helper.getSolanaTransaction(sender, to, amount);
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

support evm only.

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

### Set interface style

```typescript
let style = UserInterfaceStyle.Light;
particleAuth.setInterfaceStyle(style);
```

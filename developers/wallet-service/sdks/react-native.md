# React Native

### 1.Add the Wallet Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install @particle-network/rn-wallet
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-wallet) to get the demo source code

### 2.Configure Android project

is same with [Connect Service](../../connect-service/sdks/react-native.md)

### 3.Configure iOS project

is same with [Connect Service](../../connect-service/sdks/react-native.md)

### Initialize the SDK

### Should call particle connect init first

```typescript
const chainInfo = Ethereum;
const env = Env.Dev;
const dappMetaData = new DappMetaData('75ac08814504606fc06126541ace9df6',
    'Particle Connect',
    'https://connect.particle.network/icons/512.png',
    'https://connect.particle.network',
    'Particle Wallet', "", "");
particleConnect.init(chainInfo, env, dappMetaData);
```

{% hint style="info" %}
### Migrating to WalletConnect v2

Starting from version 0.14.0, WalletConnectV2 is supported.

```typescript
const walletMetaData = {
    walletConnectProjectId: '75ac08814504606fc06126541ace9df6',
    name: 'Particle Connect',
    icon: 'https://connect.particle.network/icons/512.png',
    url: 'https://connect.particle.network',
    description: 'Particle Wallet',
};

particleWallet.initWallet(walletMetaData);
```
{% endhint %}

### Navigator wallet

```javascript
const display = WalletDisplay.Token;
particleWallet.navigatorWallet(display);
```

### Navigator token receive

```javascript
const tokenAddress = TestAccountSolana.tokenContractAddress;
particleWallet.navigatorTokenReceive(tokenAddress);
```

### Navigator token send

```javascript
const tokenAddress = TestAccountSolana.tokenContractAddress;
const toAddress = TestAccountSolana.receiverAddress;
const amount = "1000000000";
particleWallet.navigatorTokenSend(tokenAddress, toAddress, amount);
```

### Navigator token transaction records

```javascript
const tokenAddress = TestAccountSolana.tokenContractAddress;
particleWallet.navigatorTokenTransactionRecords(tokenAddress);
```

### Navigator NFT send

```javascript
const mint = TestAccountEVM.nftContractAddress;
const receiverAddress = TestAccountEVM.receiverAddress;
const tokenId = TestAccountEVM.nftTokenId;
particleWallet.navigatorNFTSend(receiverAddress, mint, tokenId);
```

### Navigator NFT details

```dart
const mint = TestAccountEVM.nftContractAddress;
const tokenId = TestAccountEVM.nftTokenId;
particleWallet.navigatorNFTDetails(mint, tokenId);
```

### Set pay disabled

```dart
const disabled = true;
particleWallet.setPayDisabled(disabled);
```

### Get pay disabled

```dart
const result = await particleWallet.getPayDisabled();
console.log(result);
```

### Set swap disabled

```javascript
const disabled = true;
particleWallet.setSwapDisabled(disabled);
```

### Get swap swap

```javascript
const result = await particleWallet.getSwapDisabled();
console.log(result);
```

### Navigator buy crypto

```javascript
// support no parameters
particleWallet.navigatorBuyCrypto();

// also support pass public address, crypto symbol and so on.
const config = new BuyCryptoConfig("0xa0869E99886e1b6737A4364F2cf9Bb454FD637E4", "BNB", "USD", 1000, OpenBuyNetwork.BinanceSmartChain);
particleWallet.navigatorBuyCrypto(config);
```

### Navigator swap

```javascript
const fromTokenAddress = "";
const toTokenAddress = "";
const amount = "";
particleWallet.navigatorSwap(fromTokenAddress, toTokenAddress, amount);
```

### Navigator login list

```javascript
const result = await particleWallet.navigatorLoginList();
console.log(result)
```

### Set Support chain

```javascript
const chainInfos = [Ethereum, Polygon];
particleWallet.setSupportChain(chainInfos);
```

### Switch wallet

```javascript
WalletType walletType = WalletType.particle;
String publicAddress = "";
String result = await ParticleWallet.switchWallet(walletType, publicAddress);
print("result:$result");
```

### Set show test network

```javascript
const isShow = true;
particleWallet.setShowTestNetwork(isShow);
```

### Set show manage wallet

```javascript
const isShow = true;
particleWallet.setShowManageWallet(isShow);
```

### Custom UI

```javascript
setDisplayTokenAddresses = async () => {
    const tokenAddresses = ["", ""];
    particleWallet.setDisplayTokenAddresses(tokenAddresses);
};

setDisplayNFTContractAddresses = async () => {
    const nftContractAddresses = ["", ""];
    particleWallet.setDisplayNFTContractAddresses(nftContractAddresses);
};

setPriorityTokenAddresses = async () => {
    const tokenAddresses = ["", ""];
    particleWallet.setPriorityTokenAddresses(tokenAddresses);
};

setPriorityNFTContractAddresses = async () => {
    const nftContractAddresses = ["", ""];
    particleWallet.setPriorityNFTContractAddresses(nftContractAddresses);
};

setShowLanguageSetting = async () => {
    particleWallet.setShowLanguageSetting(false);
};

setShowAppearanceSetting = async () => {
    particleWallet.setShowAppearanceSetting(false);
};

setSupportAddToken = async () => {
    particleWallet.setSupportAddToken(false);
};

setSupportWalletConnect = async () => {
    particleWallet.setSupportWalletConnect(false);
};

// show or hide smart account setting in setting page
setShowSmartAccountSetting = async () => {
    particleWallet.setShowSmartAccountSetting(false);
};
```

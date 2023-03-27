# React Native

### 1.Add the Wallet Service SDK to Your React Native App <a href="#add-sdks" id="add-sdks"></a>

Run this command:

```dart
npm install react-native-particle-wallet
```

click [here](https://github.com/Particle-Network/particle-react-native/tree/master/particle-wallet) to get the demo source code&#x20;

### 2.Configure Android project

is same with [Connect Service ](../../connect-service/sdks/react-native.md)

### 3.Configure iOS project

is same with [Connect Service ](../../connect-service/sdks/react-native.md)

### Initialize the SDK

**Before using the SDK, you have to call init(Required)**&#x20;

```javascript
const chainInfo = ChainInfo.EthereumGoerli;
const env = Env.Dev;
const metadata = { name: "Particle Connect", icon: "https://connect.particle.network/icons/512.png", url: "https://connect.particle.network" }
const rpcUrl = { evm_url: null, solana_url: null };
particleConnect.init(chainInfo, env, metadata, rpcUrl);
particleWallet.initWallet()
```

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

### Navigator token send&#x20;

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

### Enable pay

```dart
const isEnable = true;
particleWallet.enablePay(isEnable);
```

### Get enable pay

```dart
const result = await particleWallet.getEnablePay();
console.log(result);
```

### Enable swap

```javascript
const isEnable = true;
particleWallet.enableSwap(isEnable);
```

### Get enable swap

```javascript
const result = await particleWallet.getEnableSwap();
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
const chainInfos = [ChainInfo.EthereumMainnet, ChainInfo.BscMainnet, ChainInfo.PolygonMainnet];
particleWallet.supportChain(chainInfos);
```

### Switch wallet

```javascript
WalletType walletType = WalletType.particle;
String publicAddress = "";
String result = await ParticleWallet.switchWallet(walletType, publicAddress);
print("result:$result");
```

### Show test network

```javascript
const isShow = true;
particleWallet.showTestNetwork(isShow);
```

### Show manage wallet

```javascript
const isShow = true;
particleWallet.showManageWallet(isShow);
```

### Custom UI

```javascript
setLanguage = async () => {
    const language = Language.JA;
    particleWallet.setLanguage(language);
}

setFiatCoin = async () => {
    const faitCoin = FaitCoin.HKD;
    particleWallet.setFiatCoin(faitCoin);
}

setDisplayTokenAddresses = async () => {
    const tokenAddresses = ["", ""];
    particleWallet.setDisplayTokenAddresses(tokenAddresses);
}

setDisplayNFTContractAddresses = async () => {
    const nftContractAddresses = ["", ""];
    particleWallet.setDisplayNFTContractAddresses(nftContractAddresses);
}

setPriorityTokenAddresses = async () => {
    const tokenAddresses = ["", ""];
    particleWallet.setPriorityTokenAddresses(tokenAddresses);
}

setPriorityNFTContractAddresses = async () => {
    const nftContractAddresses = ["", ""];
    particleWallet.setPriorityNFTContractAddresses(nftContractAddresses);
}

showLanguageSetting = async () => {
    particleWallet.showLanguageSetting(false);
}

showAppearanceSetting = async () => {
    particleWallet.showAppearanceSetting(false);
}

setSupportAddToken = async () => {
    particleWallet.setSupportAddToken(false);
}
```

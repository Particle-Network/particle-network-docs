---
description: How to connect with third wallet, take MetaMask for example.
---

# FAQ

1\. Get MetaMask connect adapter, you can get account in success call back.

```swift
// swift
let adapter = ParticleConnect.getAllAdapters()
                .filter { $0.walletType == .metamask }.first!
adapter.connect()

// flutter
final result = await ParticleConnect.connect(WalletType.metamask);
```

2\. Sign something, for example sign message, you can get signed message in success call back.

```swift
// we request public address because a adapter can connect more than one address.

// swift
adapter.signMessage(publicAddress: address, message: message)

// flutter
String result = await ParticleConnect.signMessage(WalletType.metamask, getPublicAddress(), "Hello Particle");

```

3\. Add chain, MetaMask support add chain, other wallets don't support this methods, you can get success string in success call back.

{% hint style="info" %}
MetaMask doesn't support add ethereum testnet, such as goerli, instead support switch to goerli.

MetaMask doesn't support add or switch optimism mainnet,

if you want to add or switch other chain, just call add chain is enough, MetaMask has optimized, when you call add chain, it will present add pop-up, after confirm, it will present switch pop-up.
{% endhint %}

```swift
// swift
adapter.addEthereumChain(publicAddress: publicAddress, chainId: chainId, chainName: nil, nativeCurrency: nil, rpcUrl: nil, blockExplorerUrl: nil)

// flutter
String result = await ParticleConnect.addEthereumChain(walletType, getPublicAddress(), chainId);
```

4\. Switch chain, MetaMask support add chain, other wallets don't support this methods, you can get success string in success call back.

```swift
// swift
adapter.switchEthereumChain(publicAddress: publicAddress, chainId: chainId)

// flutter
String result = await ParticleConnect.switchEthereumChain(walletType, getPublicAddress(), chainId);
```

5\. Sometimes, user metamask app is in ethereum.mainnet, but you want to send some bsc.mainnet tokens, I think it's time to call add(switch) wallet before send tokens, but it is not requested by our SDK, and it will jump to metamask app twice, we suggest make a delay before two jump.

6\. When call add(switch) chain? before signAndSendTransaction? after connect success?

It is determined by yourself.

In our iOS GUI SDK, we call add(switch) chain before signAndSendTransaction, make sure matamask is the same chain id with the transaction, if transaction's chain id not same with metamask, metamask will reject it, after add(switch) chain success, delay 0.3 second, call signAndSendTransaction.




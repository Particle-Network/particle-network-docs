# Contract Interaction

This guide will help you get started with Particle Wallet API iOS SDK by setting up your environment, read contract and write contract, create EVM transaction. By the end of this guide, you'll have a basic understanding of how to use the SDK and where to look for more advanced user cases.

If you face any problem anytime, you can always refer to the Particle[ ](https://github.com/Web3Auth/Web3Auth/discussions)Community for us to help you out as soon as possible!

[Download materials](https://github.com/Particle-Network/particle-ios/tree/main/GuideSeries/ParticleConnectExample)

## Read Contract&#x20;

If you want to read contract.

There is a convenient method `ParticleWalletAPI.getEvmService().readContract` .

Show how to read `balanceOf` from a contract address.

Note The result of write contract must be a hex string, you need convert it your you type, we know  `balanceOf` is a number, we can convert the hex string to a BigNumber type.

```swift
@IBAction func readContract() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }

    let contractAddress = "0x326C977E6efc84E512bB9C30f76E30c160eD06FB"
    let methodName = "balanceOf"
    // each element of params should be presented as a hex string, 
    // for example, you should convert your int to a hex string, start with 0x
    let params = [account.publicAddress]
    let contractParams = ContractParams.customAbiEncodeFunctionCall(contractAddress: contractAddress, methodName: methodName, params: params)
    ParticleWalletAPI.getEvmService().readContract(contractParams: contractParams).subscribe { result in
        switch result {
        case .failure(let error):
            print(error)
        case .success(let json):
            let string = json.stringValue
            // because the method name is `balanceOf`
            // we can resolve the string as BInt
            let balance = BInt(string, radix: 16)
            print(balance)
            
            // and we know the token decimals is 18
            let number = balance?.convertToBalance(decimals: 18)
            print(number)
            
        }
    }.disposed(by: self.bag)
}
```

## Write Contract

Write contract is a important way to interact with a conrtact.

There is a convenient method `ParticleWalletAPI.getEvmService().wirteContract` .

Note write contract must return a transaction string, then you and sign and send it.

```swift
@IBAction func writeContract() {
    guard let account = self.account else {
        print("you didn't connect any account")
        return
    }

    let adapter = ParticleConnect.getAllAdapters().filter {
        $0.walletType == account.walletType
    }.first!

    let contractAddress = "0x9B1AAb1492c375F011811cBdBd88FFEf3ce2De76"
    let methodName = "mint"
    // each element of params should be presented as a hex string,
    // for example, you should convert your int to a hex string, start with 0x
    let params = ["0x3"]

    let contractParams = ContractParams.customAbiEncodeFunctionCall(contractAddress: contractAddress, methodName: methodName, params: params)
    ParticleWalletAPI.getEvmService().writeContract(contractParams: contractParams, from: account.publicAddress).subscribe { result in
        switch result {
        case .failure(let error):
            print(error)
        case .success(let transaction):
            print(transaction)
            // here you get the transaction
            // you can sign and send now.
        }
    }.disposed(by: self.bag)
}
```

## Watching a transaction hash

When you successfully send a transaction, you will get a transaction signature, and you can observe the result of the transaction being executed on the blockchain

```swift
@IBAction func getTransactionReceipt() {
    let hash = "0x639c16e24acf8a39536051dba38c13fe28d6675b48548c49095e59e639071c03"
    
    ParticleWalletAPI.getEvmService().eth_getTransactionReceipt(txHash: hash).subscribe { result in
        switch result {
        case .failure(let error):
            print(error)
        case .success(let receipt):
            print(receipt)
        }
    }.disposed(by: self.bag)
}
```

{% hint style="info" %}
We provide the full EVM basic and enhanced`methods`in `ParticleWalletAPI.getEvmService().`
{% endhint %}

## Dive Deeper

In this guide, we learned how to use the Particle Connect iOS SDK for social login, sending User Operation. If you want to learn more about different use cases, check out the [Wallet Service](../../../developers/wallet-service/sdks/ios.md) page.






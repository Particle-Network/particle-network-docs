# Transaction Construction

To construct EVM transactions using the provided code, you can follow the examples below. Make sure to replace placeholders with your actual addresses, amounts, and other relevant information.

**Gradle Dependencies:**

```gradle
gradleCopy codedependencies {
    //...
    implementation "network.particle:api-service:{latest-version}"
}
```

Find the latest version of the SDK [here](https://search.maven.org/search?q=g:network.particle).

**Example Code:**

1.  **ERC20 Transfer (Native):**

    ```kotlin
    kotlinCopy codeval amount = "0x38d7ea4c68000" // Hexadecimal string
    val serializeTransaction = ParticleNetwork.evm.createTransaction(from, to, amount).serialize()
    ```
2.  **ERC20 Transfer (Token):**

    ```kotlin
    kotlinCopy codeval amount = "1000000000000000" // Decimal string
    val contractParams = ContractParams.erc20Transfer(tokenContractAddress, to, amount)
    val serializeTransaction = ParticleNetwork.evm.createTransaction(from, tokenContractAddress, contractParams).serialize()
    ```
3.  **ERC721 SafeTransferFrom:**

    ```kotlin
    kotlinCopy codeval tokenContractAddress = "0x76Afcd13B9B7D93Fb3fb8D42dDF68bEe4CEF9765"
    val contractParams = ContractParams.erc721SafeTransferFrom(tokenContractAddress, from, to, tokenId)
    val serializeTransaction = ParticleNetwork.evm.createTransaction(from, tokenContractAddress, contractParams).serialize()
    ```
4.  **ERC1155 SafeTransferFrom:**

    ```kotlin
    kotlinCopy codeval tokenContractAddress = "0xA07e45A987F19E25176c877d98388878622623FA"
    val amount = "1" // Decimal string
    val id = "123"
    val contractParams = ContractParams.erc1155SafeTransferFrom(
        tokenContractAddress,
        from,
        to,
        id,
        amount,
        data = "0x"
    )
    val serializeTransaction = ParticleNetwork.evm.createTransaction(from, tokenContractAddress, contractParams).serialize()
    ```
5.  **Write Contract:**

    ```kotlin
    kotlinCopy codeval params = ContractParams.customAbiEncodeFunctionCall(
        contractAddress = "0xd000f000aa1f8accbd5815056ea32a54777b2fc4",
        methodName = "mint",
        params = listOf("1"),
        abiJsonString = ""
    )
    val serializeTransaction = ParticleNetwork.evm.writeContract(ParticleNetwork.getAddress(), params).serialize()
    ```



The above code needs to handle exceptions


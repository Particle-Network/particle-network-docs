---
description: Fully compatible with EIP-4337 standard
---

# Bundler RPC

Powered by Particle Network's Bundler server, we provide a fast and stable bundler server. Fully compatible with EIP-4337 standard. And a lot of outstanding features:

1. UserOp Persistence
2. UserOp Concurrent Handling
3. Built-in gas price oracle
4. Multi Bundler Signers Manager
5. Auto fulfill bundler singers’ balance
6. Auto Retry for failed transaction from bundler signer account
7. Returns the correct Transaction even when affected by MEV
8. ...

For details please check our open-source bundler implementation: [https://github.com/Particle-Network/particle-bundler-server](https://github.com/Particle-Network/particle-bundler-server)

RPC Standard: [https://eips.ethereum.org/EIPS/eip-4337#rpc-methods-eth-namespace](https://eips.ethereum.org/EIPS/eip-4337#rpc-methods-eth-namespace)\


## Supported Chains

1. Ethereum Mainnet, Goerli & Sepolia
2. Polygon Mainnet & Mumbai Testnet
3. Optimism Mainnet & Optimism Goerli Testnet
4. Arbitrum One & Nova & Goerli Testnet
5. Scroll Mainnet & Sepolia Testnet
6. Linea Mainnet & Testnet
7. Base Mainnet & Testnet
8. Mantle Mainnet & Testnet
9. Manta Mainnet & Testnet
10. Taiko Jolnir Testnet
11. Avalanche Mainnet & Fuji Testnet
12. BNB Chain Mainnet & Testnet
13. opBNB Mainnet & Testnet
14. Combo Testnet
15. ... more chains are coming

## Endpoint

**Base URL: `https://bundler.particle.network`**

**Add chainId to the URL:** `https://bundler.particle.network?chainId=137`

Or you can add chainId in the request body

## eth\_supportedEntryPoints

Request

```json
{
  "chainId": 80001,
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_supportedEntryPoints",
  "params": []
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    // we only support this right now
    "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789"
  ]
}
```

## eth\_estimateUserOperationGas

Request

```json
{
  "method": "eth_estimateUserOperationGas",
  "params": [
    // partial user operation
    {
      "sender": "0x8fb859e944561678be40cdd2db16551396c0b074",
      "nonce": "0x0152",
      "initCode": "0x",
      "callData": "0x9e5d4c49000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000",
      "signature": "0x73c3ac716c487ca34bb858247b5ccf1dc354fbaabdd089af3b2ac8e78ba85a4959a2d76250325bd67c11771c31fccda87c33ceec17cc0de912690521bb95ffcb1b"
    },
    "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789"
  ],
  "id": 1695717515,
  "jsonrpc": "2.0",
  "chainId": 80001
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 43,
  "result": {
    "maxFeePerGas": "0x2718",
    "maxPriorityFeePerGas": "0x2718",
    "preVerificationGas": "0xb4ac",
    "verificationGas": "0x0123b5",
    "verificationGasLimit": "0x0123b5",
    "callGasLimit": "0x01c097"
}
```

## eth\_sendUserOperation

Request

```json
{
  "method": "eth_sendUserOperation",
  "params": [
    // user opearation
    {
      "sender": "0x8Fb859E944561678be40Cdd2dB16551396c0b074",
      "nonce": "0x0150",
      "initCode": "0x",
      "callData": "0x9e5d4c49000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000",
      "callGasLimit": "0xa13c",
      "verificationGasLimit": "0xe2d8",
      "maxFeePerGas": "0x7ca702cd",
      "maxPriorityFeePerGas": "0x7ca702b0",
      "paymasterAndData": "0x000031dd6d9d3a133e663660b959162870d755d4000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000416665636080709b968ebec098bf71fb0e79b3b480cc9ff809f192afb478c84ec50ad2de74b93a67860542099b11a1b5dbfa9bc21a2790c58e10015ce992a02f411b00000000000000000000000000000000000000000000000000000000000000",
      "preVerificationGas": "0x011120",
      "signature": "0x7cc0a2ae350b79c5b189bd36d55ab6a2756097d6d37537e3ec2c26daaa82c6d909fed87ff9d79a6fa127bd798126259ee72fa9395ecbeb1f70ed22ca35983aea1c"
    },
    // entrypoint contract address
    "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789"
  ],
  "id": 1695717470,
  "jsonrpc": "2.0",
  "chainId": 80001
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 1695717492,
  // user operation hash
  "result": "0xc876024995480fd19ddf0e72533267db7f3f20dcc1e54d27ffdd45902d954dfa"
}
```

## eth\_getUserOperationByHash

Request

```json
{
    "method": "eth_getUserOperationByHash",
    "params": [
      // user operation hash
      "0x1ee478a6e967c407e8dfb5e3f2eb1131a7418c36396147fce1f7e81a871102a3"
    ],
    "id": 1695717473,
    "jsonrpc": "2.0",
    "chainId": 80001
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 49,
  "result": {
    "userOperation": {
      "sender": "0x7038b737a81932f4fc4245A0B3453c91DE4f9FAB",
      "nonce": "0x18",
      "initCode": "0x",
      "callData": "0xb61d27f6000000000000000000000000c1c4aa02f5b65d1a885ec5cc03b39598047390eb00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000002440d097c30000000000000000000000007038b737a81932f4fc4245a0b3453c91de4f9fab00000000000000000000000000000000000000000000000000000000",
      "callGasLimit": "0x01c097",
      "verificationGasLimit": "0x0123b5",
      "preVerificationGas": "0xb4ac",
      "maxFeePerGas": "0x2718",
      "maxPriorityFeePerGas": "0x2718",
      "paymasterAndData": "0x52e9605391f43197b0ca1e6ae5f969a65091fe150000000000000000000000000000000000000000000000000000000065129d050000000000000000000000000000000000000000000000000000000000000000e74ded93d8e83f0b58df25fb7aeb21a63918595a936c2288b4424919b349b3265164bb43fa1c142b83970d8aa4a64b86be1a5a8b84d5bf929209b8587c1272c21b",
      "signature": "0xd8e4170b3f3782b88af7114d3db4b5b6813eafd74572d0683fa8eff7e065172243088d99f68be71b42a4228f6cb936a34acc457e02aa3ea0f5b8f43aca38207f1b",
      "verificationGas": "0x0123b5"
    },
    "entryPoint": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
    "transactionHash": "0x931a2363ac66b23ef09a54a130145bc6f61b656b21cfcad07c132cd0f6d4f029",
    "blockHash": "0x06a9dde92113aff80329e10993880733d471f1d5fa2e87fd0ac12ac16e0fe733",
    "blockNumber": "0x3c7dc6"
  }
}
```

## eth\_getUserOperationReceipt

Request

```json
{
  "method": "eth_getUserOperationReceipt",
  "params": [
    // user operation hash
    "0x1ee478a6e967c407e8dfb5e3f2eb1131a7418c36396147fce1f7e81a871102a3"
  ],
  "id": 1695717473,
  "jsonrpc": "2.0",
  "chainId": 80001
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 49,
  "result": {
    "userOpHash": "0xb2cc73236ea69e45de6fed082d426de4076bc8d904d3e3f2f3447db60f4eb45b",
    "sender": "0x7038b737a81932f4fc4245A0B3453c91DE4f9FAB",
    "nonce": "0x18",
    "actualGasCost": "0x53788d58",
    "actualGasUsed": "0x022299",
    "success": true,
    "logs": [
      {
        "address": "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789",
        "topics": [
          "0x49628fd1471006c1482da88028e9ce4dbb080b815c9b0344d39e5a8e6ec1419f",
          "0xb2cc73236ea69e45de6fed082d426de4076bc8d904d3e3f2f3447db60f4eb45b",
          "0x0000000000000000000000007038b737a81932f4fc4245a0b3453c91de4f9fab",
          "0x00000000000000000000000052e9605391f43197b0ca1e6ae5f969a65091fe15"
        ],
        "data": "0x000000000000000000000000000000000000000000000000000000000000001800000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000053788d580000000000000000000000000000000000000000000000000000000000022299",
        "blockNumber": "0x3c7dc6",
        "transactionHash": "0x931a2363ac66b23ef09a54a130145bc6f61b656b21cfcad07c132cd0f6d4f029",
        "transactionIndex": "0x4",
        "blockHash": "0x06a9dde92113aff80329e10993880733d471f1d5fa2e87fd0ac12ac16e0fe733",
        "logIndex": "0xb",
        "removed": false
      }
    ],
    "receipt": {
      "type": "0x2",
      "from": "0x9e375d31a8d0ed88d0ede9c7a3f775965d9442f2",
      "to": "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789",
      "status": "0x1",
      "cumulativeGasUsed": "0xd890b",
      "logsBloom": "0x000000000000000000002000000000000000000000000000000000000000000000080000000000000002000100000000001000000000000000000200000000000000000000000000000000080040000000000000000001010000000000000000000000000a0000000000000000010810000000000000200000000010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000100000000000000000000000000002040000000000000002000000000001000000020000000000000000000000000000000030000000000000000002000000000002000000000000000000000002000020000000",
      "logs": [
        {
          "address": "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789",
          "topics": [
            "0xbb47ee3e183a558b1a2ff0874b079f3fc5478b7454eacf2bfc5af2ff5878f972"
          ],
          "data": "0x",
          "blockNumber": "0x3c7dc6",
          "transactionHash": "0x931a2363ac66b23ef09a54a130145bc6f61b656b21cfcad07c132cd0f6d4f029",
          "transactionIndex": "0x4",
          "blockHash": "0x06a9dde92113aff80329e10993880733d471f1d5fa2e87fd0ac12ac16e0fe733",
          "logIndex": "0x9",
          "removed": false
        },
        {
          "address": "0xc1c4aa02f5b65d1a885ec5cc03b39598047390eb",
          "topics": [
            "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
            "0x0000000000000000000000000000000000000000000000000000000000000000",
            "0x0000000000000000000000007038b737a81932f4fc4245a0b3453c91de4f9fab",
            "0x0000000000000000000000000000000000000000000000000000000000013232"
          ],
          "data": "0x",
          "blockNumber": "0x3c7dc6",
          "transactionHash": "0x931a2363ac66b23ef09a54a130145bc6f61b656b21cfcad07c132cd0f6d4f029",
          "transactionIndex": "0x4",
          "blockHash": "0x06a9dde92113aff80329e10993880733d471f1d5fa2e87fd0ac12ac16e0fe733",
          "logIndex": "0xa",
          "removed": false
        },
        {
          "address": "0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789",
          "topics": [
            "0x49628fd1471006c1482da88028e9ce4dbb080b815c9b0344d39e5a8e6ec1419f",
            "0xb2cc73236ea69e45de6fed082d426de4076bc8d904d3e3f2f3447db60f4eb45b",
            "0x0000000000000000000000007038b737a81932f4fc4245a0b3453c91de4f9fab",
            "0x00000000000000000000000052e9605391f43197b0ca1e6ae5f969a65091fe15"
          ],
          "data": "0x000000000000000000000000000000000000000000000000000000000000001800000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000053788d580000000000000000000000000000000000000000000000000000000000022299",
          "blockNumber": "0x3c7dc6",
          "transactionHash": "0x931a2363ac66b23ef09a54a130145bc6f61b656b21cfcad07c132cd0f6d4f029",
          "transactionIndex": "0x4",
          "blockHash": "0x06a9dde92113aff80329e10993880733d471f1d5fa2e87fd0ac12ac16e0fe733",
          "logIndex": "0xb",
          "removed": false
        }
      ],
      "transactionHash": "0x931a2363ac66b23ef09a54a130145bc6f61b656b21cfcad07c132cd0f6d4f029",
      "contractAddress": null,
      "gasUsed": "0x22a80",
      "blockHash": "0x06a9dde92113aff80329e10993880733d471f1d5fa2e87fd0ac12ac16e0fe733",
      "blockNumber": "0x3c7dc6",
      "transactionIndex": "0x4",
      "effectiveGasPrice": "0x3e9",
      "l1GasPrice": "0xe66d67b9",
      "l1GasUsed": "0x2d28",
      "l1Fee": "0x28a53c53b9e8",
      "l1FeeScalar": "1"
    },
    "isPending": true
  }
}
```

## JSON-RPC Errors

| Error Code | Message                            |
| ---------- | ---------------------------------- |
| -32000     | System error                       |
| -32001     | Unsupported chainId                |
| -32002     | Arrays are not currently supported |
| -32003     | Not supported entry point          |
| -32004     | The UserOP is already processed    |
| -32005     | Estimate gas failed                |
| -32600     | Invalid Request                    |
| -32602     | Invalid params                     |
| -32603     | Not supported entry point          |
| -32604     | Send user operation failed         |
| -32605     | Validate user operation failed     |
| -32606     | Simulate user operation failed     |

## Demos

Live Demos

1. [https://scroll-testnet.particle.network/](https://scroll-testnet.particle.network/)
2. [https://opbnb-mainnet.particle.network/](https://opbnb-mainnet.particle.network/)
3. [https://combo-testnet.particle.network/](https://combo-testnet.particle.network/)

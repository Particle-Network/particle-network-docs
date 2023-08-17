# Account Abstraction RPC

{% content-ref url="../../account-abstraction/available-networks.md" %}
[available-networks.md](../../account-abstraction/available-networks.md)
{% endcontent-ref %}

## Before Start

Create Paymaster [https://dashboard.biconomy.io/](https://dashboard.biconomy.io/) & get dappApiKey

## How to use

1. Get User Smart Account Status: `particle_aa_getSmartAccount`
2. Call `particle_aa_getFeeQuotes` to obtain data for `verifyingPaymasterGasless`, `verifyingPaymasterNative`, and `tokenPaymaster`, corresponding to Gasless, Native Gas Payment, and ERC20 Token Gas Payment options:
   1. If `verifyingPaymasterGasless` is null, it means gasless is not available; if not null, it contains `userOp` and `userOpHash`.
   2. Inside `verifyingPaymasterNative`, there are `userOp`, `userOpHash`, and `feeQuote` (used for display and balance verification).
   3. `tokenPaymaster` contains `feeQuotes` and `tokenPaymasterAddress`.
3. If the user is eligible and chooses gasless, sign `userOpHash`, then add the signature field to `userOp`, and call `particle_aa_sendUserOp`.
4. If the user is eligible and chooses native gas payment, sign `userOpHash`, then add the signature field to `userOp`, and call `particle_aa_sendUserOp`.
5. If the user is eligible and chooses token gas payment, first call `particle_aa_createUserOp` to create a `userOp` and the corresponding `userOpHash`; then sign `userOpHash`, add the signature field to `userOp`, and call `particle_aa_sendUserOp`.
6. The `result` returned by `particle_sendUserOp` is the `txHash`.

## particle\_aa\_getSmartAccount

Request

```JSON
{
  "jsonrpc": "2.0",
  "id": "ee9cce2a-2f34-4c66-879e-c84c6f0e7f2d",
  "chainId": 80001,
  "method": "particle_aa_getSmartAccount",
  "params": [
    // owner address array
    [
      "0xA60123a1056e9D38B64c4993615F27cCe9A9E8D5",
      "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8"
    ]
  ]
}
```

Response

```JSON
{
  "jsonrpc": "2.0",
  "id": "ee9cce2a-2f34-4c66-879e-c84c6f0e7f2d",
  "result": [
    {
      "isDeployed": true,
      "chainId": 80001,
      "eoaAddress": "0xA60123a1056e9D38B64c4993615F27cCe9A9E8D5", // also owner
      "smartAccountAddress": "0x8Fb859E944561678be40Cdd2dB16551396c0b074",
      "fallBackHandlerAddress": "0xa04EeF9bBFd8F64d5218d4f3a3d03e8282810F51",
      "implementationAddress": "0x00006B7e42e01957dA540Dc6a8F7C30c4D816af5",
      "factoryAddress": "0x000000F9eE1842Bb72F6BBDD75E6D3d4e3e9594C",
      "owner": "0xA60123a1056e9D38B64c4993615F27cCe9A9E8D5",
      "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
      "index": 0,
      "version": "1.0.0"
    },
    {
      "isDeployed": false,
      "chainId": 80001,
      "eoaAddress": "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8",
      "factoryAddress": "0x000000F9eE1842Bb72F6BBDD75E6D3d4e3e9594C",
      "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
      "smartAccountAddress": "0x71f522803316C92731DDD007Fdb13281f52c1eef",
      "owner": "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8",
      "index": 0,
      "implementationAddress": "0x00006B7e42e01957dA540Dc6a8F7C30c4D816af5",
      "fallBackHandlerAddress": "0xa04EeF9bBFd8F64d5218d4f3a3d03e8282810F51",
      "version": "1.0.0",
      "createdAt": 1691460553368,
      "updatedAt": 1691460553368
    }
  ],
  "chainId": 80001
}
```

## particle\_aa\_getFeeQuotes

Request

```JSON
{
  "jsonrpc": "2.0",
  "id": "f216c7ea-3986-4784-954f-e54320c75b40",
  "chainId": 80001,
  "method": "particle_aa_getFeeQuotes",
  "params": [
    "LdF-gC43H.6f0ec763-fde5-4d5e-89f6-1b9ef12ba4a4", // dappApiKey
    "0xA60123a1056e9D38B64c4993615F27cCe9A9E8D5", // owner address
    // txs
    [
      {
        "to": "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8",
        "value": "0x1",
        "data": "0x"
      }
    ]
  ]
}
```

Response

```JSON
{
  "jsonrpc": "2.0",
  "id": "f216c7ea-3986-4784-954f-e54320c75b40",
  "result": {
    // maybe null
    "verifyingPaymasterGasless": {
      "userOp": {
        "sender": "0x8fb859e944561678be40cdd2db16551396c0b074",
        "nonce": "0x62",
        "initCode": "0x",
        "callData": "0x9e5d4c49000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000",
        "signature": "0x73c3ac716c487ca34bb858247b5ccf1dc354fbaabdd089af3b2ac8e78ba85a4959a2d76250325bd67c11771c31fccda87c33ceec17cc0de912690521bb95ffcb1b",
        "maxFeePerGas": "1500000034",
        "maxPriorityFeePerGas": "1500000000",
        "verificationGasLimit": "0x13ac6",
        "callGasLimit": "0x99a5",
        "preVerificationGas": "0xb678",
        "paymasterAndData": "0x000031dd6d9d3a133e663660b959162870d755d4000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce800000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000041de683496be0efcc84e2d3e4d0de6d2f4a5baae1999dfc9da0cbc23c6f8efd0572213f5d86955632257adafca88c6126a173136db2cb7f5dcca68e03947acb3771c00000000000000000000000000000000000000000000000000000000000000"
      },
      "userOpHash": "0x0300e5013b55034c9005f4faf2339e813f1bb9dfe06ef85c5ee483dca32ef891"
    },
    // won't be null
    "verifyingPaymasterNative": {
      "userOp": {
        "sender": "0x8fb859e944561678be40cdd2db16551396c0b074",
        "nonce": "0x62",
        "initCode": "0x",
        "callData": "0x9e5d4c49000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000",
        "signature": "0x73c3ac716c487ca34bb858247b5ccf1dc354fbaabdd089af3b2ac8e78ba85a4959a2d76250325bd67c11771c31fccda87c33ceec17cc0de912690521bb95ffcb1b",
        "maxFeePerGas": "1500000034",
        "maxPriorityFeePerGas": "1500000000",
        "verificationGasLimit": "0x13ac6",
        "callGasLimit": "0x99a5",
        "preVerificationGas": "0xb678",
        "paymasterAndData": "0x"
      },
      "userOpHash": "0x6093806a498a4fb0e07d3a81292fde32592ecc36ddf2346338ef9e5300230ac8",
      "feeQuote": {
        "tokenInfo": {
          "chainId": 80001,
          "address": "0x0000000000000000000000000000000000000000",
          "name": "MATIC",
          "symbol": "MATIC",
          "decimals": 18,
          "logoURI": "https://static.particle.network/token-list/polygon/native.png"
        },
        "fee": "249940505665318",
        "balance": "975499686355712581"
      }
    },
    "tokenPaymaster": {
      "tokenPaymasterAddress": "0x00000f7365cA6C59A2C93719ad53d567ed49c14C",
      "feeQuotes": [
        {
          "tokenInfo": {
            "chainId": 80001,
            "name": "WMATIC",
            "symbol": "WMATIC",
            "decimals": 18,
            "address": "0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889",
            "logoURI": "https://polygonscan.com/token/images/wMatic_32.png"
          },
          "fee": "374428266633331",
          "balance": "1019119852023946296",
          "premiumPercentage": "10"
        },
        {
          "tokenInfo": {
            "chainId": 80001,
            "name": "USDT",
            "symbol": "USDT",
            "decimals": 18,
            "address": "0xeaBc4b91d9375796AA4F69cC764A4aB509080A58",
            "logoURI": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/usdt.png"
          },
          "fee": "278847823565275",
          "balance": "0",
          "premiumPercentage": "20"
        },
        {
          "tokenInfo": {
            "chainId": 80001,
            "name": "USDC",
            "symbol": "USDC",
            "decimals": 6,
            "address": "0xdA5289fCAAF71d52a80A254da614a192b693e977",
            "logoURI": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/usdc.png"
          },
          "fee": "279",
          "balance": "0",
          "premiumPercentage": "20"
        },
        {
          "tokenInfo": {
            "chainId": 80001,
            "name": "DAI",
            "symbol": "DAI",
            "decimals": 18,
            "address": "0x27a44456bEDb94DbD59D0f0A14fE977c777fC5C3",
            "logoURI": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/dai.png"
          },
          "fee": "278594978301257",
          "balance": "0",
          "premiumPercentage": "20"
        },
        {
          "tokenInfo": {
            "chainId": 80001,
            "name": "SAND",
            "symbol": "SAND",
            "decimals": 18,
            "address": "0xE03489D4E90b22c59c5e23d45DFd59Fc0dB8a025",
            "logoURI": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/sand.png"
          },
          "fee": "683186002167101",
          "balance": "0",
          "premiumPercentage": "20"
        }
      ]
    }
  },
  "chainId": 80001
}
```

## particle\_aa\_createUserOp

Request

<pre class="language-JSON"><code class="lang-JSON">{
  "jsonrpc": "2.0",
  "id": "4259f7fe-87aa-44fd-89c8-fc1e41f6d3d8",
  "chainId": 80001,
  "method": "particle_aa_createUserOp",
  "params": [
    "LdF-gC43H.6f0ec763-fde5-4d5e-89f6-1b9ef12ba4a4", // dappApiKey
    "0xA60123a1056e9D38B64c4993615F27cCe9A9E8D5", // ownerAddress
    // txs
    [
      {
        "to": "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8",
        "value": "0x1",
        "data": "0x"
      }
    ],
<strong>    // optional: if not pass the following params, which will creates a gasless/user paid user op
</strong>    // token feeQuote
    {
      "tokenInfo": {
        "chainId": 80001,
        "name": "WMATIC",
        "symbol": "WMATIC",
        "decimals": 18,
        "address": "0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889",
        "logoURI": "https://polygonscan.com/token/images/wMatic_32.png"
      },
      "fee": "374428266633331",
      "balance": "1019119852023946296",
      "premiumPercentage": "10"
    },
    // token paymaster address
    "0x00000f7365cA6C59A2C93719ad53d567ed49c14C"
  ]
}
</code></pre>

Response

```JSON
{
  "jsonrpc": "2.0",
  "id": "4259f7fe-87aa-44fd-89c8-fc1e41f6d3d8",
  "result": {
    "userOp": {
      "sender": "0x8fb859e944561678be40cdd2db16551396c0b074",
      "nonce": "0x64",
      "initCode": "0x",
      "callData": "0x912ccaa3000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000012000000000000000000000000000000000000000000000000000000000000000020000000000000000000000009c3c9283d3e44854697cd22d3faa240cfb032889000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce80000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000044095ea7b300000000000000000000000000000f7365ca6c59a2c93719ad53d567ed49c14c0000000000000000000000000000000000000000000000000001548a5fd39873000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "signature": "0x73c3ac716c487ca34bb858247b5ccf1dc354fbaabdd089af3b2ac8e78ba85a4959a2d76250325bd67c11771c31fccda87c33ceec17cc0de912690521bb95ffcb1b",
      "verificationGasLimit": "0x163b8",
      "callGasLimit": "0xd1e7",
      "preVerificationGas": "50736",
      "paymasterAndData": "0x00000f7365ca6c59a2c93719ad53d567ed49c14c000000000000000000000000000000000000000000000000000000000064d2762e0000000000000000000000000000000000000000000000000000000064d26f260000000000000000000000009c3c9283d3e44854697cd22d3faa240cfb03288900000000000000000000000000000f7748595e46527413574a9327942e744e910000000000000000000000000000000000000000000000000de567836c0e69c0000000000000000000000000000000000000000000000000000000000010c8e09292fc583c0427b08765e4eccc030122182d141b6dbc5bb539fc4de726935042106faec9ee5b7537cf15c5f7d317fc355a0ff41877815fcbf9c906e0b584c4b61b",
      "maxFeePerGas": "1500000034",
      "maxPriorityFeePerGas": "1500000000"
    },
    "userOpHash": "0xc25a6b6a7003a173bfa61cf09a6774c221d8328e5b0c84c6e3bb12ecfd52e8b0"
  },
  "chainId": 80001
}
```

## particle\_aa\_sendUserOp

Request

```JSON
{
  "jsonrpc": "2.0",
  "id": "89916c65-2a47-4933-aa4c-55f7e29edbf0",
  "chainId": 80001,
  "method": "particle_aa_sendUserOp",
  "params": [
    "LdF-gC43H.6f0ec763-fde5-4d5e-89f6-1b9ef12ba4a4", // dappApiKey
    "0xA60123a1056e9D38B64c4993615F27cCe9A9E8D5", // owner address
    // user op
    {
      "sender": "0x8fb859e944561678be40cdd2db16551396c0b074",
      "nonce": "0x64",
      "initCode": "0x",
      "callData": "0x912ccaa3000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000012000000000000000000000000000000000000000000000000000000000000000020000000000000000000000009c3c9283d3e44854697cd22d3faa240cfb032889000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce80000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000044095ea7b300000000000000000000000000000f7365ca6c59a2c93719ad53d567ed49c14c0000000000000000000000000000000000000000000000000001548a5fd39873000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "signature": "0xf173083acfa6ddfdbbb84023ebdeb07ff2a38aa1a2e7f7b1b2eb27e180ff13987c14d72273b82f448b8e89066045aebb9e5423eea3c2314136cc9c75d7bb716f1b",
      "verificationGasLimit": "0x163b8",
      "callGasLimit": "0xd1e7",
      "preVerificationGas": "50736",
      "paymasterAndData": "0x00000f7365ca6c59a2c93719ad53d567ed49c14c000000000000000000000000000000000000000000000000000000000064d2762e0000000000000000000000000000000000000000000000000000000064d26f260000000000000000000000009c3c9283d3e44854697cd22d3faa240cfb03288900000000000000000000000000000f7748595e46527413574a9327942e744e910000000000000000000000000000000000000000000000000de567836c0e69c0000000000000000000000000000000000000000000000000000000000010c8e09292fc583c0427b08765e4eccc030122182d141b6dbc5bb539fc4de726935042106faec9ee5b7537cf15c5f7d317fc355a0ff41877815fcbf9c906e0b584c4b61b",
      "maxFeePerGas": "1500000034",
      "maxPriorityFeePerGas": "1500000000"
    }
  ]
}
```

Response

```JSON
{
  "jsonrpc": "2.0",
  "id": "f4702ec0-a0ad-4f20-82b1-302b28223c4c",
  "result": "0x9dae2ad934a6bc3f96ea6bf7fc3230f9c425f84ea99ae64330bdfb13c1b45362",
  "chainId": 80001
}

```

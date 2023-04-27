# Account Abstraction RPC

## How to use

1. Biconomy version: `1.0.0`, Docs: [https://docs.biconomy.io/](https://docs.biconomy.io/)
2. Get User Smart Account Status
3. User Paid Transactions
   1. Prepare Normal `tx/txs`
   2. Request `particle_biconomy_getFeeQuotes` & select one `feeQuote`
   3. Request `particle_biconomy_createUserPaidTransaction` with `tx/txs` and `feeQuote`, the RPC will return the wallet transaction and typed data to sign.
   4. Sign the typed data using `eth_signTypedData_v4` with any EOA signer you use (Particle MPC, MetaMask, Private Key, etc.)
   5. Request `particle_biconomy_sendUserPaidTransaction` and you will get a transaction hash
4. Gasless Transactions
   1. Create Paymaster [https://dashboard.biconomy.io/](https://dashboard.biconomy.io/) & get dappApiKey
   2. Prepare Normal `tx/txs`
   3. Request `particle_biconomy_createGaslessransaction` with `tx/txs` and `dappApiKey`, the RPC will return the user operation and user operation hash to sign.
   4. Sign the user operation hash using personal\_sign with any EOA signer you use (Particle MPC, MetaMask, Private Key, etc.)\
      `const signature = await eoaSigner.signMessage(arrayify(userOpHash));`
   5. Request `particle_biconomy_sendGaslessTransaction` and you will get a transaction hash

## particle\_biconomy\_getSmartAccount

*   Request (application/json)

    ```
        {
          "chainId": 1,
          "jsonrpc": "2.0",
          "id": "f196156e-15ea-4136-820f-afefa68663a7",
          "method": "particle_biconomy_getSmartAccount",
          "params": [
            "2.0",
            [
              // eoa address array
              "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
              "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8"
            ]
          ]
        }
    ```
*   Response 200

    ```
      {
        "jsonrpc": "2.0",
        "id": "f196156e-15ea-4136-820f-afefa68663a7",
        "result": [
          {
            "isDeployed": false,
            "chainId": 1,
            "eoaAddress": "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            "factoryAddress": "0x000000F9eE1842Bb72F6BBDD75E6D3d4e3e9594C",
            "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
            "smartAccountAddress": "0x4962F26080f3cDb3f81C21A51C2a2b4dFfd3539d",
            "owner": "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            "index": 0,
            "implementationAddress": "0x00006B7e42e01957dA540Dc6a8F7C30c4D816af5",
            "fallBackHandlerAddress": "0xa04EeF9bBFd8F64d5218d4f3a3d03e8282810F51",
            "version": "1.0.0",
            "createdAt": 1682329674566,
            "updatedAt": 1682329674566
          },
          {
            "isDeployed": false,
            "chainId": 1,
            "eoaAddress": "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8",
            "factoryAddress": "0x000000F9eE1842Bb72F6BBDD75E6D3d4e3e9594C",
            "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
            "smartAccountAddress": "0x71f522803316C92731DDD007Fdb13281f52c1eef",
            "owner": "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8",
            "index": 0,
            "implementationAddress": "0x00006B7e42e01957dA540Dc6a8F7C30c4D816af5",
            "fallBackHandlerAddress": "0xa04EeF9bBFd8F64d5218d4f3a3d03e8282810F51",
            "version": "1.0.0",
            "createdAt": 1682329672614,
            "updatedAt": 1682329672614
          }
        ],
        "chainId": 1
      }
    ```

## particle\_biconomy\_getFeeQuotes

*   Request (application/json)

    ```
        {
          "chainId": 1,
          "jsonrpc": "2.0",
          "id": "f196156e-15ea-4136-820f-afefa68663a7",
          "method": "particle_biconomy_getFeeQuotes",
          "params": [
            "1.0.0",
            // eoa address
            "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            // transactions
            [

              {
                  to: '0x072b87dc4c439ad75748ea73cb120e06ee000e8a',
                  value: 0,
                  data: '0x8d80ff0a0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000037d001111111254fb6c44bac0bed2854e76f90643097d000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003287c025200000000000000000000000000b97cd69145e5a9357b2acd6af6c5076380f17afb00000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000180000000000000000000000000b33eaad8d922b1083446dc23f610c2567fb5180f000000000000000000000000eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee000000000000000000000000b97cd69145e5a9357b2acd6af6c5076380f17afb0000000000000000000000004962f26080f3cdb3f81c21a51c2a2b4dffd3539d000000000000000000000000000000000000000000000000028361b82d780c400000000000000000000000000000000000000000000000000d5999a966c121e4000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000017300000000000000000000000000000000013500011f0000e300006800004e80206c4eca27b33eaad8d922b1083446dc23f610c2567fb5180f329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000000337875225b8610020d6bdbf78b33eaad8d922b1083446dc23f610c2567fb5180f0c20b33eaad8d922b1083446dc23f610c2567fb5180fc45092e7e73951c6668f6c46acfca9f2b1c69aef6ae4071118002dc6c0c45092e7e73951c6668f6c46acfca9f2b1c69aef0000000000000000000000000000000000000000000000000d5999a966c121e4b33eaad8d922b1083446dc23f610c2567fb5180f41010d500b1d8e8ef31e21c99d1db9a6444d3adf127000042e1a7d4d0000000000000000000000000000000000000000000000000000000000000000c0611111111254fb6c44bac0bed2854e76f90643097d000000000000000000000000000000000000000000000000028361b82d780c4000000000000000000000000000b49e18cb000000'
              }
            ]
          ]
        }
    ```
*   Response 200

    ```
      {
          "jsonrpc": "2.0",
          "id": "5d67161e-3e22-44a2-9bb3-c480b4713aee",
          "result": [
            {
              "symbol": "MATIC",
              "address": "0x0000000000000000000000000000000000000000",
              "decimal": 18,
              "logoUrl": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/matic.png",
              "tokenGasPrice": 1500000030,
              "offset": 1,
              "payment": 358378507167570,
              "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
              "tokenBalance": "350210624054464161"
            },
            {
              "symbol": "USDC",
              "address": "0xdA5289fCAAF71d52a80A254da614a192b693e977",
              "decimal": 6,
              "logoUrl": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/usdc.png",
              "tokenGasPrice": 1500,
              "offset": 1000000,
              "payment": 382,
              "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
              "tokenBalance": "0"
            },
            {
              "symbol": "USDT",
              "address": "0xeaBc4b91d9375796AA4F69cC764A4aB509080A58",
              "decimal": 18,
              "logoUrl": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/usdt.png",
              "tokenGasPrice": 1500000030000000,
              "offset": 1000000,
              "payment": 381891007637820,
              "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
              "tokenBalance": "0"
            },
            {
              "symbol": "DAI",
              "address": "0xf61cBcaC6C5E4F27543495890536B799D18f7178",
              "decimal": 18,
              "logoUrl": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/dai.png",
              "tokenGasPrice": 1500000030,
              "offset": 1,
              "payment": 381891007637820,
              "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
              "tokenBalance": "0"
            },
            {
              "symbol": "WETH",
              "address": "0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa",
              "decimal": 18,
              "logoUrl": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/eth.png",
              "tokenGasPrice": 804622,
              "offset": 1,
              "payment": 204851933468,
              "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
              "tokenBalance": "0"
            }
          ],
          "chainId": 80001
      }
    ```

## particle\_biconomy\_createUserPaidTransaction

*   Request (application/json)

    ```
        {
          "chainId": 1,
          "jsonrpc": "2.0",
          "id": "f196156e-15ea-4136-820f-afefa68663a7",
          "method": "particle_biconomy_createUserPaidTransaction",
          "params": [
            "1.0.0",
            // eoa address
            "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            // transactions
            [
              {
                  to: '0xb33EaAd8d922B1083446DC23f610c2567fB5180f',
                  value: 0,
                  data: '0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000028361b8281fc800'
              }
            ]
            // feeQuote
            {
                "symbol": "MATIC",
                "address": "0x0000000000000000000000000000000000000000",
                "decimal": 18,
                "logoUrl": "https://raw.githubusercontent.com/spothq/cryptocurrency-icons/master/128/color/matic.png",
                "tokenGasPrice": 449700549040,
                "offset": 1,
                "payment": 105216887159437840,
                "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e"
            }
          ]
        }
    ```
*   Response 200

    ```
      {
        "jsonrpc": "2.0",
        "id": "a1145926-f2ff-48a9-9e07-0430cb3645f7",
        "result": {
          "walletTransaction": {
            "to": "0xb33EaAd8d922B1083446DC23f610c2567fB5180f",
            "value": 0,
            "data": "0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000028361b8281fc800",
            "operation": 0,
            "targetTxGas": 54541,
            "baseGas": 10259,
            "gasPrice": "396269290755",
            "tokenGasPriceFactor": 1,
            "gasToken": "0x0000000000000000000000000000000000000000",
            "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
            "nonce": 25
          },
          "typedData": {
            "types": {
              "AccountTx": [
                { "type": "address", "name": "to" },
                { "type": "uint256", "name": "value" },
                { "type": "bytes", "name": "data" },
                { "type": "uint8", "name": "operation" },
                { "type": "uint256", "name": "targetTxGas" },
                { "type": "uint256", "name": "baseGas" },
                { "type": "uint256", "name": "gasPrice" },
                { "type": "uint256", "name": "tokenGasPriceFactor" },
                { "type": "address", "name": "gasToken" },
                { "type": "address", "name": "refundReceiver" },
                { "type": "uint256", "name": "nonce" }
              ],
              "EIP712Domain": [
                { "name": "chainId", "type": "uint256" },
                { "name": "verifyingContract", "type": "address" }
              ]
            },
            "domain": {
              "chainId": "137",
              "verifyingContract": "0x4962f26080f3cdb3f81c21a51c2a2b4dffd3539d"
            },
            "primaryType": "AccountTx",
            "message": {
              "to": "0xb33eaad8d922b1083446dc23f610c2567fb5180f",
              "value": "0",
              "data": "0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000028361b8281fc800",
              "operation": "0",
              "targetTxGas": "54541",
              "baseGas": "10259",
              "gasPrice": "396269290755",
              "tokenGasPriceFactor": "1",
              "gasToken": "0x0000000000000000000000000000000000000000",
              "refundReceiver": "0xfc614d949ccdc9ec2ffc2b17ea0824c5a3bec69e",
              "nonce": "25"
            }
          }
        },
        "chainId": 137
      }
    ```

## particle\_biconomy\_sendUserPaidTransaction

*   Request (application/json)

    ```
        {
          "chainId": 1,
          "jsonrpc": "2.0",
          "id": "f196156e-15ea-4136-820f-afefa68663a7",
          "method": "particle_biconomy_sendUserPaidTransaction",
          "params": [
            "1.0.0",
            // eoa address
            "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            // wallet transaction
            {
                "to": "0xb33EaAd8d922B1083446DC23f610c2567fB5180f",
                "value": 0,
                "data": "0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000028361b8281fc800",
                "operation": 0,
                "targetTxGas": 54541,
                "baseGas": 10259,
                "gasPrice": "396269290755",
                "tokenGasPriceFactor": 1,
                "gasToken": "0x0000000000000000000000000000000000000000",
                "refundReceiver": "0xfC614d949CcdC9EC2Ffc2b17eA0824c5A3beC69e",
                "nonce": 25
            },
            // signature
            "0xfb34f35974715e95218f703f4cc894ba90f796e5488a23ed0f4de4a869da8ff45a6d3946ee0bfd6ee6d31f2ad05ea4c1945e15536284e1a1ebca32e23ff055611b"
          ]
        }
    ```
*   Response 200

    ```
      {
        "jsonrpc": "2.0",
        "id": "a1145926-f2ff-48a9-9e07-0430cb3645f7",
        "result": "0xe3c9722c177f4d3125c75d8b44b9956e29c87c5447531f11b2915172f71f580d"
      }
    ```

## particle\_biconomy\_createGaslessTransaction

*   Request (application/json)

    ```
        {
          "chainId": 1,
          "jsonrpc": "2.0",
          "id": "f196156e-15ea-4136-820f-afefa68663a7",
          "method": "particle_biconomy_createGaslessTransaction",
          "params": [
            "1.0.0",
            // eoa address
            "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            // transactions
            [
              {
                  to: '0xb33EaAd8d922B1083446DC23f610c2567fB5180f',
                  value: 0,
                  data: '0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce8000000000000000000000000000000000000000000000000028361b8281fc800'
              }
            ],
            // dapp api key
            'LdF-gC43H.6f0ec763-fde5-4d5e-89f6-1b9ef12ba4a4'
          ]
        }
    ```
*   Response 200

    ```
      {
        "jsonrpc": "2.0",
        "id": "fb24c91a-f2b2-43b3-9e0e-70683e734651",
        "result": {
          "userOp": {
            "sender": "0x8Fb859E944561678be40Cdd2dB16551396c0b074",
            "nonce": "0x34",
            "initCode": "0x",
            "callData": "0x912ccaa3000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001200000000000000000000000000000000000000000000000000000000000000002000000000000000000000000b33eaad8d922b1083446dc23f610c2567fb5180f000000000000000000000000b33eaad8d922b1083446dc23f610c2567fb5180f00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "callGasLimit": "0xc261",
            "verificationGasLimit": "0x17b14",
            "maxFeePerGas": "0x59682f1e",
            "maxPriorityFeePerGas": "0x59682f00",
            "paymasterAndData": "0x",
            "preVerificationGas": "0xb254",
            "signature": ""
          },
          "userOpHash": "0xeb3b53b7932e3e41d9ac4ef6c937f091bf6bc22e42b76f6eb49c5908af9935a6"
        },
        "chainId": 80001
      }
    ```

## particle\_biconomy\_sendGaslessTransaction

*   Request (application/json)

    ```
        {
          "chainId": 1,
          "jsonrpc": "2.0",
          "id": "f196156e-15ea-4136-820f-afefa68663a7",
          "method": "particle_biconomy_sendGaslessTransaction",
          "params": [
            "1.0.0",
            // eoa address
            "0x631571040DD9fe3BF37BDcA6b2731dbe2Ae8bA9E",
            // user op with signature
            {
                "sender": "0x8Fb859E944561678be40Cdd2dB16551396c0b074",
                "nonce": "0x34",
                "initCode": "0x",
                "callData": "0x912ccaa3000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001200000000000000000000000000000000000000000000000000000000000000002000000000000000000000000b33eaad8d922b1083446dc23f610c2567fb5180f000000000000000000000000b33eaad8d922b1083446dc23f610c2567fb5180f00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
                "callGasLimit": "0xc261",
                "verificationGasLimit": "0x17b14",
                "maxFeePerGas": "0x59682f1e",
                "maxPriorityFeePerGas": "0x59682f00",
                "paymasterAndData": "0x",
                "preVerificationGas": "0xb254",
                "signature": "0xfb34f35974715e95218f703f4cc894ba90f796e5488a23ed0f4de4a869da8ff45a6d3946ee0bfd6ee6d31f2ad05ea4c1945e15536284e1a1ebca32e23ff055611b"
            },
            // dapp api key
            'LdF-gC43H.6f0ec763-fde5-4d5e-89f6-1b9ef12ba4a4'
          ]
        }
    ```
*   Response 200

    ```
      {
        "jsonrpc": "2.0",
        "id": "796667c5-7017-4145-8857-1bdcf76a5dbf",
        "result": "0x765ca11039bfab237553c11a5d1c2c86bd630ef72c3090b15fa6f033a9b1fbad",
        "chainId": 80001
      }
    ```

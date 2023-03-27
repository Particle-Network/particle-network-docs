---
description: Simple And Powerful
---

# Enhanced RPC

### 🔥 particle\_getPrice

> Get the real-time exchange rate of the token

**Parameters:**

* `<[string]>` - address array

{% hint style="info" %}
The native token has no contract address, so it's value is **`native`**
{% endhint %}

* `<[string]>` - Array of supported currencies, the values of currencies is as follows:
  * `usd`
  * `cny`

**Results:**

* `<[object]>` - a JSON object array containing:
  * `address: <string>`, token contract address
  * `currencies: <[object]>`, currency info object

Request example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 42,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getPrice',
        params: [
            [
                'native', 
                '0x7968bc6a03017eA2de509AAA816F163Db0f35148',
            ],
            [
                'usd', 
                'cny',
            ],
        ],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        },
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic YmEwNTA5ZTctZThiYi00MzY2LTg5YjctYjM5ZjAyYmNkMDg0OmNnZjE4YXNMbG9zSkJzZlZXbWxvNHNuZ2lFRVZzc1gzNHFlTUxmZzQ=' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":42,"jsonrpc":"2.0","id":1,"method":"particle_getPrice","params":[["native","0x7968bc6a03017eA2de509AAA816F163Db0f35148"],["usd","cny"]]}
'
```
{% endtab %}
{% endtabs %}

Response example:

```typescript
{
    "chainId": 42,
    "jsonrpc": "2.0", 
    "id": 1, 
    "result": [
        {
            "address": "native", 
            "currencies": [
                {
                    "type": "cny", 
                    "price": 12170.15, 
                    "marketCap": 1471838560900.835, 
                    "24hChange": -6.152514002972175, 
                    "24hVol": 134476255636.24605, 
                    "lastUpdatedAt": 1654153126
                }, 
                {
                    "type": "usd", 
                    "price": 1822.72, 
                    "marketCap": 220186784486.6231, 
                    "24hChange": -5.940275233699975, 
                    "24hVol": 20140522643.179638, 
                    "lastUpdatedAt": 1654153126
                }
            ]
        },
        {
            "address": "0x7968bc6a03017eA2de509AAA816F163Db0f35148", 
            "currencies": [
                {
                    "type": "cny", 
                    "price": 5.68, 
                    "marketCap": 9983229.811334804, 
                    "24hChange": -3.653366801443764, 
                    "24hVol": 1225668.578963783, 
                    "lastUpdatedAt": 1654173523
                }, 
                {
                    "type": "usd", 
                    "price": 0.85174, 
                    "marketCap": 1496287.4417468207, 
                    "24hChange": -3.6244889459436456, 
                    "24hVol": 183684.05277680393, 
                    "lastUpdatedAt": 1654173523
                }
            ]
        }
    ]
}
```

### 🔥 particle\_suggestedGasFees

> Get suggested gas fees for EIP-1559

**Parameters:**

* no parameters

**Results:**

* `<object>` - a JSON object containing:
  * `low: <object>`, suggested gas fees info
  * `medium: <object>`, suggested gas fees info
  * `fast: <object>`, suggested gas fees info
  * `baseFee: <string>`, current base fee

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 42,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_suggestedGasFees',
        params: [],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":42,"jsonrpc":"2.0","id":1,"method":"particle_suggestedGasFees","params":[]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 42,
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "low": {
            "maxPriorityFeePerGas": "1",
            "maxFeePerGas": "20.261906202",
            "minWaitTime": 15000,
            "maxWaitTime": 30000
        },
        "medium": {
            "maxPriorityFeePerGas": "1.5",
            "maxFeePerGas": "27.503573373",
            "minWaitTime": 15000,
            "maxWaitTime": 45000
        },
        "high": {
            "maxPriorityFeePerGas": "2",
            "maxFeePerGas": "34.745240544",                
            "minWaitTime": 15000,
            "maxWaitTime": 60000
        },
        "baseFee": "19.261906202"
    }
}
```

### 🔥 particle\_getTransactionsByAddress

> Get parsed transaction history by giving an address

**Parameters:**

* `<string>` - account address

**Results:**

* `<[object]>` - a JSON object containing:
  * `hash: <string>`, transaction hash
  * `from: <string>`, transaction send from address
  * `to: <string>`, transaction send to address
  * `value: <string>`, transaction send value
  * `data: <string>`, transaction send data
  * `gasLimit: <int64>`, transaction gas limit
  * `timestamp: <int64>`, transaction time
  * `status: <int64>`, transaction status:, the value is as below:
    * `0`, pending transaction in `TX-QUEUE`, can cancel
    * `1`, pending transaction in `TX-POOL`, can speed up or cancel
    * `3`, confirmed transaction which status is `Success`
    * `4`, confirmed transaction which status is `Fail`

when status is `0|1`, the object containing:

* `type: <int64>`, transaction type, the value is as below:
  * `0:` legacy transaction
  * `1:` EIP-2930 access list transaction
  * `2:` EIP-1559 gas fee market transaction
* `nonce: <int64>`, trancsaction nonce
* `accessList: <[]>`, EIP-2930 access list
* `maxPriorityFeePerGas: <string>`, EIP-1559 max priority fee per gas
* `maxFeePerGas: <string>`, EIP-1559 max fee per gas

when status is `3|4`, the object containing:

* `gasSpent: <string>`, transaction actual used gas
* `gasPrice: <string>`, transaction gas price
* `fees: <string>`, transaction gas fees

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 42,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_getTransactionsByAddress',
        params: ['0x425249Cf0F2f91f488E24cF7B1AA3186748f7516'],
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":42,"jsonrpc":"2.0","id":1,"method":"particle_getTransactionsByAddress","params":["0x425249Cf0F2f91f488E24cF7B1AA3186748f7516"]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 42,
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "chainId": "0x2a",
            "nonce": "0x9",
            "maxPriorityFeePerGas": "0x8c347c90",
            "maxFeePerGas": "0xb60535d5740",
            "gasLimit": "0x5208",
            "to": "0x6D5fCEd0C74F22a1B145ef48B25527Ce9BF829bF",
            "value": "0x16345785d8a0000",
            "data": "0x",
            "accessList": [ ],
            "hash": "0x301657f8691004172b756e198a680f1e38065899b74f41175dde206071c17008",
            "from": "0x425249Cf0F2f91f488E24cF7B1AA3186748f7516",
            "type": 2,
            "status": 1,
            "timestamp": 1653593819
        },
        {
            "chainId": "0x2a",
            "hash": "0xbe18f8fd8c0694af8fa3dbd29b53f20981269a7c08f0c0e9ae888aa863ef74c7",
            "from": "0x425249Cf0F2f91f488E24cF7B1AA3186748f7516",
            "to": "0x5F5D96fac4cca23196531A4Fa221bC309C16544E",
            "value": "0x0",
            "gasLimit": "0x37775",
            "gasSpent": "0x33600",
            "gasPrice": "0x77359400",
            "fees": "0x17ec609380000",
            "status": 3,
            "timestamp": 1630521436
        }
    ]
}
```

### 🔥 particle\_abi\_encodeFunctionCall

> Call ABI function

**Parameters:**

* `<string>` - contract address
* `<string>` - function name, the value is as below:
  * `erc20_transfer`
  * `erc20_approve`
  * `erc20_transferFrom`
  * `erc721_safeTransferFrom`
  * `erc1155_safeTransferFrom`
  * `custom_{methodName}` - any other functions which not mentioned above
* `<array[any]>` - function params
* `<string>: optional` - function abi json string

**Results:**

* `<string>` - function call result string

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: "particle_abi_encodeFunctionCall",
        params: [
            "0xB8c77482e45F1F44dE1745F52C74426C631bDD52", 
            "erc20_transfer", 
            [
                "0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8", 
                "1000000000000000000"
            ]
        ]
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":1,"jsonrpc":"2.0","id":1,"method":"particle_abi_encodeFunctionCall","params":["0xB8c77482e45F1F44dE1745F52C74426C631bDD52","erc20_transfer",["0x329a7f8b91Ce7479035cb1B5D62AB41845830Ce8","1000000000000000000"]]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 1,
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0xa9059cbb000000000000000000000000329a7f8b91ce7479035cb1b5d62ab41845830ce80000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

### 🔥 **particle\_abi\_getContractAbi**

> Get verified contract abi

**Parameters:**

* `<string>` - contract address
* `<boolen>` - if the contract is a proxy contract, whether to parse the final contract abi

**Results:**

* `<string>` - an abi string(array object); if abi not verified, null returned

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 56,
        jsonrpc: '2.0',
        id: 1,
        method: "particle_abi_getContractAbi",
        params: [
            "0x2170Ed0880ac9A755fd29B2688956BD959F933F8", 
            true,
        ]
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response.data);
})();
```
{% endtab %}
{% endtabs %}

Response Example:

```json
{
  "jsonrpc": "2.0",
  "id": "3a66b102-bc5c-4437-8c22-2133a9a4008f",
  "result": "[{\"inputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"constructor\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"owner\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"address\",\"name\":\"spender\",\"type\":\"address\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"Approval\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"previousOwner\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"address\",\"name\":\"newOwner\",\"type\":\"address\"}],\"name\":\"OwnershipTransferred\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"from\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"address\",\"name\":\"to\",\"type\":\"address\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"Transfer\",\"type\":\"event\"},{\"constant\":true,\"inputs\":[],\"name\":\"_decimals\",\"outputs\":[{\"internalType\":\"uint8\",\"name\":\"\",\"type\":\"uint8\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"_name\",\"outputs\":[{\"internalType\":\"string\",\"name\":\"\",\"type\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"_symbol\",\"outputs\":[{\"internalType\":\"string\",\"name\":\"\",\"type\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"owner\",\"type\":\"address\"},{\"internalType\":\"address\",\"name\":\"spender\",\"type\":\"address\"}],\"name\":\"allowance\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"spender\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"approve\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"account\",\"type\":\"address\"}],\"name\":\"balanceOf\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"burn\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"decimals\",\"outputs\":[{\"internalType\":\"uint8\",\"name\":\"\",\"type\":\"uint8\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"spender\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"subtractedValue\",\"type\":\"uint256\"}],\"name\":\"decreaseAllowance\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"getOwner\",\"outputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"spender\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"addedValue\",\"type\":\"uint256\"}],\"name\":\"increaseAllowance\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"mint\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"name\",\"outputs\":[{\"internalType\":\"string\",\"name\":\"\",\"type\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"owner\",\"outputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[],\"name\":\"renounceOwnership\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"symbol\",\"outputs\":[{\"internalType\":\"string\",\"name\":\"\",\"type\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"totalSupply\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"recipient\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"transfer\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"sender\",\"type\":\"address\"},{\"internalType\":\"address\",\"name\":\"recipient\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"transferFrom\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"newOwner\",\"type\":\"address\"}],\"name\":\"transferOwnership\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]",
  "chainId": 56
}
```

### 🔥 **particle\_deserializeTransaction**

> Get transaction deserialized data with balance change

**Parameters:**

* `<string>` - account address
* `<string>` - serialized transaction string

**Results:**

* `<[object]>` - a JSON object containing:
  * `estimatedChanges: <object>`
    * `natives: <[object]>` - estimated native token changes
      * `address: <string>` - transaction from address
      * `nativeChange: <string>` amount changes
    * `nfts: <[object]>` - estimated NFT changes
      * `address: <string>` - contract address
      * `name: <string>`
      * `symbol: <string>`
      * `image: <string>`
      * `fromAddress: <string>` - transaction from address
      * `tokenId: <string>` - NFT token id
      * `amountChange: <string>`
    * `tokens: <[object]>` - estimated token changes
      * `address: <string>` - contraction address
      * `name: <string>` - token name
      * `symbol: <string>` - token symbol
      * `image: <string>` - token image
      * `fromAddress: <string>` - transaction from address
      * `decimals: <int64>` token decimals
      * `amountChange: <string>`
  * `data: <object>` - deserialized transaction data
    * `from: <string>`
    * `chainId: <string>`
    * `nonce: <string>`
    * `maxPriorityFeePerGas: <string>`
    * `maxFeePerGas: <string>`
    * `gasLimit: <string>`
    * `to: <string>`
    * `value: <string>`
    * `data: <string>`
    * `accessList: <[object]>`
    * `v: <string>`
    * `r: <string>`
    * `s: <string>`
    * `type: <string>` - transaction type, the value is as below:
      * `0x0:` legacy transaction
      * `0x1:` EIP-2930 access list transaction
      * `0x2:` EIP-1559 gas fee market transaction
    * `function: <object>`
      * `name: <string>`
      * `params: <[object]>`
        * `name: <string>`
        * `value: <string>`
        * `type: <string>`

Request Example:

{% tabs %}
{% tab title="Javascript" %}
```typescript
const axios = require('axios');

(async () => {
    const response = await axios.post('https://rpc.particle.network/evm-chain', {
        chainId: 1,
        jsonrpc: '2.0',
        id: 1,
        method: 'particle_deserializeTransaction',
        params: [
            '0x03afc65278e6d37f23bc0b8bf4c9d61bd35edfc8',
            '0x02f8d30182030a843b9aca00850608379d4b8301122e9499ecdf17ded4fcb6c5f0fe280d21f832af464f6780b86442842e0e00000000000000000000000003afc65278e6d37f23bc0b8bf4c9d61bd35edfc8000000000000000000000000a058fb195d274afbae4dc317be362d4e96ffa1b400000000000000000000000000000000000000000000000000000000000006c1c001a0f7e0c908e4b549d24dad47f28205d3437e00b123e9b57bec95dcd61e1fd8065ca053495aaeec3740c0520e7717060a208aac0970dbc30ef3380e3ae9f81f4f6f66',
        ]
    }, {
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        }
    });

    console.log(response.data);
})();
```
{% endtab %}

{% tab title="Curl" %}
```powershell
curl 'https://rpc.particle.network/evm-chain' \
--header 'Authorization: Basic { Auth String }' \
-X POST -H "Content-Type: application/json" -d '
    {"chainId":1,"jsonrpc":"2.0","id":1,"method":"particle_deserializeTransaction","params":["0x03afc65278e6d37f23bc0b8bf4c9d61bd35edfc8","0x02f8d30182030a843b9aca00850608379d4b8301122e9499ecdf17ded4fcb6c5f0fe280d21f832af464f6780b86442842e0e00000000000000000000000003afc65278e6d37f23bc0b8bf4c9d61bd35edfc8000000000000000000000000a058fb195d274afbae4dc317be362d4e96ffa1b400000000000000000000000000000000000000000000000000000000000006c1c001a0f7e0c908e4b549d24dad47f28205d3437e00b123e9b57bec95dcd61e1fd8065ca053495aaeec3740c0520e7717060a208aac0970dbc30ef3380e3ae9f81f4f6f66"]}
'
```
{% endtab %}
{% endtabs %}

Response Example:

```typescript
{
    "chainId": 1,
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "estimatedChanges": {
            "natives": [{
                "address": "0x03afC65278E6D37f23bC0B8Bf4C9d61Bd35edFC8",
                "nativeChange": "0"
            }],
            "nfts": [{
                "address": "0x99EcdF17DED4fCb6C5f0fE280d21f832af464f67",
                "name": "",
                "symbol": "",
                "image": "",
                "fromAddress": "0x03afC65278E6D37f23bC0B8Bf4C9d61Bd35edFC8",
                "tokenId": "1729",
                "amountChange": "-1"
            }],
            "tokens": []
        },
        "data": {
            "from": "0x03afC65278E6D37f23bC0B8Bf4C9d61Bd35edFC8",
            "chainId": "0x1",
            "nonce": "0x30a",
            "maxPriorityFeePerGas": "0x3b9aca00",
            "maxFeePerGas": "0x608379d4b",
            "gasLimit": "0x1122e",
            "to": "0x99EcdF17DED4fCb6C5f0fE280d21f832af464f67",
            "value": "0x0",
            "data": "0x42842e0e00000000000000000000000003afc65278e6d37f23bc0b8bf4c9d61bd35edfc8000000000000000000000000a058fb195d274afbae4dc317be362d4e96ffa1b400000000000000000000000000000000000000000000000000000000000006c1",
            "accessList": [],
            "v": "0x1",
            "r": "0xf7e0c908e4b549d24dad47f28205d3437e00b123e9b57bec95dcd61e1fd8065c",
            "s": "0x53495aaeec3740c0520e7717060a208aac0970dbc30ef3380e3ae9f81f4f6f66",
            "type": "0x2",
            "function": {
                "name": "safeTransferFrom",
                "params": [{
                    "name": "from_",
                    "value": "0x03afc65278e6d37f23bc0b8bf4c9d61bd35edfc8",
                    "type": "address"
                }, {
                    "name": "to_",
                    "value": "0xa058fb195d274afbae4dc317be362d4e96ffa1b4",
                    "type": "address"
                }, {
                    "name": "tokenId_",
                    "value": "1729",
                    "type": "uint256"
                }]
            }
        }
    }
}
```

### 🔥 **particle\_getLatestBlock**

> Get latest block with transaction receipts.

**Parameters:**

Empty array

**Results:**

* `result of eth_getBlockByNumber`
* `& receipts`

Response example:

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "difficulty": "0x2",
    "extraData": "0xd88301010b846765746888676f312e31382e32856c696e7578000000de3b3a04d9038da607050a588616d8a1c39cb6b4190645ff6ef81ce765aeef20547cd82131367c734b58237c96bd63032b834a9467766b27c59fe1e56f3de2c53a9f4cae01",
    "gasLimit": "0x1c9c380",
    "gasUsed": "0x911f5",
    "hash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
    "logsBloom": "0x00000000008100801000000020000000000008000000002000000010000000000010200000000000000082000021000010000000002000000000000000200400000000100400000100000008082100002010000000000002000000000200200000100020020200200010000042220820080180000000000000000010000000000000020000010000000002000004000000000400000000200000000000200020020001000000000000000000000000000000000200000000020000000000000000080102000000000000080000002100001100000000000000000003000020010011000000000029010000000008000100000000008000000800000000000002",
    "miner": "0x96c5d20b2a975c050e4220be276ace4892f4b41a",
    "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "nonce": "0x0000000000000000",
    "number": "0x14914c4",
    "parentHash": "0x6d4809bcfe42ffba2d77e803ae77d9ecfe4819784bf78162e4cb9f92502e24cb",
    "receiptsRoot": "0x6a64e02e8d34f7f6d17a75bb43d77212929fd6a576f9da4435918922b88fb919",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x925",
    "stateRoot": "0xdab0f8bac71c5f6fb76c447d05d73e7f488ef1d038d29f8b6a4a5aefb5ed5010",
    "timestamp": "0x62e79753",
    "totalDifficulty": "0x28fd1d6",
    "transactions": [
      "0x4911bb832a6ad5f7d7fee77344427935478c01ab0f67981346264bdaa3bdb723"
    ],
    "transactionsRoot": "0x8e081b66278e186d0099e0f778543d991abb759cfb61e82e11c91653f4dfcce0",
    "uncles": [],
    "receipts": [
      {
        "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
        "blockNumber": "0x14914c4",
        "contractAddress": null,
        "cumulativeGasUsed": "0x911f5",
        "from": "0x96c5d20b2a975c050e4220be276ace4892f4b41a",
        "gasUsed": "0x6d45",
        "logs": [
          {
            "address": "0x0000000000000000000000000000000000001000",
            "topics": [
              "0x627059660ea01c4733a328effb2294d2f86905bf806da763a89cee254de8bee5"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000002145ef39c2800",
            "blockNumber": "0x14914c4",
            "transactionHash": "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800",
            "transactionIndex": "0xa",
            "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
            "logIndex": "0xa",
            "removed": false
          },
          {
            "address": "0x0000000000000000000000000000000000001000",
            "topics": [
              "0x93a090ecc682c002995fad3c85b30c5651d7fd29b0be5da9d784a3302aedc055",
              "0x00000000000000000000000096c5d20b2a975c050e4220be276ace4892f4b41a"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000012b756907d6800",
            "blockNumber": "0x14914c4",
            "transactionHash": "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800",
            "transactionIndex": "0xa",
            "blockHash": "0x64f3d1897dd3fd69126298c1e93fccb1be29d6b04f3a5c269037485ab592f3ff",
            "logIndex": "0xb",
            "removed": false
          }
        ],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000002010000000000000000000000000000000000020000200000000000000020000080000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000002000000000000000000000000010000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0x0000000000000000000000000000000000001000",
        "transactionHash": "0x97a53ba170994b891c0b442038c761e9156e1656715f6395772d988fce348800",
        "transactionIndex": "0xa",
        "type": "0x0"
      }
    ]
  },
  "chainId": 97
}
```

### [Tokens & NFTs](tokens-and-nfts.md)

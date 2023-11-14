---
description: Gasless for multi chains
---

# Paymaster RPC

{% hint style="info" %}
Endpoint：**https://paymaster.particle.network**
{% endhint %}

We receive a request in JSON RPC 2.0 format

When calling RPC, you need to pass your project information in the query

* chainId - which chain you use
* projectUuid - your project uuid on Particle
* projectKey - your project client key or project server key

```typescript
https://paymaster.particle.network?chainId=${chainId}&projectUuid=${projectUuid}&projectKey=${projectKey}
```

### pm\_sponsorUserOperation

Obtain Paymaster's signature to sponsor GasFee

* Body
  * method - pm\_sponsorUserOperation
  * params
    * userOp
    * entryPoint

Example Request

```typescript
import Axios from "axios";

const chainId = 11155111;
const projectUuid = "Your project uuid";
const projectKey = "Your project client key or server key";
const entryPoint = "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789";
const userOp = {
    sender: "0xfE71795B01d2c36FD5a001fe9D47EAec0da77e59",
    nonce: "0x00",
    initCode:
        "0x9406cc6185a346906296840746125a0e449764545fbfb9cf0000000000000000000000009a8c05c7ac9acecc1185d5a624eb185e63dde9c20000000000000000000000000000000000000000000000000000000000000000",
    callData:
        "0xb61d27f6000000000000000000000000aae0de40f94469761b797920a46f223d0fffd013000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000",
    maxFeePerGas: "0x5e18a0d2",
    maxPriorityFeePerGas: "0x5de29812",
    paymasterAndData: "0x",
    signature: "0x",
    preVerificationGas: "0xc954",
    verificationGasLimit: "0x066c12",
    callGasLimit: "0xf2a0",
};

const paymasterUrl = "https://paymaster.particle.network";
(async () => {
    const response = await Axios.post(
        paymasterUrl,
        {
            method: "pm_sponsorUserOperation",
            params: [userOp, entryPoint],
        },
        {
            params: {
                chainId,
                projectUuid,
                projectKey,
            },
        }
    );

    console.log(response.data);
})();
```

Example Response

```json
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "paymasterAndData": "0xc89b723809598d0ebf821e89087dc8e1a6ee04990000000000000000000000000000000000000000000000000000000065520e4300000000000000000000000000000000000000000000000000000000000000002b065ddfec26c0771030d57fb7693548a5a564d5f14a5d91f28bc8512c64d23048727b4a6f91d8e8286bb989b87828f9be87b0d665ea63d6b3f66b068de7bf851c"
    }
}
```

### pm\_paymasterBalance

Get your project's balance (USD)

{% hint style="info" %}
The decimals of the USD balance is **6**
{% endhint %}

* Body
  * method - pm\_paymasterBalance

Example Request

```typescript
import Axios from "axios";

const projectUuid = "Your project uuid";
const projectKey = "Your project client key or server key";

const paymasterUrl = "https://paymaster.particle.network";
(async () => {
    const response = await Axios.post(
        paymasterUrl,
        {
            method: "pm_paymasterBalance",
            params: [],
        },
        {
            params: {
                projectUuid,
                projectKey,
            },
        }
    );

    console.log(response.data);
})();
```

Example Response

```json
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "balance": "0x5ae4ea" // $5.956842
    }
}
```

### JSON-RPC Errors

<table><thead><tr><th>Error Code</th><th>Error Message</th><th data-hidden>Error Message</th><th data-hidden></th></tr></thead><tbody><tr><td>-32000</td><td>System error</td><td></td><td></td></tr><tr><td>-32001</td><td>Unsupported chainId</td><td></td><td></td></tr><tr><td>-32002</td><td>Arrays are not currently supported</td><td></td><td></td></tr><tr><td>-32003</td><td>The sign request is rejected</td><td></td><td></td></tr><tr><td>-32004</td><td>Can not fetch the usd price</td><td></td><td></td></tr><tr><td>-32005</td><td>Insufficient deposit for the project</td><td></td><td></td></tr><tr><td>-32006</td><td>Method not found</td><td></td><td></td></tr><tr><td>-32600</td><td>Invalid Request</td><td></td><td></td></tr><tr><td>-32602</td><td>Invalid params</td><td></td><td></td></tr><tr><td>-32603</td><td>Not supported entry point</td><td></td><td></td></tr><tr><td>-32606</td><td>Simulate user operation failed</td><td></td><td></td></tr></tbody></table>
